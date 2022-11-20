---
title: 事件引擎
category: event
order: 1
---

## class Event
定义事件对象, 包含两部分
1. 事件类型: 字符串(事件名称)
2. 事件数据: 任意类型

## HandlerType
事件处理器, 实际就是一个回调函数, 输入是Event, 没有返回值

## class EventEngine
定义事件引擎
1. 事件计时器间隔: self._interval, 默认1s触发一次
2. 事件运行标志: self._active, Bool类型
3. 创建一个线程处理事件任务: self._thread
4. 创建一个线程做计时器: self._timer
5. 创建一个队列做通信: self._queue
6. 创建一个指定类型的事件处理器字典: self._handlers
- 字典键是事件类型(事件名称)
- 字典值是处理器(回调函数)列表
7. 创建一个不指定类型的事件处理器列表: self._general_handlers

#### start
启动事件引擎, 做了三件事
1. 事件运行中标志: self._active=True
2. 启动线程: self._thread.start()
3. 启动计时器: self._timer.start()

#### stop
停止事件引擎, 做了三件事
1. 事件停止标志: self._active=False
2. 停止线程: self._thread.join()
3. 停止计时器: self._timer.join()

#### _run
一个死循环, 由self._thread启动, 做了两件事
1. 不断从队列中获取事件(Event类型)
2. 处理获取的事件(self._process)

#### _run_timer
一个死循环, 由self._timer启动, 做两三件事
1. 等待1s(self._interval)
2. 创建一个计时器事件(EVENT_TIMER = "eTimer")
3. 将计时器事件放入队列self._queue

#### put
将传入的事件放入队列self._queue

#### register
注册一个指定类型的事件处理器handler, 做了两件事
1. 获取传入类型的事件处理器列表
2. 如果传入的处理器不在列表中则添加进去

#### unregister
取消注册一个指定类型的事件处理器handler, 做了两件事
1. 获取传入类型的事件处理器列表
2. 如果传入的处理器在列表中则移除
3. 如果传入类型的事件处理器列表为空, 则从字典中删除该类型

#### register_general
注册一个不指定类型的事件处理器handler, 只做一件事
1. 如果传入的处理器不在列表中则添加进去

#### unregister_general
取消注册一个不指定类型的事件处理器handler, 只做一件事
1. 如果传入的处理器在列表中则移除

#### _process
处理传入的事件, 做了两件事
1. 如果事件在self._handlers中
- 通过事件类型获取事件处理器列表(self._handlers[event.type])
- 遍历事件处理器列表
- 用事件处理器处理传入的事件
2. 如果self._general_handlers不为空
- 遍历事件处理器列表
- 用事件处理器处理事件

> 总结
> 1. 事件处理器就是一个处理Event事件的回调函数
> 2. EventEngine中有两个事件处理器集合, 分别是指定类型(字典保存))和不指定类型(列表保存)
> 3. EventEngine启动了两个线程, 一个专门处理事件循环(self._thread), 一个是用作计时器(self._timer)
> 4. 未解决问题: 如何向self._handlers中添加新的类型事件呢?

## defaultdict字典
```python
from collections import defaultdict

# 1. 创建字典
handler = defaultdict(list)
# 2. 创建etimer事件
etimer = "timer"
# 3. 创建etimer事件处理器
htimer = "htimer"

# 1. 查看handler为空
print("0: ", handler)
# 2. 从空的handler获取etimer事件处理列表
h = handler[etimer]
# 3. 此处关键点是即使handler中没有键名为etimer, 也会自动创建, 值为空列表
print("1: ", handler)
if htimer not in h:
    handler[etimer].append(htimer)

print("2: ", handler)
```

## 计时器流程
1. 通过计时器线程self._timer启动_run_timer
2. 通过_run_timer生成计时器事件
3. 通过put将计时器事件传入队列
4. 通过事件线程self._thread启动_run
5. 通过_run获取队列中的计时器事件
6. 通过_process处理计时器事件
- 计时器类型是: EVENT_TIMER = "eTimer"
- 计时器类型不在self._handlers中所以不会执行(未注册)

**计时器测试**
```python
def print_timer(e: Event):
    print("计时器启动: ", e.type)

if __name__ == '__main__':
    en = EventEngine()
    # 将计时器事件注册到事件处理器字典中
    en.register(EVENT_TIMER, print_timer)
    en.start()
```