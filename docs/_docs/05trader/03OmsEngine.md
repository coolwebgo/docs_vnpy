---
title: 订单管理子引擎
category: trader
order: 3
---
> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)
---

## class OmsEngine
1. 传入一个主引擎和一个事件引擎
2. 创建订单数据字典
- self.ticks: 保存ticks数据
- self.orders: 保存orders数据
- self.trades: 保存trades数据
- self.positions: 保存positions数据
- self.accounts: 保存accounts数据
- self.contracts: 保存contracts数据
- self.quotes: 保存quotes数据
- self.active_orders: 保存活跃订单数据
- self.active_quotes: 保存活跃报单数据
3. 将子引擎方法添加到主引擎中: add_function
4. 注册事件: register_event
- 注册tick事件, 回调函数是: process_tick_event
- 注册order事件, 回调函数是: process_order_event
- 注册trade事件, 回调函数是: process_trade_event
- 注册position事件, 回调函数是: process_position_event
- 注册account事件, 回调函数是: process_account_event
- 注册contract事件, 回调函数是: process_contract_event
- 注册quote事件, 回调函数是: process_quote_event

#### 添加数据
1. process_tick_event: 处理tick事件
- 将tick数据添加到self.ticks中
2. process_order_event: 处理order事件
- 将order数据添加到self.orders中
- 添加/删除活跃订单数据
3. process_trade_event: 处理trade事件
- 将trade数据添加到self.trades中
4. process_position_event: 处理positon事件
- 将position数据添加到self.positions中
5. process_account_event: 处理account事件
- 将account数据添加到self.accounts中
6. process_contract_event: 处理contract事件
- 将contract数据添加到self.contracts中
7. process_quote_event: 处理quote事件
- 将quote数据添加到self.quotes中
- 添加/删除活跃报单数据

#### 获取一条数据
- get_tick
- get_order
- get_trade
- get_position
- get_account
- get_contract
- get_quote

#### 获取全部数据
- get_all_ticks
- get_all_orders
- get_all_trades
- get_all_positions
- get_all_accounts
- get_all_contracts
- get_all_quotes
- get_all_active_orders
- get_all_active_quotes


> 总结
> 1. 订单管理引擎主要定义了一些获取和保存数据的方法