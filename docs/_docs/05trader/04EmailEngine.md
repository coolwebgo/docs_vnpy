---
title: 邮件子引擎
category: trader
order: 4
---

> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)
---

## class EmailEngine
1. 传入一个主引擎和一个事件引擎
2. 创建一个线程(专门处理邮件): self.thread
3. 创建一个队列(传递邮件数据): self.queue
4. 将发送邮件的方法传递给主引擎

#### send_email
发送邮件
1. 判断邮件发送线程是否启动
2. 如果没有传入邮箱, 则使用全局配置中的邮箱
3. 定义发送邮件的数据
4. 将邮件数据放入队列

#### run
启动邮件发送线程死循环
1. 从队列中获取邮件数据
2. 利用smtp登陆邮箱
3. 利用smtp发送邮件

#### start
启动邮件发送线程

#### close
关闭邮件发送线程

> 总结
> 邮件发送引擎单独启动一个线程, 默认根据配置发送