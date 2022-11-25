---
title: 日志子引擎
category: trader
order: 2
---

> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)

---

## class LogEngine
1. 传入一个主引擎和一个事件引擎
2. 根据配置判断是否启动日志引擎: SETTINGS["log.active"]
3. 根据配置进行日志设置
- 日志等级: self.level
- 日志对象: self.logger
- 日志格式: self.formatter
4. 添加空的日志句柄: self.add_null_handler
5. 根据配置添加控制台句柄: self.add_console_handler
- 设置日志等级
- 设置日志格式
- 将控制台句柄添加到日志对象self.logger中
6. 根据配置添加文件句柄: self.add_file_handler
- 获取当天日期
- 根据日期创建日志文件名
- 获取日志路径
- 创建文件日志句柄
- 设置日志等级
- 设置日志格式
- 将文件句柄添加到日志对象self.logger中
7. 注册日志事件

#### register_event
注册日志事件
1. 通过事件引擎注册日志事件
2. 处理日志的回调函数是self.process_log_event

#### process_log_event
处理日志事件
1. 获取日志数据
2. 输出日志

> 总结
> 1. 日志引擎主要根据配置控制/添加日志输出