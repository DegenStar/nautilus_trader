# NautilusTrader 极简实盘安装与使用指南

本指南面向准备使用 Python 部署实盘策略的用户，介绍从安装到首次上线的最短安全路径。
交易所适配器和策略各不相同，具体参数必须以对应版本的官方文档为准。

> [!WARNING]
>
> 实盘交易会产生真实订单并可能造成资金损失。不要把示例策略直接用于生产环境，
> 也不要在尚未确认交易场所、账户、合约、方向和数量时连接实盘执行客户端。

## 1. 上线前准备

开始前至少确认以下事项：

- 策略已经完成回测，并考虑手续费、滑点、延迟、断线和拒单。
- 已在交易所测试网或模拟环境验证行情、下单、撤单和持仓同步。
- 使用独立的实盘子账户，并只存入首次验证所需的最低资金。
- API 密钥只授予读取和交易权限，关闭提币权限，并尽可能设置 IP 白名单。
- 已确定最大订单量、最大持仓和人工停机条件。
- 服务器时间保持同步，网络、磁盘、日志和告警可以持续监控。

## 2. 安装运行环境

官方支持 Python 3.12 至 3.14，以及 64 位 Linux、Apple 芯片版 macOS 和 64 位 Windows。
Linux 的 glibc 必须为 2.35 或更高版本，可运行 `ldd --version` 查看。

- 安装环境依赖：

**🖥️ macOS 或 Linux：**

```bash
curl -fsSL https://gitlab.com/nautilustrader/scripts/raw/main/install.sh | bash
```

**🖥️ Windows PowerShell（以管理员身份运行）：**

```powershell
iwr -useb https://gitlab.com/nautilustrader/scripts/raw/main/install.ps1 | iex
```

在 NautilusTrader 源代码目录之外创建实盘项目目录：

```bash
mkdir nautilus-live
cd nautilus-live
uv venv --python 3.14
uv pip install nautilus_trader
```

以上命令安装 PyPI 最新稳定版。生产环境不要使用 `--pre` 安装候选版或开发版。

- 验证安装：

**🖥️ macOS 或 Linux：**

```bash
.venv/bin/python -c "import nautilus_trader; print(nautilus_trader.__version__)"
```

**🖥️ Windows PowerShell：**

```powershell
.\.venv\Scripts\python.exe -c "import nautilus_trader; print(nautilus_trader.__version__)"
```

记录输出的版本号。后续文档、配置和示例必须与该版本一致，不要混用 `develop` 分支示例与稳定版软件包。

## 3. 选择交易场所适配器

在[集成文档](https://nautilustrader.io/docs/latest/integrations/)中找到你的交易场所，确认：

- 现货、期货、期权等产品是否受支持。
- 实盘、测试网或模拟环境的配置枚举。
- API 密钥所需的环境变量名称。
- `InstrumentId`、`AccountId`、保证金模式和持仓模式的格式。
- 行情和执行客户端支持的订单类型与限制。

不要把 API 密钥直接写入 Python 文件，也不要提交包含密钥的 `.env` 文件。

## 4. 先验证行情连接

从与安装版本匹配的[实盘示例目录](https://github.com/nautechsystems/nautilus_trader/tree/master/examples/live)
取得对应交易场所的 `data_tester.py`，并在运行前阅读文件顶部说明和模块级配置。

重点确认脚本只订阅行情、不发送订单，并检查环境和交易工具。然后运行：

macOS 或 Linux：

```bash
.venv/bin/python data_tester.py
```

Windows PowerShell：

```powershell
.\.venv\Scripts\python.exe data_tester.py
```

确认可以持续收到预期交易工具的行情，日志中没有重复断线、订阅失败或时间异常。
按 `Ctrl+C` 停止，并确认程序完成正常断开。

## 5. 在测试网验证执行

对应交易场所的 `exec_tester.py` 仅用于诊断连接和订单流程，不是生产策略。

> [!CAUTION]
>
> 多数 `exec_tester.py` 默认设置 `DRY_RUN = False`，启动后会立即发送订单；如果配置为实盘环境，
> 就会使用真实资金。此类脚本通常还会设置 `LiveRiskEngineConfig(bypass=True)`，不得直接作为生产模板。

运行执行测试前：

1. 确认交易场所环境是测试网或模拟环境。
1. 将脚本中的 `DRY_RUN` 改为 `True`，先只验证认证和连接。
1. 检查 `InstrumentId`、`AccountId`、订单方向和订单数量。
1. 按适配器文档设置测试网 API 环境变量。
1. 确认 `DRY_RUN = True` 时无订单产生，再使用测试网最小允许数量验证下单和撤单。

每次测试结束后，都要登录交易所页面核对未成交订单、持仓和账户余额，不能只依赖本地日志。

## 6. 准备生产节点

实盘程序应使用经过审查的独立 Python 脚本或服务，不要使用 Jupyter Notebook。
生产节点至少应满足以下要求：

- 每个进程只运行一个 `LiveNode`；多个策略可以加入同一节点，其他节点使用独立进程。
- 为每个节点设置唯一的 `TraderId` 后缀，避免同一账户上的订单和持仓 ID 冲突。
- 保持启动对账开启，在策略启动前同步交易所订单和持仓状态。
- 启用风险引擎，不要使用 `LiveRiskEngineConfig(bypass=True)`。
- 根据恢复要求，按文档配置 Redis 或 Postgres 缓存持久化；策略状态持久化需要 Redis，
  且状态保存不是连续检查点。
- 策略回调中不要执行同步网络请求、重计算或其他阻塞操作。
- 日志必须包含连接、拒单、成交、持仓、对账和关闭过程，并由外部系统监控。

节点的基本组成顺序如下：

1. 创建 `LiveNode` 并配置风险引擎和执行引擎。
1. 注册行情客户端和执行客户端。
1. 添加经过回测的策略。
1. 构建节点并完成启动对账。
1. 调用 `node.run()` 进入事件循环。
1. 停止后调用 `node.dispose()` 释放资源并刷新状态。

具体配置请参阅[实盘节点配置](https://nautilustrader.io/docs/latest/how_to/configure_live_trading)。

## 7. 首次实盘上线

首次连接实盘环境时采用以下顺序：

1. 使用专用子账户和最低资金，只连接行情客户端。
1. 加入执行客户端，但保持策略不发送订单。
1. 核对账户、余额、持仓、交易工具和市场状态。
1. 使用交易所允许的最小数量发送一笔可识别的测试订单。
1. 验证下单、成交、撤单、持仓更新和启动对账均符合预期。
1. 逐步增加运行时间和资金，不要首次启动就使用完整仓位。

上线前再次确认代码中的环境确实是目标实盘环境，并检查所有数量和价格单位。
合约数量不一定等于标的资产数量。

## 8. 启动、停止与故障处理

- 首次运行时保持前台启动并观察完整日志，稳定后再交给 systemd、容器平台等进程管理器。
- 使用 `Ctrl+C` 或 `SIGTERM` 请求正常停止，等待节点完成清理；不要直接使用 `SIGKILL`。
- 停止后在交易所侧核对订单和持仓。策略的停止流程不等于交易所一定没有剩余风险敞口。
- 节点异常退出后不要立即盲目重启。先检查交易所状态和日志，再依靠启动对账恢复。
- 保留独立于交易程序的人工撤单和平仓通道，并定期演练。

## 上线检查清单

- [ ] 安装版本、文档和示例版本一致。
- [ ] 目标交易场所、实盘环境、账户和交易工具正确。
- [ ] API 密钥无提币权限，且未写入代码或 Git。
- [ ] `TraderId` 后缀在同一账户的所有节点间唯一。
- [ ] 风险引擎和启动对账已启用。
- [ ] 订单量、持仓上限和停机条件已确认。
- [ ] 测试网已完成行情、下单、撤单和持仓验证。
- [ ] 日志、告警、时间同步和人工应急通道可用。
- [ ] 首次实盘测试只使用最低资金和最小订单量。

更多信息请参阅[中文 README](README-CN.md)和
[实盘交易概念](https://nautilustrader.io/docs/latest/concepts/live)。
