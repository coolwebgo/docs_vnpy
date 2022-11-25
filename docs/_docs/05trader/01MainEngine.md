---
title: 主引擎
category: trader
order: 1
---
> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)
---

## class MainEngine
整个系统都通过主引擎调度
1. 传入一个事件引擎或新建一个事件引擎
2. 启动事件引擎
3. 创建交易所网关字典: self.gateways
- 键是网关名称: 字符串
- 值是网关对象: BaseGateway类型
4. 创建子引擎字典: self.engines
- 键是引擎名称: 字符串
- 值是引擎对象: BaseEngine类型
5. 创建app字典: self.apps
- 键是app名称: 字符串
- 值是app对象: BaseApp类型
6. 创建交易所列表: self.exchanges
7. 修改全局工作目录, 所有数据都在该目录下: ".vntrader"
8. 添加三个重要的子引擎: init_engines
- LogEngine 
- OmsEngine
- EmailEngine

#### add_engine
添加子引擎
1. 将主引擎自己(self)和事件引擎(self.event_engine)作为参数创建子引擎对象
2. 将子引擎对象添加到子引擎字典self.engines中

#### add_gateway
添加网关
1. 将事件引擎作为参数创建网关对象
2. 将网关对象添加到self.gateways中
3. 将网关对象的交易所添加到self.exchanges中

#### add_app
添加app
1. 直接生成app对象
2. 将app对象添加到self.apps中
3. 将app中的engine对象添加到主引擎

#### write_log
输出日志
1. 创建LogData对象
2. 创建log事件
3. 将log事件添加到事件引擎中

#### get_...
1. get_gateway: 获取网关对象
2. get_engine: 获取子引擎对象
3. get_default_setting: 获取网关的默认设置
4. get_all_gateway_names: 获取所有网关名称
5. get_all_apps: 获取所有app对象
6. get_all_exchanges: 获取所有交易所

#### 网关相关方法
1. connect: 连接网关
- 通过名称获取网关对象
- 网关对象启动连接
2. subscribe: 网关订阅数据
- 通过名称获取网关对象
- 网关对象订阅数据
3. send_order: 通过网关发送订单
4. cancel_order: 通过网关撤单
5. send_quote: 通过网关发送报单
6. cancel_quote: 通过网关撤销报单
7. query_history: 通过网关查询历史数据

#### close
关闭主引擎
1. 停止事件引擎
2. 关闭所有子引擎
3. 关闭所有网关

> 总结
> 1. 主引擎会将自己作为参数传递给子引擎(二者无继承关系)
> 2. 主引擎中包含所有网关, 所有子引擎和所有app对象