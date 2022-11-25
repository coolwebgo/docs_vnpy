---
title: engine
category: vnpy_datamanager
order: 1
---

> [文档纠错/补充](https://github.com/dumengru/docs_vnpy/tree/master/docs/_docs)

---

## 简介
"vnpy_datamanager/engine.py"中的`ManagerEngine`是专门用来管理数据库的引擎接口

## ManagerEngine
1. 初始化
- self.database: 数据库对象
- self.datafeed: 数据源对象
2. import_data_from_csv: 从csv文件中加载数据保存到数据库中
3. output_data_to_csv: 从数据库中导出数据到csv文件中
4. get_bar_overview: 获取数据库中bar数据视图
5. load_bar_data: 从数据库中加载bar数据
6. delete_bar_data: 从数据库中删除bar数据
7. download_bar_data: 从交易所网关或数据源获取历史数据并保存到数据库
8. download_tick_data: 从数据源获取bar历史数据并保存到数据库

这些方法对应着数据库管理的图形界面, 比较简单