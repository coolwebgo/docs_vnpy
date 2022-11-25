---
title: backtesting
category: vnpy_ctastrategy
order: 1
---

> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)

---

## 简介
"vnpy_ctastrategy/backtesting.py"文件中专门为回测编写了`BacktestingEngine`, 了解回测逻辑能够使我们对策略的编写有更深刻的认知

## 流程解析
回顾一下backtesting_demo中的回测流程
```python
# 1. 创建回测引擎对象
engine = BacktestingEngine()
# 2. 设置回测参数
engine.set_parameters(
    vt_symbol="ru2301.SHFE",
    interval=Interval.MINUTE,
    start=datetime(2022, 11, 1),
    end=datetime(2022, 11, 22),
    rate=0.3/10000,
    slippage=10,
    size=10,
    pricetick=10,
    capital=1_000_000,
)
# 3. 添加策略
engine.add_strategy(NewStrategy, {})
# 4. 加载数据
engine.load_data()
# 5. 运行回测
engine.run_backtesting()
# 6. 统计结果并绘图
df = engine.calculate_result()
engine.calculate_statistics()
engine.show_chart()
```
#### 参数设置
1. vt_symbol是"合约.交易所"全称, 至于合约名称大小写, 合约月份3位/4位数, 需要根据交易标准来定(因为这里是回测, 从数据库中拿数据, 因此这个名称可以是数据库中自己定义的名称)
2. rate: 手续费率, 因为只有一种设置方式, 因此不管品种是固定/浮动手续费, 单/双边手续费或是平今/平历史手续费统统都只能近似转化为比率形式
3. slippage: 滑点, 不同期货品种流动性不一样, 一般都设1跳(1个最小变动价位), 但是不同品种最小变动价位不一样, 因此这个滑点要根据回测品种仔细填写
4. size: 合约乘数, 根据交易所设计的品种标准合约填写, 不同品种不一样
5. pricetick: 最小变动价位, 根据交易所设计的品种标准合约填写, 不同品种不一样
6. capital: 初始资金, 默认100w
7. mode: 回测模式, 这里默认是Bar回测所以没写, 如果是Tick级别的策略, 需要指明回测模式`mode = BacktestingMode.TICK`

#### 添加策略
`engine.add_strategy`第一个参数传递的是策略类名, 如果策略需要从外部传入参数, 可以使用字典形式从第二个参数中传入. 这里没有需要从外部传入的参数, 因此给了空字典, 但是对参数优化而言, 第二个参数很重要

#### 加载数据
如果是bar回测, 就通过`load_bar_data`加载数据库中的Bar数据, 如果是Tick回测, 就通过`load_tick_data`加载数据库中的Tick数据. 数据加载完成后再通过回放数据, 完成回测.

#### 运行回测
1. 数据回放开始前执行`self.strategy.on_init()`, 这里其实就是回调了策略中的`on_init`函数
2. 初始化完成后首先执行了`self.strategy.on_start()`, 这里其实就是回调了策略中的`on_start`函数
3. 在数据回放过程中, 如果是Bar回测, 就执行`self.new_bar`, 如果是Tick回测就执行`self.new_tick`, 我们以Bar回测为例, 它主要完成了以下几步
```python
def new_bar(self, bar: BarData) -> None:
    """"""
    self.bar = bar
    # 1. 更新数据时间
    self.datetime = bar.datetime
    # 2. 是否有限价单成交
    self.cross_limit_order()
    # 3. 是否有停止单成交
    self.cross_stop_order()
    # 4. 回调策略的on_bar
    self.strategy.on_bar(bar)
    # 5. 更新日收盘价
    self.update_daily_close(bar.close_price)
```
- 以`cross_limit_order`为例, 如果有发单就回调`on_order`, 然后模拟订单撮合, 如果订单撮合成功, 再回调`on_trade`, 同时保存`TradeData`, 方便结束后统计收益.
- 数据回放完毕, 所有成交单信息都保存在`self.trades`中

#### 收益统计
这里是按照期货的"逐日盯市"制度统计收益的, 因此我们在回测结果中看到的每日盈亏都是按照当天最后一个K线收盘价计算的盈亏