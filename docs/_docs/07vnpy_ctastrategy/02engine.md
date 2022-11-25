---
title: engine
category: vnpy_ctastrategy
order: 2
---

> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)

---

## 简介
CtaEngine是用来对接实盘交易的引擎接口, 其逻辑显然要比回测引擎复杂

## CtaEngine
1. 初始化
- self.strategies: 创建成功的策略字典
- self.classes: 策略列表
- self.stop_orders: 保存停止单的字典
- self.offset_converter: 仓位转换(先平历史仓位, 再平今仓)
- self.database: 数据库
- self.datafeed: 数据源
2. init_engine
- self.init_datafeed: 初始化数据源
- self.load_strategy_class: 从策略文件夹加载策略保存到`self.classes`中
- self.load_strategy_setting: 加载策略配置json文件"cta_strategy_setting.json"
- self.load_strategy_data: 加载策略数据json文件"cta_strategy_data.json"
- self.register_event: 注册事件引擎
3. query_bar_from_datafeed: 从数据源查询数据
4. check_stop_order: 检查停止单
5. get_pricetick: 通过网关获取品种最小变动价位
6. load_bar: 从数据源/本地数据库加载bar数据
7. load_tick: 从数据库加载tick数据
8. add_strategy: 创建策略保存在`self.strategies`
9. init_strategy: 用线程池初始化策略, 回调策略`on_init`
10. start_strategy/stop_strategy: 启动/停止策略, 回调策略`on_start`/`on_stop`

#### 订单管理
1. send_server_order: 通过网关发送订单, 并获取本地订单id
2. send_limit_order: 通过调用`send_server_order`向服务器发送限价单
3. send_server_stop_order: 通过调用`send_server_stop_order`向服务器发送停止单
4. send_local_stop_order: 发送本地限价单(订单数据保存在本地)
5. cancel_server_order: 撤销服务器订单
6. cancel_local_stop_order: 撤销本地订单
7. send_order: 发送订单的统一接口
8. cancel_order: 撤销订单的统一接口
9. cancel_all: 撤销全部订单

#### 事件处理
- process_tick_event
1. 检查是否触发停止单
2. 回调策略的`on_tick`函数
- process_order_event
1. 订单仓位转换
2. 通过订单id找到对应策略
3. 回调策略的`on_stop_order`和`on_order`
- process_trade_event
1. 成交仓位转换
2. 通过成交id找到对应策略
3. 更新策略持仓
4. 回调策略的`on_trade`
- process_position_event
1. 持仓转换

#### 问题: on_bar在哪回调?
参考"vnpy/trader/utility.py"中的"BarGenerator.update_tick`源码
