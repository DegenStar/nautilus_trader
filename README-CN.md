# <img src="https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/nautilus-trader-logo.png" alt="NautilusTrader" width="500">

[![rustc](https://img.shields.io/crates/msrv/nautilus-core?color=ea7233&logo=rust&label=rustc)](https://crates.io/crates/nautilus-core)
[![crates.io](https://img.shields.io/crates/v/nautilus-core?logo=rust)](https://crates.io/crates/nautilus-core)
[![codspeed](https://img.shields.io/endpoint?url=https://codspeed.io/badge.json)](https://codspeed.io/nautechsystems/nautilus_trader)
![pythons](https://img.shields.io/pypi/pyversions/nautilus_trader)
![pypi-version](https://img.shields.io/pypi/v/nautilus_trader)
[![下载量](https://img.shields.io/pepy/dt/nautilus-trader?color=blue)](https://pepy.tech/projects/nautilus-trader)
[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?logo=discord&logoColor=white)](https://discord.gg/NautilusTrader)

| 分支        | 版本                                                                                                                                                                                                                          | 状态                                                                                                                                                                                                |
| :-------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `master`  | [![version](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Fnautechsystems%2Fnautilus_trader%2Fmaster%2Fversion.json)](https://packages.nautechsystems.io/simple/nautilus-trader/index.html)  | [![build](https://github.com/nautechsystems/nautilus_trader/actions/workflows/build.yml/badge.svg?branch=master)](https://github.com/nautechsystems/nautilus_trader/actions/workflows/build.yml)  |
| `nightly` | [![version](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Fnautechsystems%2Fnautilus_trader%2Fnightly%2Fversion.json)](https://packages.nautechsystems.io/simple/nautilus-trader/index.html) | [![build](https://github.com/nautechsystems/nautilus_trader/actions/workflows/build.yml/badge.svg?branch=nightly)](https://github.com/nautechsystems/nautilus_trader/actions/workflows/build.yml) |
| `develop` | [![version](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Fnautechsystems%2Fnautilus_trader%2Fdevelop%2Fversion.json)](https://packages.nautechsystems.io/simple/nautilus-trader/index.html) | [![build](https://github.com/nautechsystems/nautilus_trader/actions/workflows/build.yml/badge.svg?branch=develop)](https://github.com/nautechsystems/nautilus_trader/actions/workflows/build.yml) |

| 平台                 | Rust   | Python    |
| :----------------- | :----- | :-------- |
| `Linux (x86_64)`   | 1.98.0 | 3.12-3.14 |
| `Linux (ARM64)`    | 1.98.0 | 3.12-3.14 |
| `macOS (ARM64)`    | 1.98.0 | 3.12-3.14 |
| `Windows (x86_64)` | 1.98.0 | 3.12-3.14 |

- **文档**：<https://nautilustrader.io/docs/>
- **网站**：<https://nautilustrader.io>
- **支持**：[support@nautilustrader.io](mailto:support@nautilustrader.io)

## 简介

NautilusTrader 是一个开源、生产级、基于 Rust 原生引擎构建的多资产、多交易场所交易系统。

该系统在同一个事件驱动架构中贯通研究、确定性模拟与实盘执行，并以 Python 作为策略逻辑、
配置和编排的控制平面。

这种分层既具备编译型交易引擎的性能与安全性，又拥有 Python 在系统组合和策略开发方面的灵活性。
对于关键任务型工作负载，也可以完全使用 Rust 编写交易系统。

研究系统与实盘系统采用相同的执行语义和确定性时间模型。策略从研究环境部署至生产环境时无需修改代码，
从而实现研究与实盘的一致性，并减少通常会引发部署风险的差异。

NautilusTrader 不限定资产类别。任何提供 REST API 或 WebSocket 数据流的交易场所都可以通过模块化适配器接入。
目前的集成涵盖加密货币交易所（CEX 和 DEX）、传统市场（外汇、股票、期货、期权）以及博彩交易所。

![nautilus-trader](https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/nautilus-trader.png "nautilus-trader")

## 特性

- **高性能**：Rust 核心采用 [mimalloc](https://github.com/microsoft/mimalloc) 内存分配器，并使用 [tokio](https://crates.io/crates/tokio) 实现异步网络通信。
- **可靠**：由 Rust 提供类型安全和线程安全保障，并可选用 Redis 支持的状态持久化。
- **可移植**：可在 Linux、macOS 和 Windows 上运行，并可使用 Docker 部署。
- **灵活**：通过模块化适配器集成任意 REST API 或 WebSocket 数据流。
- **高级功能**：支持 `IOC`、`FOK`、`GTC`、`GTD`、`DAY`、`AT_THE_OPEN`、`AT_THE_CLOSE` 等有效期类型、高级订单类型和条件触发器；支持 `post-only`、`reduce-only` 和冰山单等执行指令；支持包括 `OCO`、`OUO`、`OTO` 在内的关联订单。
- **可定制**：可使用用户自定义组件，也可利用[缓存](https://nautilustrader.io/docs/latest/concepts/cache)和[消息总线](https://nautilustrader.io/docs/latest/concepts/message_bus)从零组装完整系统。
- **回测**：可使用纳秒级历史报价逐笔、成交逐笔、K 线、订单簿和自定义数据，同时对多个交易场所、交易工具和策略进行回测。
- **实盘**：研究环境与实盘部署采用完全相同的策略实现。
- **多交易场所**：可同时在多个交易场所运行做市策略和跨场所策略。
- **AI 训练**：引擎速度足以训练 AI 交易智能体（RL/ES）。

![nautilus](https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/nautilus-art.png "nautilus")

> *nautilus（鹦鹉螺）一词源自古希腊语中的“水手”和表示“船”的 naus。*
>
> *鹦鹉螺的外壳由模块化腔室组成，其生长因子近似形成对数螺旋。
> 这一意象可以转化为设计和架构的美学。*

## 为什么选择 NautilusTrader？

交易策略研究通常使用 Python 和向量化方法完成，而生产交易系统则另行使用编译型语言和事件驱动架构实现。

NautilusTrader 消除了这种割裂。

Rust 原生核心为研究和实盘执行提供确定性的事件驱动运行时，Python 则作为控制平面。
两个环境采用相同的架构、执行语义和时间模型，使策略无需重新实现即可从研究环境迁移至生产环境。

基于 Rust 的 v2 运行时通过 [PyO3](https://pyo3.rs) 提供 Python 绑定。
在向 v2 过渡期间，v1 只会在 `develop_v1` 分支接收关键安全修复的向后移植。
有关迁移步骤和兼容性详情，请参阅 [v2 迁移指南](https://github.com/nautechsystems/nautilus_trader/blob/develop/MIGRATION_V2.md)。
安装预构建 wheel 无需 Rust 工具链。

本项目遵循 [健全性承诺（Soundness Pledge）](https://raphlinus.github.io/rust/2020/01/18/soundness-pledge.html)：

> “本项目致力于杜绝健全性缺陷。
> 开发者将尽最大努力避免此类缺陷，并欢迎大家协助分析和修复。”

> [!NOTE]
>
> **MSRV：** NautilusTrader 大量采用 Rust 语言和编译器的最新改进。
> 因此，最低支持的 Rust 版本（MSRV）通常与最新稳定版 Rust 相同。

## 集成

NautilusTrader 采用模块化设计，通过*适配器*连接交易场所和数据提供商，
将其原始 API 转换为统一接口和标准化领域模型。

目前支持以下集成；详情请参阅 [docs/integrations/](https://nautilustrader.io/docs/latest/integrations/)：

| 名称                                                         | ID                    | 类型           | 状态                                               | 文档                                             |
| :--------------------------------------------------------- | :-------------------- | :----------- | :----------------------------------------------- | :--------------------------------------------- |
| [AX Exchange](https://architect.exchange)                  | `AX`                  | 永续合约交易所      | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/architect_ax.md)        |
| [Betfair](https://betfair.com)                             | `BETFAIR`             | 体育博彩交易所      | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/betfair.md)             |
| [Binance](https://binance.com)                             | `BINANCE`             | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/binance.md)             |
| [BitMEX](https://www.bitmex.com)                           | `BITMEX`              | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/bitmex.md)              |
| [Bybit](https://www.bybit.com)                             | `BYBIT`               | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/bybit.md)               |
| [Coinbase](https://coinbase.com)                           | `COINBASE`            | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/coinbase.md)            |
| [Databento](https://databento.com)                         | `DATABENTO`           | 数据提供商        | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/databento.md)           |
| [Deribit](https://www.deribit.com)                         | `DERIBIT`             | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/deribit.md)             |
| [Derive](https://www.derive.xyz)                           | `DERIVE`              | 加密货币交易所（DEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/derive.md)              |
| [dYdX](https://dydx.exchange/)                             | `DYDX`                | 加密货币交易所（DEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/dydx.md)                |
| [Hyperliquid](https://hyperliquid.xyz)                     | `HYPERLIQUID`         | 加密货币交易所（DEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/hyperliquid.md)         |
| [Interactive Brokers](https://www.interactivebrokers.com)  | `INTERACTIVE_BROKERS` | 经纪商（多交易场所）   | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/interactive_brokers.md) |
| [Kraken](https://kraken.com)                               | `KRAKEN`              | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/kraken.md)              |
| [Lighter](https://lighter.xyz)                             | `LIGHTER`             | 加密货币交易所（DEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/lighter.md)             |
| [Lighter on Robinhood](https://robinhoodchain.lighter.xyz) | `LIGHTER_ROBINHOOD`   | 加密货币交易所（DEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/lighter.md)             |
| [OKX](https://okx.com)                                     | `OKX`                 | 加密货币交易所（CEX） | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/okx.md)                 |
| [Polymarket](https://polymarket.com)                       | `POLYMARKET`          | 预测市场（DEX）    | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/polymarket.md)          |
| [Tardis](https://tardis.dev)                               | `TARDIS`              | 加密货币数据提供商    | ![状态](https://img.shields.io/badge/stable-green) | [指南](docs/integrations/tardis.md)              |

- **ID**：集成适配器客户端的默认客户端 ID。
- **类型**：集成类型（通常为交易场所类型）。

对于 Lighter on Robinhood，应注册的交易场所和显式客户端 ID 为 `LIGHTER_ROBINHOOD`。
共享的 Lighter 工厂仍以 `LIGHTER` 作为兼容性默认值。

### 状态

- `planned`：计划在未来开发。
- `building`：正在开发，可能尚不可用。
- `beta`：已达到最低可用状态，正处于 Beta 测试阶段。
- `stable`：功能集和 API 已稳定，开发者和用户已对该集成进行了合理程度的测试（仍可能存在部分缺陷）。

更多详情请参阅[集成](https://nautilustrader.io/docs/latest/integrations/)文档。

## 路线图

[路线图](https://github.com/nautechsystems/nautilus_trader/blob/develop/ROADMAP.md)概述了 NautilusTrader 的战略方向。
目前的重点包括稳定 Rust 原生核心、改进文档以及提升代码易用性。

本开源项目专注于面向个人及小型团队量化交易者的单节点回测与实盘交易。
为保持对核心引擎和生态系统可持续性的专注，UI 仪表盘、分布式编排以及内置 AI/ML 工具不在项目范围内。

新的集成提案应先创建 RFC issue，讨论其适用性后再提交 PR。
有关准则，请参阅[社区贡献的集成](https://github.com/nautechsystems/nautilus_trader/blob/develop/ROADMAP.md#community-contributed-integrations)。

## 安全

[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/nautechsystems/nautilus_trader/badge)](https://scorecard.dev/viewer/?uri=github.com/nautechsystems/nautilus_trader)

安全是 NautilusTrader 项目的首要任务，我们重视每一位帮助发现和解决漏洞的贡献者。
我们在整个开发和发布生命周期中实施分层控制，包括签名发布、持续漏洞管理以及透明的开发实践：

- **源代码与审查控制**：CODEOWNERS 对关键基础设施、依赖清单和锁文件进行把关；受保护分支要求提交经过签名且 CI 通过；发布标签不可变；Rust 依赖仅从 crates.io 获取。
- **依赖引入**：锁文件使用加密校验和固定每个依赖；第三方 Python 包只能从 wheel 安装；采用新的依赖和工具版本前须经过发布冷却期；cargo-vet 审计 Rust 依赖来源；cargo-deny 根据与 NautilusTrader `LGPL-3.0-only` 许可证兼容的许可证允许列表检查 Rust 依赖。
- **扫描与模糊测试**：提交前运行 Gitleaks 密钥扫描和 Zizmor Actions 审计；CodeQL 针对提交到 `master` 的 PR 以及推送到 `nightly` 的代码运行；cargo-audit、cargo-deny、cargo-vet、OSV Scanner 和 pip-audit 针对与审计相关的 PR 并按日运行；cargo-fuzz 目标覆盖部分适配器和签名模块。
- **构建与发布完整性**：GitHub Actions 固定至具体提交 SHA；CI 运行器通过出口允许列表进行加固；Python 构件包含 SLSA 构建来源证明；容器镜像使用 Sigstore 签名，并附带经证明的 SPDX SBOM；PyPI 和 crates.io 发布采用 OIDC 可信发布机制，且只允许在永不运行 PR 或 fork 代码的受保护 `release` 环境中执行。
- **运行时加密**：TLS 和大多数运行时加密使用 [aws-lc-rs](https://github.com/aws/aws-lc-rs)，即 AWS-LC 的 Rust 绑定；Ed25519 签名使用 [ed25519-dalek](https://github.com/dalek-cryptography/curve25519-dalek)。

上方的 OpenSSF Scorecard 徽章是一个自动化的仓库健康度信号；它是人工审查、CI 加固和安全审计的补充，而不能替代这些措施。

### 报告漏洞

请通过 [GitHub Security Advisories](https://github.com/nautechsystems/nautilus_trader/security/advisories/new)
私下报告，或发送邮件至 <security@nautechsystems.io>（可应要求提供 PGP 密钥）。我们会在 48 小时内确认收到报告，
并在 30 天内修复严重漏洞。

严谨的漏洞报告需要投入大量时间和精力。我们对此深表感谢；除非您希望保持匿名，
否则我们会在相关安全公告和发布说明中致谢报告者。

[安全策略](SECURITY.md)详细说明了适用范围、协同披露和逐步发布验证流程。
[安全架构](docs/developer_guide/security.md)介绍了端到端的发布供应链。
完整策略请参阅[负责任披露](https://nautilustrader.io/security/responsible-disclosure/)和
[供应链安全](https://nautilustrader.io/security/supply-chain/)策略；CI/CD 安全记录在
[.github/OVERVIEW.md](.github/OVERVIEW.md#security) 中。

## 版本管理与发布

> [!WARNING]
>
> **NautilusTrader 仍在积极开发中**。部分功能可能尚未完成；尽管 API 日趋稳定，
> 不同版本之间仍可能发生破坏性变更。
> 我们会尽力在发布说明中记录这些变更，但不作绝对保证。

我们计划保持**每两周发布一次**，但实验性功能或规模较大的功能可能导致延期。

### 分支

我们致力于在所有分支上维持稳定且可通过构建的状态。

- `master`：对应最新发布版本的源代码，推荐用于生产环境。
- `nightly`：`develop` 分支的每日快照，用于早期测试；每天 **14:00 UTC** 及必要时合并。
- `develop`：供贡献者和功能开发使用的活跃开发分支。

> [!NOTE]
>
> v2 候选发布版本系列正逐步迈向 **2.x 版本的稳定 API**。
> 达到这一里程碑后，我们计划针对所有 API 变更实施正式的弃用流程。
> 这种方式使我们目前仍能保持快速的开发节奏。

## 精度模式

NautilusTrader 的核心值类型（`Price`、`Quantity`、`Money`）支持两种精度模式，
两者的内部位宽和最大小数精度不同。

- **高精度**：使用 128 位整数，最多支持 16 位小数，并具有更大的数值范围。
- **标准精度**：使用 64 位整数，最多支持 9 位小数，数值范围较小。

> [!NOTE]
>
> 默认情况下，官方 Python wheel 在所有支持的平台上均采用高精度（128 位）模式。
>
> 对于纯 Rust crate，由于 Rust 通过软件模拟处理 `i128`/`u128`，高精度模式可在所有平台
> （包括 Windows）上运行。默认采用标准精度模式，除非显式启用 `high-precision` 功能标志。

更多详情请参阅[安装指南](QUICKSTART-CN.md)。

**Rust 功能标志**：要在 Rust 中启用高精度模式，请在 `Cargo.toml` 中添加 `high-precision` 功能：

```toml
[dependencies]
nautilus_model = { version = "*", features = ["high-precision"] }
```

## 安装

我们建议使用受支持的最新 Python 版本，并在虚拟环境中安装
[nautilus_trader](https://pypi.org/project/nautilus_trader/)，以隔离依赖。

**支持以下两种安装方式**：

1. 从 PyPI 或 Nautech Systems 软件包索引安装预构建的二进制 wheel。
2. 从源代码构建。

> [!TIP]
>
> 强烈建议使用 [uv](https://docs.astral.sh/uv) 软件包管理器搭配“原生”CPython 进行安装。
>
> Conda 和其他 Python 发行版*可能*可以使用，但不在官方支持范围内。

### 从 PyPI 安装

使用 Python 的 pip 软件包管理器从 PyPI 安装最新的二进制 wheel（或 sdist 包）：

```bash
pip install -U nautilus_trader
```

要测试 PyPI 上的 v2 候选发布 wheel：

```bash
pip install -U nautilus_trader --pre
```

v2 候选发布 wheel 使用 `2.0.0rcN` 版本号，供社区在最终 `2.0.0` 发布前进行测试。
我们不建议在控制真实资金进行实盘交易等生产环境中使用候选发布版本。

使用 `visualization` extra 安装交互式业绩报告和图表所需的可选依赖：

```bash
pip install -U "nautilus_trader[visualization]"
```

详情请参阅[安装指南](QUICKSTART-CN.md)。

### 从 Nautech Systems 软件包索引安装

Nautech Systems 软件包索引（`packages.nautechsystems.io`）符合
[PEP-503](https://peps.python.org/pep-0503/)，托管 `nautilus_trader` 的稳定版和开发版二进制 wheel。
用户既可以安装最新稳定版本，也可以安装预发布版本进行测试。

#### 稳定版 wheel

稳定版 wheel 对应 PyPI 上 `nautilus_trader` 的正式发布，并使用标准版本号。

安装最新稳定版本：

```bash
pip install -U nautilus_trader --index-url=https://packages.nautechsystems.io/simple
```

> [!TIP]
>
> 如果希望 pip 自动回退到 PyPI，请使用 `--extra-index-url`，而不是 `--index-url`。

#### 开发版 wheel

主软件包索引会发布 `nightly` 和 `develop` 分支的 v2 开发版 wheel，
供用户在稳定版本发布前测试新功能和修复。

此流程还能节省计算资源，并让用户方便地获取 CI 流水线中测试过的同一二进制文件，
同时遵循 [PEP-440](https://peps.python.org/pep-0440/) 版本标准：

- `develop` wheel 使用版本后缀 `.devYYYYMMDD+run`。
- 当基础版本已经是预发布版本时，`nightly` wheel 使用 `.devYYYYMMDD`；否则使用 `aYYYYMMDD`。

| 平台                 | Develop | Nightly |
| :----------------- | :------ | :------ |
| `Linux (x86_64)`   | ✓       | ✓       |
| `Linux (ARM64)`    | -       | ✓       |
| `macOS (ARM64)`    | -       | ✓       |
| `Windows (x86_64)` | -       | ✓       |

> [!WARNING]
>
> 我们不建议在控制真实资金进行实盘交易等生产环境中使用开发版 wheel。

#### 安装命令

默认情况下，pip 会安装最新稳定版本。添加 `--pre` 标志后，才会考虑包括开发版 wheel 在内的预发布版本。

安装最新的可用预发布版本（包括开发版 wheel）：

```bash
pip install -U nautilus_trader --pre --index-url=https://packages.nautechsystems.io/simple
```

#### 可用版本

可以在[软件包索引](https://packages.nautechsystems.io/simple/nautilus-trader/index.html)中查看
`nautilus_trader` 的所有可用版本。

以编程方式获取并列出可用版本：

```bash
curl -s https://packages.nautechsystems.io/simple/nautilus-trader/index.html | sed -n 's/.*<a href="\([^"]*\)".*/\1/p' | awk -F'#' '{print $1}' | sort
```

> [!NOTE]
>
> 在 Linux 上，安装二进制 wheel 前请运行 `ldd --version` 确认 glibc 版本，并确保其为 **2.35** 或更高版本。

#### 分支更新

- `develop` 分支 wheel（`.devYYYYMMDD+run`）：每次合并提交后持续构建并发布。
- `nightly` 分支 wheel（`.devYYYYMMDD` 或 `aYYYYMMDD`）：每天 **14:00 UTC** 自动合并
  `develop` 分支时（如有变更）构建并发布。

#### 保留策略

- `develop` 分支 wheel：只保留最新一次 wheel 构建。
- `nightly` 分支 wheel：每个平台只保留最近 30 个发布日期的 wheel。

#### 验证构建来源

项目发布的所有构件均包含由 CI/CD 流水线生成的加密证明：

- Python wheel 和源码发行包（PyPI、GitHub Releases、Nautech Systems 软件包索引）：[SLSA](https://slsa.dev/) 构建来源证明。
- Docker 镜像（`ghcr.io/nautechsystems/nautilus_trader`、`ghcr.io/nautechsystems/jupyterlab`）：无密钥 [cosign](https://github.com/sigstore/cosign) 签名和 SPDX SBOM 证明。

这两类证明均通过 [Sigstore](https://www.sigstore.dev/) 签发并绑定至特定提交 SHA，
因此验证能够确认构件由 NautilusTrader 官方 GitHub Actions 工作流生成，且此后未被篡改。

有关逐步验证命令，请参阅 `SECURITY.md` 中的[验证发布](SECURITY.md#verifying-releases)。

> [!NOTE]
>
> 验证 Python 构件需要 [GitHub CLI](https://cli.github.com/)（`gh`），验证 Docker 镜像需要
> [cosign](https://github.com/sigstore/cosign)。
> `develop` 和 `nightly` 分支的开发版 wheel 同样带有证明。

### 从源代码安装

如果先安装 `pyproject.toml` 中指定的构建依赖，就可以使用 pip 从源代码安装。

1. 安装 [rustup](https://rustup.rs/)（Rust 工具链安装程序）：

   - Linux 和 macOS：

       ```bash
       curl https://sh.rustup.rs -sSf | sh
       ```

   - Windows：

     - 下载并安装 [`rustup-init.exe`](https://win.rustup.rs/x86_64)。
     - 使用 [Build Tools for Visual Studio 2022](https://visualstudio.microsoft.com/visual-cpp-build-tools/) 安装“使用 C++ 的桌面开发”。

   - 验证（适用于所有系统）：在终端会话中运行 `rustc --version`。

2. 在当前 shell 中启用 `cargo`：

   - Linux 和 macOS：

       ```bash
       source $HOME/.cargo/env
       ```

   - Windows：

     - 启动新的 PowerShell。

3. 安装 [clang](https://clang.llvm.org/)（LLVM 的 C 语言前端）：

   - Linux（同时安装 [lld](https://lld.llvm.org/)，作为 Rust 链接器以加快构建）：

       ```bash
       sudo apt-get install clang lld
       ```

   - macOS：

       ```bash
       xcode-select --install
       ```

   - Windows：

     1. 将 Clang 添加到 [Build Tools for Visual Studio 2022](https://visualstudio.microsoft.com/visual-cpp-build-tools/)：

        - 开始 | Visual Studio Installer | 修改 | C++ Clang tools for Windows (latest) = 勾选 | 修改。

     2. 在当前 shell 中启用 `clang`：

        ```powershell
        [System.Environment]::SetEnvironmentVariable('path', "C:\Program Files\Microsoft Visual Studio\2022\BuildTools\VC\Tools\Llvm\x64\bin\;" + $env:Path,"User")
        ```

   - 验证（适用于所有系统）：在终端会话中运行 `clang --version`。

4. 安装 uv（详情请参阅 [uv 安装指南](https://docs.astral.sh/uv/getting-started/installation)）：

   - Linux 和 macOS：

       ```bash
       curl -LsSf https://astral.sh/uv/install.sh | sh
       ```

   - Windows（PowerShell）：

       ```powershell
       irm https://astral.sh/uv/install.ps1 | iex
       ```

5. 使用 `git` 克隆源代码，然后从项目根目录同步依赖：

   ```bash
   git clone --branch develop --depth 1 https://github.com/nautechsystems/nautilus_trader
   cd nautilus_trader
   make sync
   ```

> [!NOTE]
>
> `--depth 1` 标志只获取最新提交，以更快、更轻量地完成克隆。

6. 设置 PyO3 编译所需的环境变量（仅限 Linux 和 macOS）。运行 `make sync` 后，
   从仓库根目录执行以下命令：

   ```bash
   # 设置 PyO3 使用的 Python 可执行文件路径
   export PYO3_PYTHON="$PWD/.venv/bin/python"

   # 仅限 Linux：设置 uv 管理的 Python 运行时库路径
   PYTHON_LIB_DIR="$("$PYO3_PYTHON" -c 'import sysconfig; print(sysconfig.get_config_var("LIBDIR"))')"
   export LD_LIBRARY_PATH="$PYTHON_LIB_DIR${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}"

   # 使用 uv 安装的 Python 运行 Rust 测试时需要此变量
   export PYTHONHOME="$("$PYO3_PYTHON" -c 'import sys; print(sys.base_prefix)')"
   ```

> [!NOTE]
>
> `LD_LIBRARY_PATH` 导出仅适用于 Linux，macOS 不需要。
>
> 使用 `uv` 安装的 Python 运行 `make cargo-test` 时需要设置 `PYTHONHOME` 变量。
> 否则，依赖 PyO3 的测试可能无法找到 Python 运行时。

7. 以 release 模式构建并安装 NautilusTrader：

   ```bash
   make build
   ```

有关其他选项和更多详情，请参阅[安装指南](QUICKSTART-CN.md)。

## Redis

在 NautilusTrader 中使用 [Redis](https://redis.io) 是**可选的**，仅当 Redis 被配置为
[缓存](https://nautilustrader.io/docs/latest/concepts/cache)数据库或
[消息总线](https://nautilustrader.io/docs/latest/concepts/message_bus)的后端时才需要。
更多详情请参阅[安装指南](https://nautilustrader.io/docs/latest/getting_started/installation#redis)中的 **Redis** 章节。

## Makefile

项目提供了 `Makefile`，用于自动执行开发过程中的大多数安装和构建任务。部分目标如下：

- `make install`：以 `release` 构建模式安装，并包含所有依赖组和 extra。
- `make install-debug`：与 `make install` 相同，但使用 `debug` 构建模式。
- `make sync`：安装 Python 依赖，但不构建软件包。
- `make build`：以 `release` 模式构建并安装软件包（默认）。
- `make build-debug`：以 `debug` 模式构建并安装软件包。
- `make build-wheel`：以 `release` 模式构建 wheel。
- `make cargo-test`：使用 `cargo-nextest` 运行所有 Rust crate 测试。
- `make clean`：删除构建构件、缓存和构建目录。
- `make distclean`：**注意**，使用 `FORCE=1` 运行时会删除所有不在 git 索引中的构件，
  包括尚未执行 `git add` 的源文件。
- `make docs`：使用 Sphinx 构建 Python 文档，并使用 Cargo 构建 Rust 文档。
- `make pre-commit`：对所有文件运行提交前检查。
- `make ruff`：使用 `python/pyproject.toml` 中的配置对所有文件运行 Ruff（并自动修复）。
- `make pytest`：使用 `pytest` 运行所有测试。
- `make cargo-ci-benches`：构建 CI 使用的 Rust 基准测试。
- `make cargo-codspeed-build`：构建用于 CodSpeed 比较的 Rust 基准测试子集。

> [!TIP]
>
> 运行 `make help` 可查看所有可用 make 目标的文档。

> [!TIP]
>
> 有关如何运行基础设施集成测试，请参阅
> [crates/infrastructure/TESTS.md](https://github.com/nautechsystems/nautilus_trader/blob/develop/crates/infrastructure/TESTS.md)。

## 示例

指标和策略均可使用 Python 或 Rust 开发。对于性能和延迟敏感型应用，我们推荐使用 Rust。以下是一些示例：

- 通过 PyO3 暴露的[指标](https://github.com/nautechsystems/nautilus_trader/tree/develop/python/nautilus_trader/indicators/)实现。
- 直接使用 `BacktestEngine` 的[回测](https://github.com/nautechsystems/nautilus_trader/tree/develop/examples/backtest/)示例。
- 使用 Rust 编写的 [EMA 交叉回测](https://github.com/nautechsystems/nautilus_trader/blob/develop/crates/backtest/examples/engine_ema_cross.rs)示例。

## Docker

Docker 容器使用以下变体标签构建：

- `nautilus_trader:latest`：安装了最新发布版本。
- `nautilus_trader:nightly`：安装了 `nightly` 分支的最新代码。
- `jupyterlab:latest`：安装了最新发布版本、`jupyterlab`，并包含一个带配套数据的回测示例 notebook。
- `jupyterlab:nightly`：安装了 `nightly` 分支的最新代码、`jupyterlab`，并包含一个带配套数据的回测示例 notebook。

可以使用以下命令拉取容器镜像：

```bash
docker pull ghcr.io/nautechsystems/<image_variant_tag> --platform linux/amd64
```

可以运行以下命令启动回测示例容器：

```bash
docker pull ghcr.io/nautechsystems/jupyterlab:nightly --platform linux/amd64
docker run -p 8888:8888 ghcr.io/nautechsystems/jupyterlab:nightly
```

然后在浏览器中打开以下地址：

```bash
http://127.0.0.1:8888/lab
```

> [!WARNING]
>
> 示例使用 `log_level="ERROR"`，因为 Nautilus 的日志输出会超过 Jupyter 的 stdout 速率限制，
> 较低的日志级别会导致 notebook 卡住。

## 开发

我们致力于为这个 Rust 与 Python 混合代码库提供尽可能舒适的开发体验。
有关实用信息，请参阅[开发者指南](https://nautilustrader.io/docs/latest/developer_guide/)。

[Nautilus Engineering](https://github.com/nautechsystems/nautilus_engineering) 维护 Nautilus 项目之间共享的
工程标准、lint 配置、提交前检查定义、工具版本固定和仓库检查。
本仓库引入了其中部分文件，所用修订版本记录在 `.nautilus-engineering.lock` 中；
项目特有的策略和 CI 配置仍保留在本仓库内。

> [!TIP]
>
> 修改 Rust 代码后运行 `make build-debug` 进行编译，可获得最高效的开发工作流。

修改 PyO3 绑定、存根注解或封装的 Rust 文档后，请从仓库根目录重新生成 Python 构件：

```bash
make py-stubs
```

请提交该目标所修改的生成 `.pyi` 文件和 PyO3 包装器文档注释。
详情请参阅[生成的 Python 构件](docs/developer_guide/rust.md#generated-python-artifacts)。

### 使用 Rust 进行测试

[cargo-nextest](https://nexte.st) 是 NautilusTrader 的标准 Rust 测试运行器。
其主要优势是让每项测试在独立进程中运行，避免相互干扰，从而保证测试可靠性。
有关完整测试套件支持以及普通 `cargo test` 的局限，请参阅
[Rust 测试指南](docs/developer_guide/testing.md#rust-tests)。

可以运行以下命令安装 cargo-nextest：

```bash
cargo install cargo-nextest
```

> [!TIP]
>
> 使用 `make cargo-test` 运行 Rust 测试；该命令通过高效配置使用 **cargo-nextest**。

## 贡献

感谢您考虑为 NautilusTrader 贡献代码。我们欢迎能够改进项目的高质量贡献。
在开始重大变更前，请先创建 [issue](https://github.com/nautechsystems/nautilus_trader/issues)，
与团队讨论问题和方案。小型、独立的修复无需事先获得同意。

开始前，请务必查看项目路线图中说明的[开源范围](https://github.com/nautechsystems/nautilus_trader/blob/develop/ROADMAP.md#open-source-scope)，
了解项目范围内和范围外的内容。

准备开始贡献后，请遵循 [CONTRIBUTING.md](https://github.com/nautechsystems/nautilus_trader/blob/develop/CONTRIBUTING.md)
中的准则。其中包括签署贡献者许可协议（CLA），以确保您的贡献可以纳入项目。

> [!NOTE]
>
> Pull request 应以 `develop` 分支（默认分支）为目标。新功能和改进会先集成到该分支，再进行发布。

再次感谢您对 NautilusTrader 的关注！我们期待审查您的贡献，并与您携手改进本项目。

## 社区

欢迎加入由用户和贡献者组成的 [Discord](https://discord.gg/NautilusTrader) 社区，参与交流并及时了解
NautilusTrader 的最新公告和功能。无论您是希望参与贡献的开发者，还是只想进一步了解平台，
我们的 Discord 服务器都欢迎您的加入。

> [!WARNING]
>
> NautilusTrader 不发行、推广或认可任何加密货币代币。任何与此相反的声明或传播均未经授权且不属实。
>
> NautilusTrader 的所有官方更新和信息仅会通过 <https://nautilustrader.io>、我们的
> [GitHub](https://github.com/nautechsystems)、[Discord 服务器](https://discord.gg/NautilusTrader)，
> 或经过验证的 X（Twitter）账号 [@NautilusTrader](https://x.com/NautilusTrader) 发布。
>
> 如果发现任何可疑活动，请向相应平台举报，并通过 <info@nautechsystems.io> 联系我们。

## 许可证

NautilusTrader 的源代码在 GitHub 上依据
[GNU 宽通用公共许可证 v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html)提供。
我们欢迎项目贡献；贡献者须完成标准的
[贡献者许可协议（CLA）](https://github.com/nautechsystems/nautilus_trader/blob/develop/CLA.md)。

---

NautilusTrader™ 由 Nautech Systems 开发和维护。Nautech Systems 是一家专注于开发高性能交易系统的科技公司。
更多信息请访问 <https://nautilustrader.io>。

使用本软件须遵守[免责声明](https://nautilustrader.io/legal/disclaimer/)。

© 2015-2026 Nautech Systems Pty Ltd. 保留所有权利。

![nautechsystems](https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/ns-logo.png "nautechsystems")
<img src="https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/ferris.png" alt="Ferris" width="128">
