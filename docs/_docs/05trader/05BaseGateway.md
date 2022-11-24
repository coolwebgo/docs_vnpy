---
title: 网关父类
category: trader
order: 5
---
> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)
---
## class BaseGateway
1. 传入默认设置
- 默认网关名称
- 默认网关设置
- 默认交易所列表
2. 传入事件引擎和网关名称

#### on_event
创建事件
1. 根据传入类型创建指定事件
2. 将事件放入事件引擎

#### 创建特殊事件
1. on_tick
- 创建tick事件
- 创建特定tick事件(tick+tick.vt_symbol)
2. on_trade
- 创建trade事件
- 创建特定trade事件
3. on_order
- 创建order事件
- 创建特定order事件
4. on_position
- 创建position事件
- 创建特定position事件
5. on_account
- 创建account事件
- 创建特定account事件
6. on_quote
- 创建quote事件
- 创建特定quote事件
7. on_contract
- 创建contract事件
8. on_log
- 创建log事件

#### write_log
输出日志
1. 创建日志对象
2. 创建日志对象self.log

#### 接口函数
需要子类定义, 不同交易所网关方法不同
1. connect: 连接网关
2. close: 关闭网关连接
3. subscribe: 订阅数据
4. send_order: 发送订单
5. cancel_order: 撤销订单
6. send_quote: 发送报单
7. cancel_qote: 撤销报单
8. 查询函数
- query_account: 查询账户
- query_position: 查询持仓
- query_history: 查询历史数据

> 总结
> 1. 网关父类主要定义各种类型事件和一些公共接口