# 功能清单 - Streamlit 应用完整功能盘点

本文档详细列出当前 Streamlit 应用的**所有功能**，用于确认重构范围。

---

## 📊 功能总览

| 模块 | 功能数 | 复杂度 | 迁移优先级 |
|------|--------|--------|-----------|
| 股票分析 | 15 | 🔴 高 | P0 |
| 智瞰龙虎 | 12 | 🟡 中 | P1 |
| 智策板块 | 10 | 🟡 中 | P1 |
| 主力选股 | 8 | 🟡 中 | P1 |
| 低价擒牛 | 7 | 🟢 低 | P2 |
| 智能盯盘 | 9 | 🔴 高 | P2 |
| 持仓管理 | 8 | 🟡 中 | P2 |
| 实时监测 | 10 | 🔴 高 | P2 |
| 系统配置 | 6 | 🟢 低 | P0 |

**总计：85+ 个独立功能点**

---

## 🚀 重构后项目结构 (Next.js + Agno)

> **Agno** 是一个高性能多智能体框架，原生支持 DeepSeek，内置 FastAPI 运行时（AgentOS）。
> 文档：https://docs.agno.com

```
aiagents-stock/
│
├── 🖥️ frontend/                    # Next.js 15 前端
│   ├── app/                        # App Router 页面
│   │   ├── layout.tsx              # 根布局（左侧导航）
│   │   ├── page.tsx                # 首页/仪表盘
│   │   ├── stock/                  # 股票分析
│   │   │   ├── page.tsx            # 单股分析
│   │   │   └── batch/page.tsx      # 批量分析
│   │   ├── longhubang/             # 智瞰龙虎
│   │   │   ├── page.tsx            # 龙虎榜分析
│   │   │   └── history/page.tsx    # 历史记录
│   │   ├── sector/                 # 智策板块
│   │   │   └── page.tsx
│   │   ├── selector/               # 选股策略
│   │   │   ├── main-force/page.tsx # 主力选股
│   │   │   ├── low-price/page.tsx  # 低价擒牛
│   │   │   └── small-cap/page.tsx  # 小市值
│   │   ├── monitor/                # 监控管理
│   │   │   ├── page.tsx            # 实时监测
│   │   │   └── smart/page.tsx      # 智能盯盘
│   │   ├── portfolio/              # 持仓管理
│   │   │   └── page.tsx
│   │   ├── history/                # 历史记录
│   │   │   └── page.tsx
│   │   └── settings/               # 系统设置
│   │       └── page.tsx
│   │
│   ├── components/                 # React 组件
│   │   ├── ui/                     # 基础UI组件
│   │   ├── layout/                 # 布局组件
│   │   ├── charts/                 # 图表组件 (K线图等)
│   │   └── analysis/               # 分析展示组件
│   │
│   ├── lib/                        # 工具库
│   │   ├── api.ts                  # API调用封装
│   │   └── hooks/                  # 自定义Hooks
│   │
│   ├── styles/globals.css
│   ├── package.json
│   └── next.config.js
│
├── 🤖 backend/                      # Agno 多智能体后端
│   ├── agents/                     # 智能体定义
│   │   ├── __init__.py
│   │   ├── technical_agent.py      # 📊 技术分析智能体
│   │   ├── fundamental_agent.py    # 💼 基本面分析智能体
│   │   ├── fund_flow_agent.py      # 💰 资金面分析智能体
│   │   ├── risk_agent.py           # ⚠️ 风险管理智能体
│   │   ├── sentiment_agent.py      # 📈 市场情绪智能体
│   │   ├── news_agent.py           # 📰 新闻分析智能体
│   │   └── coordinator_agent.py    # 🎯 协调员智能体（综合决策）
│   │
│   ├── teams/                      # 智能体团队
│   │   ├── __init__.py
│   │   ├── stock_analysis_team.py  # 股票分析团队
│   │   ├── longhubang_team.py      # 龙虎榜分析团队
│   │   └── sector_team.py          # 板块策略团队
│   │
│   ├── tools/                      # Agno工具（供智能体调用）
│   │   ├── __init__.py
│   │   ├── stock_data_tool.py      # 股票数据获取工具
│   │   ├── fund_flow_tool.py       # 资金流向工具
│   │   ├── news_tool.py            # 新闻获取工具
│   │   ├── technical_tool.py       # 技术指标计算工具
│   │   └── database_tool.py        # 数据库操作工具
│   │
│   ├── knowledge/                  # 知识库（RAG）
│   │   ├── stock_knowledge.py      # 股票知识库
│   │   └── strategy_knowledge.py   # 策略知识库
│   │
│   ├── data/                       # 数据获取层
│   │   ├── stock_data.py           # ← 复用现有模块
│   │   ├── longhubang_data.py      # ← 复用现有模块
│   │   ├── sector_data.py
│   │   └── fund_flow_data.py
│   │
│   ├── db/                         # 数据库层
│   │   ├── database.py             # Agno SqliteDb 集成
│   │   └── models.py               # 数据模型
│   │
│   ├── services/                   # 服务层
│   │   ├── notification.py         # 通知服务
│   │   └── scheduler.py            # 定时任务
│   │
│   ├── main.py                     # AgentOS 入口
│   ├── config.py                   # 配置管理
│   └── pyproject.toml              # uv 包管理配置
│
├── 💾 data/                         # 数据目录
│   ├── *.db                        # SQLite数据库
│   └── exports/                    # 导出文件
│
└── 📝 配置文件
    ├── .env                        # 环境变量
    ├── docker-compose.yml
    └── README.md
```

### 📦 使用 uv 进行包管理

**后端环境初始化**
```bash
cd backend

# 创建虚拟环境 (Python 3.12)
uv venv --python 3.12
source .venv/bin/activate

# 安装依赖
uv pip install -U agno deepseek 'fastapi[standard]' sqlalchemy akshare pandas

# 或使用 pyproject.toml
uv sync
```

**pyproject.toml 示例**
```toml
[project]
name = "aiagents-stock-backend"
version = "0.1.0"
description = "AI Stock Analysis Multi-Agent Backend"
requires-python = ">=3.12"
dependencies = [
    "agno>=2.0.0",
    "deepseek>=0.1.0",
    "fastapi[standard]>=0.115.0",
    "sqlalchemy>=2.0.0",
    "akshare>=1.14.0",
    "pandas>=2.2.0",
    "plotly>=5.24.0",
    "python-dotenv>=1.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0.0",
    "ruff>=0.7.0",
]

[tool.uv]
dev-dependencies = [
    "pytest>=8.0.0",
    "ruff>=0.7.0",
]
```

**启动后端服务**
```bash
cd backend
source .venv/bin/activate
export DEEPSEEK_API_KEY=your_api_key

# 使用 uv 运行
uv run fastapi dev main.py

# 或直接运行
fastapi dev main.py
```

### 🤖 Agno 后端核心代码示例

**main.py - AgentOS 入口**
```python
from agno.agent import Agent
from agno.team import Team
from agno.db.sqlite import SqliteDb
from agno.models.deepseek import DeepSeek
from agno.os import AgentOS

from agents.technical_agent import technical_agent
from agents.fundamental_agent import fundamental_agent
from agents.fund_flow_agent import fund_flow_agent
from agents.risk_agent import risk_agent
from agents.coordinator_agent import coordinator_agent
from tools.stock_data_tool import StockDataTool
from tools.technical_tool import TechnicalIndicatorTool

# ============ 创建股票分析团队 ============
stock_analysis_team = Team(
    name="Stock Analysis Team",
    description="多智能体股票分析团队",
    agents=[
        technical_agent,
        fundamental_agent,
        fund_flow_agent,
        risk_agent,
    ],
    leader=coordinator_agent,  # 协调员作为团队领导
    db=SqliteDb(db_file="data/stock_analysis.db"),
)

# ============ 创建 AgentOS ============
agent_os = AgentOS(
    teams=[stock_analysis_team],
    # 也可以直接添加独立的 agents
)

# 获取 FastAPI 应用
app = agent_os.get_app()

# 运行服务
if __name__ == "__main__":
    agent_os.serve(app="main:app", reload=True)
```

**agents/technical_agent.py - 技术分析智能体**
```python
from agno.agent import Agent
from agno.models.deepseek import DeepSeek
from tools.technical_tool import TechnicalIndicatorTool
from tools.stock_data_tool import StockDataTool

technical_agent = Agent(
    name="Technical Analyst",
    description="技术面分析师，负责K线形态、技术指标分析",
    model=DeepSeek(id="deepseek-chat"),
    tools=[StockDataTool(), TechnicalIndicatorTool()],
    instructions=[
        "你是一位专业的技术分析师",
        "分析股票的K线形态、MACD、RSI、KDJ等技术指标",
        "给出趋势判断和关键价位",
    ],
    markdown=True,
)
```

**tools/stock_data_tool.py - 股票数据工具**
```python
from agno.tools import tool
from data.stock_data import StockDataFetcher

@tool
def get_stock_data(symbol: str, period: str = "1y") -> dict:
    """获取股票行情数据和技术指标"""
    fetcher = StockDataFetcher()
    stock_info = fetcher.get_stock_info(symbol)
    stock_data = fetcher.get_stock_data(symbol, period)
    indicators = fetcher.get_latest_indicators(stock_data)
    return {
        "info": stock_info,
        "data": stock_data.to_dict(),
        "indicators": indicators
    }
```

### 🔄 模块迁移对照表

| 现有模块 | 迁移到 Agno | 说明 |
|----------|-------------|------|
| `ai_agents.py` | `backend/agents/*.py` | 重构为 Agno Agent |
| `deepseek_client.py` | `agno.models.deepseek` | **Agno 原生支持** |
| `stock_data.py` | `backend/tools/stock_data_tool.py` | 封装为 Agno Tool |
| `*_engine.py` | `backend/teams/*.py` | 重构为 Agent Team |
| `*_ui.py` | `frontend/components/` | 重写为 React |
| `*_db.py` | `backend/db/` | 使用 Agno SqliteDb |
| `notification_service.py` | `backend/services/` | 迁移通知服务 |

### ✨ 使用 Agno 的优势

| 特性 | 说明 |
|------|------|
| 🚀 **内置 FastAPI** | AgentOS 自动生成 REST API 端点 |
| 🤖 **原生 DeepSeek** | `agno.models.deepseek.DeepSeek` 直接使用 |
| 👥 **多智能体团队** | Team 类支持智能体协作 |
| 💾 **内置数据库** | SqliteDb 自动管理会话和历史 |
| 🧠 **知识库支持** | 内置 RAG 知识库功能 |
| 🔧 **工具系统** | @tool 装饰器轻松创建工具 |
| 📊 **控制面板** | AgentOS UI 可视化监控 |

---

## 📁 当前项目文件结构 (参考)

```
aiagents-stock/
│
├── 📄 入口与配置
│   ├── app.py                      # 主程序入口 (108KB, 2700+行)
│   ├── run.py                      # 启动脚本
│   ├── config.py                   # 基础配置
│   ├── config_manager.py           # 环境配置管理器
│   ├── model_config.py             # AI模型配置
│   ├── .env                        # 环境变量
│   └── requirements.txt            # Python依赖
│
├── 🤖 AI核心模块
│   ├── ai_agents.py                # 多智能体分析系统 (22KB)
│   ├── deepseek_client.py          # DeepSeek API客户端 (17KB)
│   ├── longhubang_agents.py        # 龙虎榜专用智能体 (20KB)
│   └── sector_strategy_agents.py   # 板块策略智能体 (21KB)
│
├── 🎨 UI界面模块 (*_ui.py)
│   ├── longhubang_ui.py            # 智瞰龙虎界面 (65KB)
│   ├── sector_strategy_ui.py       # 智策板块界面 (42KB)
│   ├── smart_monitor_ui.py         # 智能盯盘界面 (35KB)
│   ├── main_force_ui.py            # 主力选股界面 (37KB)
│   ├── portfolio_ui.py             # 持仓管理界面 (28KB)
│   ├── low_price_bull_ui.py        # 低价擒牛界面 (21KB)
│   ├── profit_growth_ui.py         # 净利增长界面 (14KB)
│   ├── small_cap_ui.py             # 小市值界面 (9KB)
│   ├── monitor_ui.py               # 监测界面 (8KB)
│   ├── monitor_manager.py          # 监测管理界面 (33KB)
│   ├── main_force_history_ui.py    # 主力历史界面 (8KB)
│   └── low_price_bull_monitor_ui.py # 低价擒牛监控界面 (10KB)
│
├── 📊 数据获取模块 (*_data.py)
│   ├── stock_data.py               # 股票行情数据 (41KB)
│   ├── smart_monitor_data.py       # 盯盘实时数据 (35KB)
│   ├── market_sentiment_data.py    # 市场情绪数据 (32KB)
│   ├── sector_strategy_data.py     # 板块策略数据 (32KB)
│   ├── risk_data_fetcher.py        # 风险数据获取 (18KB)
│   ├── quarterly_report_data.py    # 季报数据 (17KB)
│   ├── fund_flow_akshare.py        # 资金流向数据 (13KB)
│   ├── news_announcement_data.py   # 新闻公告数据 (12KB)
│   ├── longhubang_data.py          # 龙虎榜数据 (11KB)
│   ├── qstock_news_data.py         # qstock新闻数据 (11KB)
│   └── smart_monitor_tdx_data.py   # 通达信数据 (16KB)
│
├── 🗄️ 数据库模块 (*_db.py)
│   ├── sector_strategy_db.py       # 板块策略数据库 (36KB)
│   ├── smart_monitor_db.py         # 智能盯盘数据库 (23KB)
│   ├── monitor_db.py               # 监测数据库 (21KB)
│   ├── portfolio_db.py             # 持仓数据库 (20KB)
│   ├── longhubang_db.py            # 龙虎榜数据库 (16KB)
│   ├── main_force_batch_db.py      # 主力批量数据库 (9KB)
│   └── database.py                 # 主数据库 (5KB)
│
├── ⚙️ 引擎模块 (*_engine.py)
│   ├── smart_monitor_engine.py     # 盯盘引擎 (26KB)
│   ├── sector_strategy_engine.py   # 板块策略引擎 (22KB)
│   └── longhubang_engine.py        # 龙虎榜引擎 (14KB)
│
├── 📋 选股策略模块 (*_selector.py)
│   ├── main_force_selector.py      # 主力选股器 (16KB)
│   ├── main_force_analysis.py      # 主力分析 (20KB)
│   ├── low_price_bull_selector.py  # 低价擒牛选股器 (5KB)
│   ├── low_price_bull_strategy.py  # 低价擒牛策略 (8KB)
│   ├── small_cap_selector.py       # 小市值选股器 (4KB)
│   └── profit_growth_selector.py   # 净利增长选股器 (4KB)
│
├── ⏰ 定时调度模块 (*_scheduler.py)
│   ├── portfolio_scheduler.py      # 持仓定时分析 (25KB)
│   ├── sector_strategy_scheduler.py# 板块策略定时 (24KB)
│   └── monitor_scheduler.py        # 监测定时调度 (8KB)
│
├── 📄 PDF生成模块
│   ├── longhubang_pdf.py           # 龙虎榜PDF (18KB)
│   ├── pdf_generator.py            # PDF生成器 (17KB)
│   ├── sector_strategy_pdf.py      # 板块策略PDF (20KB)
│   ├── main_force_pdf_generator.py # 主力选股PDF (14KB)
│   ├── pdf_generator_pandoc.py     # Pandoc PDF (10KB)
│   └── pdf_generator_fixed.py      # 修复版PDF (8KB)
│
├── 🔔 服务模块
│   ├── notification_service.py     # 通知服务 (32KB)
│   ├── monitor_service.py          # 监测服务 (13KB)
│   ├── low_price_bull_service.py   # 低价擒牛服务 (16KB)
│   └── low_price_bull_monitor.py   # 低价擒牛监控 (12KB)
│
├── 🔌 外部接口
│   ├── miniqmt_interface.py        # MiniQMT量化接口 (20KB)
│   ├── smart_monitor_qmt.py        # 盯盘QMT集成 (22KB)
│   ├── smart_monitor_deepseek.py   # 盯盘DeepSeek (21KB)
│   ├── smart_monitor_kline.py      # K线图模块 (18KB)
│   └── data_source_manager.py      # 数据源管理 (13KB)
│
├── 📊 评分模块
│   └── longhubang_scoring.py       # 龙虎榜评分 (20KB)
│
├── 💾 SQLite数据库文件
│   ├── stock_analysis.db           # 主分析数据库 (22MB)
│   ├── main_force_batch.db         # 主力批量分析 (7MB)
│   ├── longhubang.db               # 龙虎榜数据 (991KB)
│   ├── smart_monitor.db            # 智能盯盘 (315KB)
│   ├── sector_strategy.db          # 板块策略 (295KB)
│   ├── portfolio_stocks.db         # 持仓股票 (28KB)
│   ├── stock_monitor.db            # 股票监测 (20KB)
│   ├── low_price_bull_monitor.db   # 低价擒牛监控 (20KB)
│   └── profit_growth_monitor.db    # 净利增长监控 (20KB)
│
├── 📝 配置文件
│   ├── .env                        # 环境变量配置
│   ├── .env.example                # 环境变量示例
│   ├── monitor_schedule_config.json# 监测调度配置
│   ├── docker-compose.yml          # Docker编排
│   └── Dockerfile                  # Docker镜像
│
├── 📚 文档目录 (docs/)
│   └── [71个文档文件]               # 功能说明、使用指南等
│
└── 🔧 其他
    ├── test_tdx_api.py             # 通达信API测试
    ├── update_env_example.py       # 环境示例更新
    └── stm.py                      # 辅助脚本
```

### 📈 代码统计

| 类别 | 文件数 | 总大小 |
|------|--------|--------|
| UI模块 | 12 | ~350KB |
| 数据模块 | 11 | ~240KB |
| 数据库模块 | 7 | ~130KB |
| AI核心 | 4 | ~80KB |
| 引擎模块 | 3 | ~60KB |
| 定时调度 | 3 | ~57KB |
| PDF模块 | 6 | ~87KB |
| 服务模块 | 4 | ~73KB |
| 选股策略 | 6 | ~57KB |
| **总计** | **~60+** | **~1.2MB** |

---

## 🏠 模块1：股票分析 (app.py)

### 1.1 单股分析
| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 股票代码输入 | 支持 A股/港股/美股 格式解析 | 🟢 |
| 数据获取 | 通过 AkShare/yfinance 获取行情数据 | 🟡 |
| 技术指标计算 | MA/MACD/RSI/KDJ/BOLL 等 | 🟡 |
| AI 分析师选择 | 6 位分析师可选配置 | 🟢 |
| K线图展示 | Plotly 交互式 K 线图 | 🟡 |
| 分析结果展示 | 各分析师报告 + 团队讨论 | 🟡 |
| 最终决策 | AI 综合投资建议 | 🟢 |
| PDF 导出 | 生成分析报告 PDF | 🟡 |
| 保存到数据库 | 分析记录持久化 | 🟢 |
| 加入监测 | 一键添加到监测列表 | 🟢 |

### 1.2 批量分析
| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 多股票输入解析 | 换行/逗号/空格分隔 | 🟢 |
| 顺序分析模式 | 逐个分析，稳定 | 🟢 |
| 多线程并行模式 | 并发分析，快速 | 🟡 |
| 进度条展示 | 实时显示分析进度 | 🟡 |
| 批量结果汇总 | 汇总表格 + 评级分布 | 🟡 |

### 1.3 AI 分析师团队
| 分析师 | 主要职责 | 数据源 |
|--------|---------|--------|
| 📊 技术分析师 | K线形态、指标分析 | 行情数据 |
| 💼 基本面分析师 | 财务分析、估值评估 | 财务数据 |
| 💰 资金面分析师 | 资金流向、主力行为 | 资金数据 |
| ⚠️ 风险管理师 | 风险识别、止损建议 | 综合数据 |
| 📈 市场情绪分析师 | ARBR 指标分析 | 情绪数据 |
| 📰 新闻分析师 | 新闻事件、舆情分析 | qstock 新闻 |

---

## 🐉 模块2：智瞰龙虎 (longhubang_ui.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 日期选择 | 选择龙虎榜日期（单/多日） | 🟢 |
| 数据获取 | AkShare 获取龙虎榜数据 | 🟢 |
| AI 智能评分 | 多维度评分排名 | 🟡 |
| 评分排名展示 | TOP 股票卡片列表 | 🟡 |
| 推荐股票展示 | AI 推荐 + 理由 | 🟢 |
| AI 分析师报告 | 3 大分析师完整报告 | 🟡 |
| 数据详情表格 | 完整龙虎榜数据 | 🟢 |
| 可视化图表 | 资金流向/机构分布图 | 🟡 |
| PDF 导出 | 报告 PDF 下载 | 🟡 |
| 历史报告查看 | 历史分析记录列表 | 🟡 |
| 批量分析 TOP 股票 | 对 TOP N 股票深度分析 | 🔴 |
| 加入监测 | 一键添加到监测 | 🟢 |

---

## 🎯 模块3：智策板块 (sector_strategy_ui.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 板块数据获取 | 获取行业/概念板块数据 | 🟡 |
| AI 分析运行 | 板块策略分析 | 🟡 |
| 预测结果展示 | 看多/看空板块 | 🟢 |
| 智能体报告 | 各智能体分析报告 | 🟡 |
| 综合研判报告 | 最终结论 | 🟢 |
| 数据可视化 | 板块热力图/对比图 | 🟡 |
| PDF 导出 | 报告下载 | 🟡 |
| 历史报告 | 历史记录查看 | 🟢 |
| 定时任务设置 | 定时分析配置 | 🔴 |
| 邮件通知 | 分析完成邮件推送 | 🟡 |

---

## 💰 模块4：主力选股 (main_force_ui.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 时间区间选择 | 3个月/6个月/1年/自定义 | 🟢 |
| 高级筛选参数 | 涨幅/市值范围设置 | 🟢 |
| 数据获取（问财） | 主力资金 TOP100 | 🟡 |
| 智能筛选 | 过滤不符合条件股票 | 🟡 |
| 3 大分析师分析 | 资金/行业/财务 | 🟡 |
| 精选推荐展示 | 推荐股票卡片 | 🟢 |
| 候选列表表格 | 完整数据下载 | 🟢 |
| 批量深度分析 | TOP N 股票 AI 分析 | 🔴 |

---

## 🐂 模块5：低价擒牛 (low_price_bull_ui.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 筛选条件展示 | 策略说明 | 🟢 |
| 选股执行 | 低价高成长股筛选 | 🟡 |
| 结果展示 | 股票卡片列表 | 🟢 |
| 股票详情 | 基本信息 + 财务指标 | 🟢 |
| 策略模拟 | 量化交易模拟 | 🟡 |
| 钉钉通知 | 选股结果推送 | 🟡 |
| 策略监控 | 加入监控列表 | 🟡 |

---

## 🤖 模块6：智能盯盘 (smart_monitor_ui.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 实时分析界面 | 单股实时分析 | 🟡 |
| 分析结果展示 | K 线 + AI 决策 | 🟡 |
| 监控任务管理 | 添加/删除监控股票 | 🟡 |
| 任务 K 线展示 | 实时 K 线图 | 🔴 |
| 持仓管理 | 持仓录入和跟踪 | 🟡 |
| 历史记录 | 分析历史查看 | 🟢 |
| 系统设置 | 配置参数 | 🟢 |
| AI 决策信号 | 买入/卖出/持有信号 | 🟡 |
| 数据源切换 | AkShare/TDX 切换 | 🟡 |

---

## 💼 模块7：持仓管理 (portfolio_ui.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 持仓股票列表 | 卡片式展示 | 🟢 |
| 添加股票 | 表单添加持仓 | 🟢 |
| 编辑/删除 | 持仓信息修改 | 🟢 |
| 批量分析 | 持仓股票批量 AI 分析 | 🔴 |
| 分析结果卡片 | 单股分析结果 | 🟢 |
| 定时任务管理 | 定时分析配置 | 🔴 |
| 分析历史 | 历史分析记录 | 🟢 |
| 进度展示 | 批量分析进度 | 🟡 |

---

## 📡 模块8：实时监测 (monitor_manager.py)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| 监测服务状态 | 启动/停止服务 | 🟢 |
| 添加监测股票 | 输入代码+关键价位 | 🟢 |
| 监测卡片展示 | 当前价格+监测状态 | 🟡 |
| 编辑监测 | 修改关键价位 | 🟢 |
| 删除监测 | 移除监测股票 | 🟢 |
| 通知管理 | 查看/清除通知 | 🟢 |
| MiniQMT 状态 | 量化交易连接状态 | 🟡 |
| 定时调度配置 | 扫描频率/时间设置 | 🔴 |
| 实时价格刷新 | 自动获取最新价格 | 🟡 |
| 触发通知 | 价格触发邮件/Webhook | 🔴 |

---

## ⚙️ 模块9：系统配置 (config_manager.py + display_config_manager)

| 功能 | 描述 | 复杂度 |
|------|------|--------|
| DeepSeek API 配置 | API Key + Base URL | 🟢 |
| Tushare 配置 | 数据接口 Token | 🟢 |
| MiniQMT 配置 | 量化交易连接参数 | 🟢 |
| 邮件通知配置 | SMTP 服务器设置 | 🟡 |
| Webhook 配置 | 钉钉/飞书通知 | 🟡 |
| 配置保存/测试 | 配置验证和持久化 | 🟢 |

---

## 🔔 通知服务 (notification_service.py)

| 功能 | 描述 |
|------|------|
| 邮件通知 | SMTP 邮件发送 |
| 钉钉 Webhook | Markdown 格式消息 |
| 飞书 Webhook | 飞书消息卡片 |
| 通知测试 | 测试邮件/Webhook |
| 持仓分析通知 | 批量分析完成推送 |
| 通知队列管理 | 待发送通知处理 |

---

## 📦 数据模块概览

| 模块 | 职责 |
|------|------|
| `stock_data.py` | 股票行情、技术指标 |
| `longhubang_data.py` | 龙虎榜数据 |
| `sector_strategy_data.py` | 板块数据 |
| `market_sentiment_data.py` | 市场情绪数据 |
| `fund_flow_akshare.py` | 资金流向数据 |
| `quarterly_report_data.py` | 季报数据 |
| `news_announcement_data.py` | 新闻公告数据 |
| `risk_data_fetcher.py` | 风险数据 |
| `smart_monitor_data.py` | 盯盘实时数据 |

---

## 🗄️ 数据库模块

| 模块 | 数据库文件 |
|------|-----------|
| `database.py` | `stock_analysis.db` |
| `longhubang_db.py` | `longhubang.db` |
| `sector_strategy_db.py` | `sector_strategy.db` |
| `monitor_db.py` | `stock_monitor.db` |
| `smart_monitor_db.py` | `smart_monitor.db` |
| `portfolio_db.py` | `portfolio_stocks.db` |
| `main_force_batch_db.py` | `main_force_batch.db` |

---

## ✅ 功能确认清单

请确认以下功能是否需要迁移：

### 核心功能 (强烈建议迁移)
- [ ] 单股 AI 分析
- [ ] 批量股票分析
- [ ] 分析历史记录
- [ ] K 线图可视化
- [ ] PDF 报告导出

### 策略分析功能
- [ ] 智瞰龙虎
- [ ] 智策板块
- [ ] 主力选股
- [ ] 低价擒牛

### 监控管理功能
- [ ] 实时价格监测
- [ ] 智能盯盘
- [ ] 持仓管理
- [ ] 定时任务

### 通知功能
- [ ] 邮件通知
- [ ] 钉钉 Webhook
- [ ] 飞书 Webhook

### 系统配置
- [ ] API 密钥管理
- [ ] 数据源配置
- [ ] 通知配置

---

## 🎯 迁移建议

### Phase 1 (1-2 周) - 核心功能
1. 股票分析 (单股 + 批量)
2. 历史记录
3. 系统配置
4. K 线图表

### Phase 2 (2-3 周) - 策略模块
1. 智瞰龙虎
2. 智策板块
3. 主力选股

### Phase 3 (1-2 周) - 监控通知
1. 实时监测
2. 持仓管理
3. 通知服务
4. 智能盯盘

### Phase 4 (1 周) - 高级功能
1. 定时任务
2. PDF 导出
3. 低价擒牛
4. MiniQMT 集成

---

**请告诉我哪些功能需要保留，哪些可以暂时不做？**
