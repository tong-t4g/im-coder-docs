# IM-Coder

IM-Coder 让手机聊天成为创世工具 -- 连接 Claude Code / Codex，用想象力构建世界。
目前已支持微信，很快会支持：飞书、QQ、Telegram、Discord 等应用

[English](README.md)

---

## 功能特性

- **主流平台** — 支持 Claude Code / Codex。支持微信，随后会支持飞书、QQ、Telegram、Discord 等主流应用。支持 MacOS/Windows/Linux
- **轻量化** — 无需升级手机系统版本和安装 APP 即可操作 Coding Agent
- **Skill** — 支持在 Coding Agent 中使用对话进行引导式安装、配置及所有其它操作
- **CLI** — 支持安装、启动、关闭、激活、状态、日志、诊断等 CLI 命令
- **权限控制** — 可通过文本 `/perm` 命令或快捷 `1/2/3` 批准工具调用
- **会话管理** — 对话在 Coding Agent 本地保存，支持重启保留会话、新建会话、恢复会话
- **隐私安全** — 无遥测，无监控，敏感信息自动脱敏，不接触任何 Coding Agent 账号信息
- **可靠投递** — 按平台限制自动分块、指数退避重试、HTML 降级、消息去重

## 前置要求

- **Node.js >= 20**
- **Claude Code CLI / Codex CLI** — 已安装并可使用

## 安装

### 1. 安装/更新 IM-Coder CLI

```bash
npm install -g @tong-t4g/im-coder
```

### 2. 安装/更新 Skill

如果你使用 Claude Code：

```bash
im-coder install claude
```

如果你使用 Codex：

```bash
im-coder install codex
```

如果两个都在用：

```bash
im-coder install auto
```

更新 IM-Coder 时会保留配置、日志、消息历史、微信绑定等数据。

## 激活

在命令行使用

```bash
im-coder machine-id
```

获取机器ID，然后向客服申请激活码（微信号：13758206010，费用 9 元）
之后使用

```bash
im-coder activate <licenseKey>
```
来完成激活

## 配置

安装完成后，即可在 Claude Code / Codex 会话中使用

```text
使用自然语言：帮我配置微信桥接 或 /im-coder setup
```

进行引导式配置

## 启动

可在命令行使用：

```text
im-coder start
```

来启动 IM-Coder 守护进程。**守护进程不受 IM 影响，聊天窗口重开仍会保持原会话。推荐将守护进程设置为操作系统自启动。**

## 对话

打开IM 应用发送消息，Claude Code / Codex 会通过 IM-Coder 回复。

当 Claude Code / Codex 需要使用工具（网络请求、运行命令等）时，聊天中会提醒使用 `/perm` 命令 或  快捷 `1/2/3` 回复（适用于飞书/QQ/微信，只需复制提醒中的指令发送即可）。

## IM-Coder 命令列表

以下为常见操作在命令行、Claude Code、Codex 中的对应方式：

| CLI | Claude Code 命令 | Codex /自然语言                                          | 说明 |
|---|------------|------------------------------------------------------|---|
| `-` | `/im-coder setup` | "im-coder setup" / "配置微信桥接"                          | 交互式配置向导 |
| `im-coder start` | `/im-coder start` | "start bridge" / "启动IM-Coder"                        | 启动桥接守护进程 |
| `im-coder stop` | `/im-coder stop` | "stop bridge" / "停止IM-Coder"                         | 停止守护进程 |
| `im-coder status` | `/im-coder status` | "bridge status" / "查看IM-Coder状态"                     | 查看运行状态 |
| `im-coder logs` | `/im-coder logs` | "查看IM-Coder日志"                                       | 查看最近 50 行日志 |
| `im-coder logs 200` | `/im-coder logs 200` | "logs 200"                                           | 查看最近 200 行日志 |
| `-` | `/im-coder reconfigure` | "reconfigure" / "修改IM-Coder配置"                       | 交互式修改配置 |
| `im-coder doctor` | `/im-coder doctor` | "doctor" / "诊断IM-Coder"                              | 故障诊断 |
| `im-coder machine-id` | `/im-coder machine-id` | "machine-id" / "获取机器ID"                              | 获取机器ID |
| `im-coder activate <licenseKey>` | `/im-coder activate <licenseKey>` | "activate <licenseKey>" / "IM-Coder 激活 <licenseKey>" | 激活 |

其中 `setup` 与 `reconfigure` 当前主要通过 Skill 交互式执行，CLI 暂不提供对应子命令。

## 聊天窗口命令

在 IM 中，可以通过以下斜杠指令控制 Coding Agent 会话：  

### 会话管理

- `/new [路径]` — 开始一个新会话，可选传入绝对路径作为工作目录，如 `/new /home/user/project`，如无参数则在当前路径新建会话
- `/bind <session_id>` — 绑定到已有会话，session_id 为 32-64 位十六进制字符串
- `/stop` — 中止当前正在运行的任务
- `/sessions` — 列出最近的会话（最多显示 10 条）
- `/status` — 查看当前会话状态（Session ID、工作目录、模式、模型）

### 工作环境

- `/cwd /路径` — 修改当前会话的工作目录，需传入绝对路径
- `/mode plan|code|ask` — 切换工作模式：plan（规划）、code（编码）、ask（问答）

### 权限审批

当 Claude 请求执行某项操作时，需要手动裁决：

- `/perm allow <id>` — 永久允许该权限请求
- `/perm allow_session <id>` — 本会话内允许
- `/perm deny <id>` — 拒绝该权限请求
- `1` / `2` / `3` — 飞书、QQ、微信专用快捷回复：1 允许，2 本次允许，3 拒绝（仅限当前只有一条待处理权限时有效）

### 其他

- `/help` — 显示所有可用指令

## 常见问题
- 整个安装配置过程均可在 Coding Agent 中通过对话完成，体验丝滑
- 在 Codex 中使用 IM-Coder 命令，无需输入“/”
- 在配置过程中，一直微信扫码失败的话，基本就是因为微信版本较低，无法使用 ClawBot 插件，升级微信即可
- `IMC_WEIXIN_MEDIA_ENABLED` 只控制图片 / 文件 / 视频的入站下载
- 语音消息只使用微信自带的语音转文字结果
- 如果微信没有提供 `voice_item.text`数据，桥会直接报错，不会自行下载或转写原始语音
- 权限确认使用消息提醒中的文本 `/perm ...` 命令或快捷 `1/2/3` 回复

## 许可与商业授权

本项目不是开源项目。源代码默认不公开，未获书面授权的第三方不得复制、修改、分发、转售、再许可、托管部署或以任何形式向第三方提供本项目。

如需评估、部署、商业使用、私有化交付、`OEM` / 白标、渠道分销或其他例外授权，请联系维护者签署单独的商业协议。

详见 [LICENSE](LICENSE) 与 [LICENSING.md](LICENSING.md)。
