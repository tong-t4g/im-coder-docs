# IM-Coder

IM-Coder turns mobile chat into a creation tool — connect Claude Code / Codex and build worlds with your imagination.
WeChat is currently supported, with Feishu, QQ, Telegram, Discord and more coming soon.

[中文文档](README_CN.md)

---

## Features

- **Major Platforms** — Supports Claude Code / Codex. Supports WeChat, with Feishu, QQ, Telegram, Discord and more coming soon. Supports MacOS/Windows/Linux
- **Lightweight** — Operate Coding Agent without upgrading your phone OS or installing any app
- **Skill** — Use conversational guided installation, configuration, and all other operations within the Coding Agent
- **CLI** — Supports CLI commands for install, start, stop, activate, status, logs, diagnostics, and more
- **Permission Control** — Approve tool calls via text `/perm` commands or quick `1/2/3` replies
- **Session Management** — Conversations are saved locally in the Coding Agent, supporting session persistence across restarts, new sessions, and session recovery
- **Privacy & Security** — No telemetry, no monitoring, sensitive information is automatically redacted, no access to any Coding Agent account credentials
- **Reliable Delivery** — Automatic chunking per platform limits, exponential backoff retries, HTML fallback, message deduplication

## Prerequisites

- **Node.js >= 20**
- **Claude Code CLI / Codex CLI** — Installed and ready to use

## Installation

### 1. Install/Update IM-Coder CLI

```bash
npm install -g @tong-t4g/im-coder
```

### 2. Install/Update Skill

If you use Claude Code:

```bash
im-coder install claude
```

If you use Codex:

```bash
im-coder install codex
```

If you use both:

```bash
im-coder install auto
```

Updating IM-Coder preserves configuration, logs, message history, WeChat bindings, and other data.

## Activation

Run the following in the command line:

```bash
im-coder machine-id
```

to get your Machine ID, then contact customer service to request an activation code (WeChat: 13758206010, fee: ¥9).
Then use:

```bash
im-coder activate <licenseKey>
```

to complete activation.

## Configuration

After installation, you can use the following in a Claude Code / Codex session:

```text
Use natural language: help me configure WeChat bridge, or /im-coder setup
```

for guided configuration.

## Starting

Run in the command line:

```text
im-coder start
```

to start the IM-Coder daemon. **The daemon is independent of IM — reopening the chat window will maintain the original session. It is recommended to set the daemon to auto-start with your OS.**

## Chatting

Open the IM app and send a message. Claude Code / Codex will reply through IM-Coder.

When Claude Code / Codex needs to use a tool (network requests, running commands, etc.), the chat will prompt you to reply with the `/perm` command or quick `1/2/3` (for Feishu/QQ/WeChat, simply copy and send the command from the prompt).

## IM-Coder Command List

Below are the corresponding methods for common operations in CLI, Claude Code, and Codex:

| CLI | Claude Code Command | Codex / Natural Language | Description |
|---|---|---|---|
| `-` | `/im-coder setup` | "im-coder setup" / "configure WeChat bridge" | Interactive setup wizard |
| `im-coder start` | `/im-coder start` | "start bridge" / "start IM-Coder" | Start the bridge daemon |
| `im-coder stop` | `/im-coder stop` | "stop bridge" / "stop IM-Coder" | Stop the daemon |
| `im-coder status` | `/im-coder status` | "bridge status" / "check IM-Coder status" | View running status |
| `im-coder logs` | `/im-coder logs` | "view IM-Coder logs" | View last 50 log lines |
| `im-coder logs 200` | `/im-coder logs 200` | "logs 200" | View last 200 log lines |
| `-` | `/im-coder reconfigure` | "reconfigure" / "modify IM-Coder config" | Interactively modify config |
| `im-coder doctor` | `/im-coder doctor` | "doctor" / "diagnose IM-Coder" | Run diagnostics |
| `im-coder machine-id` | `/im-coder machine-id` | "machine-id" / "get machine ID" | Get Machine ID |
| `im-coder activate <licenseKey>` | `/im-coder activate <licenseKey>` | "activate <licenseKey>" / "IM-Coder activate <licenseKey>" | Activate |

Note: `setup` and `reconfigure` are currently executed interactively via Skill; corresponding CLI subcommands are not yet available.

## Chat Window Commands

In the IM app, you can control the Coding Agent session with the following slash commands:

### Session Management

- `/new [path]` — Start a new session, optionally passing an absolute path as the working directory, e.g., `/new /home/user/project`. If no parameter is given, a new session is created in the current path
- `/bind <session_id>` — Bind to an existing session; session_id is a 32-64 character hexadecimal string
- `/stop` — Abort the currently running task
- `/sessions` — List recent sessions (up to 10)
- `/status` — View current session status (Session ID, working directory, mode, model)

### Working Environment

- `/cwd /path` — Change the working directory of the current session; requires an absolute path
- `/mode plan|code|ask` — Switch working mode: plan, code, or ask

### Permission Approval

When Claude requests to perform an action, manual approval is required:

- `/perm allow <id>` — Permanently allow this permission request
- `/perm allow_session <id>` — Allow for the current session only
- `/perm deny <id>` — Deny this permission request
- `1` / `2` / `3` — Quick replies for Feishu, QQ, and WeChat: 1 = allow, 2 = allow for this session, 3 = deny (only effective when there is exactly one pending permission request)

### Other

- `/help` — Show all available commands

## FAQ

- The entire installation and configuration process can be completed through conversation within the Coding Agent for a smooth experience
- When using IM-Coder commands in Codex, no "/" prefix is needed
- If WeChat QR code scanning keeps failing during configuration, it is most likely because the WeChat version is too old to use the ClawBot plugin — simply upgrade WeChat
- `IMC_WEIXIN_MEDIA_ENABLED` only controls inbound downloads of images / files / videos
- Voice messages only use WeChat's built-in speech-to-text result
- If WeChat does not provide `voice_item.text` data, the bridge will return an error directly without downloading or transcribing the raw voice audio
- Permission approval uses the text `/perm ...` command or quick `1/2/3` replies from the message prompt

## License & Commercial Licensing

This project is not open source. The source code is not publicly available, and no third party may copy, modify, distribute, resell, sublicense, host, deploy, or make this project available to others in any form without prior written authorization.

For evaluation, deployment, commercial use, private delivery, `OEM` / white-label rights, channel distribution, or any other exception, please contact the maintainer for a separate commercial agreement.

See [LICENSE](LICENSE) and [LICENSING.md](LICENSING.md).
