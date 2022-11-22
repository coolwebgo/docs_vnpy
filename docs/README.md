## 文档目录
#### 入门
> 假设你对编程一无所知
1. 量化交易流程
2. [Windows安装指南](https://www.vnpy.com/docs/cn/windows_install.html)
3. [启动VeighNa Station](https://www.vnpy.com/docs/cn/veighna_station.html)
4. [启动VeighNa Trader](https://www.vnpy.com/docs/cn/veighna_trader.html)
5. [CTP交易接口](https://www.vnpy.com/docs/cn/gateway.html)
6. [TTS交易接口](https://www.vnpy.com/docs/cn/gateway.html)
7. [数据配置](https://www.vnpy.com/docs/cn/database.html)
8. [回测策略](https://www.vnpy.com/docs/cn/cta_backtester.html)
9. [启动策略](https://www.vnpy.com/docs/cn/cta_strategy.html)
10. [价差套利](https://www.vnpy.com/docs/cn/spread_trading.html)
11. [本地仿真](https://www.vnpy.com/docs/cn/paper_account.html)
12. [数据管理](https://www.vnpy.com/docs/cn/data_manager.html)
13. [事前风控](https://www.vnpy.com/docs/cn/risk_manager.html)
14. [实时K线](https://www.vnpy.com/docs/cn/chart_wizard.html)

#### 基础
> 假设你对python一知半解
1. 源码安装
- 安装talib
- 安装源码
2. 代码启动
3. 策略编写
- 模板/逻辑
- 注意事项
4. 策略回测
5. [脚本运行](https://www.vnpy.com/docs/cn/script_trader.html)
6. 数据管理
7. K线合成

#### 进阶
> 假设你对源码兴趣浓厚
1. 通用类组件逻辑
- vnpy.event，简洁易用的事件驱动引擎，作为事件驱动型交易程序的核心
- vnpy.chart，Python高性能K线图表，支持大数据量图表显示以及实时数据更新功能
- vnpy.trader.database，集成了几大数据库管理端模块，以支持数据库读写性能和未来的新数据库扩展
- vnpy.trader.datafeed，提供标准化接口BaseDataFeed，带来了更加灵活的数据服务支持
2. 数据接口源码
3. 回测源码
4. 仿真交易源码
5. 事前风控源码
