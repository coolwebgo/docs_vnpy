---
title: sqlite_database
category: vnpy_sqlite
order: 1
---

> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)

---

## 简介
vnpy使用peewee库来管理各种数据库接口, "vnpy_sqlite/sqlite_database.py"实际只是简单实现了"vnpy/trader/database.py"中的方法

1. DbBarData: 定义bar数据类型
2. DbTickData: 定义tick数据类型
3. DbBarOverview: 定义bar数据视图
4. DbTickOverview: 定义tick数据视图

## SqliteDatabase
1. 初始化: 创建并连接数据库, 同时创建四张数据表
- DbBarData
- DbTickData
- DbBarOverview
- DbTickOverview
2. save_bar_data/load_bar_data/delete_bar_data: 在DbBarData数据表中保存/加载/删除bar数据
3. save_tick_data/load_tick_data/delete_tick_data: 在DbTickData数据表中保存/加载/删除tick数据

