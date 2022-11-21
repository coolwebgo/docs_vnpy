---
title: 运行vn_trader
category: 示例演示
order: 1
---

## 启动代码

```python
from vnpy.event import EventEngine
from vnpy.trader.engine import MainEngine
from vnpy.trader.ui import MainWindow, create_qapp
from vnpy_ctp import CtpGateway
from vnpy_ctastrategy import CtaStrategyApp
from vnpy_ctabacktester import CtaBacktesterApp
from vnpy_datamanager import DataManagerApp


def main():
    """"""
    # 1. 创建一个QT界面
    qapp = create_qapp()
    # 2. 创建事件引擎
    event_engine = EventEngine()
    # 3. 将事件引擎添加到主引擎, 并创建主引擎
    main_engine = MainEngine(event_engine)
    # 4. 添加交易网关
    main_engine.add_gateway(CtpGateway)
    # 5. 添加功能模块
    main_engine.add_app(CtaStrategyApp)
    main_engine.add_app(CtaBacktesterApp)
    main_engine.add_app(DataManagerApp)

    # 6. 将主引擎和事件引擎添加到QT窗口
    main_window = MainWindow(main_engine, event_engine)
    main_window.showMaximized()
    qapp.exec()

if __name__ == "__main__":
    main()
```

## 踩坑记

首次启动会报错, `ModuleNotFoundError: No module named ...`, 直接`pip install ...`即可

```python
pip install qdarkstyle

pip install PySide6==6.3.0     # 只能用该版本(6.4会报错)
pip install deap
pip install vnpy_sqlite
pip install vnpy_rqdata
pip install pyqtgraph
```

弹出VnTrader界面说明启动成功
