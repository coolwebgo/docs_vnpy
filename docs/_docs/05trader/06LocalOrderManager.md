---
title: 本地订单管理类
category: trader
order: 6
---
> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)
---
## class LocalOrderManager
1. 传入一个网关和订单前缀
2. 创建本地订单字典
3. 创建本地和系统订单字典
4. 创建系统订单字典: self.push_data_buf
5. 创建系统订单数据处理函数: self.push_data_callback
6. 创建本地撤单字典: self.cancel_request_buf
7. 本地撤单数据回调函数(网关的撤单函数): gateway.cancel_order

#### new_local_orderid
生成新的订单id
- 订单id规则: 订单前缀+8位数字

#### get_local_orderid
根据系统订单id获取本地订单id

#### get_sys_orderid
根据本地订单id获取系统订单id

#### update_orderid_map
更新订单id映射
1. 更新系统订单映射
2. 更新本地订单映射
3. 检查本地撤单
4. 处理系统订单数据

#### check_cancel_request
1. 如果订单id不在self.cancel_request_buf中(说明已撤单成功), 直接返回
2. 通过self.cancel_order撤单

#### check_push_data
检查系统订单是否被处理
1. 如果系统订单id在self.push_data_buf中(说明订单已被处理)则直接返回
2. 从self.push_data_buf中获取订单数据
3. 通过self.push_data_callback处理订单数据

#### add_push_data
向self.push_data_buf中添加数据

#### get_order_with_sys_orderid
获取本地订单
1. 传入系统订单id
2. 通过系统id获取本地id
3. 通过本地id获取本地订单

#### get_order_with_local_orderid
通过本地id获取本地订单

#### on_order
1. 向self.orders中添加数据
2. 回调网关on_order函数

#### cancel_order
撤单
1. 传入本地撤单订单
2. 通过本地订单id获取系统id
3. 如果系统id不存在(说明系统已撤单成功), 则将将其添加到self.cancel_request_buf中
4. 如果存在, 则通过网关撤单

> 总结
> 1. 本地订单管理类主要管理本地订单和系统订单之间的对应关系