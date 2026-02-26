# OpenClaw Command Cheat Sheet

Quick reference for OpenClaw CLI commands. Use `openclaw <command> --help` for full options.

---

## Quick Start (Daily Use)

| # | Command | Use when |
|---|---------|----------|
| 1 | `openclaw status` | Check if OpenClaw is running |
| 2 | `openclaw health` | Verify services are healthy |
| 3 | `openclaw gateway status` | See gateway state |
| 4 | `openclaw logs --follow` | Tail logs for debugging |
| 5 | `openclaw message send --channel telegram --target <id> --message "..."` | Send a quick message |
| 6 | `openclaw cron list` | List scheduled jobs |
| 7 | `openclaw models status --plain` | Check model availability |
| 8 | `openclaw sessions` | List active sessions |
| 9 | `openclaw security audit` | Run security check |
| 10 | `openclaw help` | List all commands |

---

## 1. Core Status & Health

| Command | Purpose |
|---------|---------|
| `openclaw status` | Show overall OpenClaw status (running/stopped, key services). |
| `openclaw status --deep` | Detailed status including sub-services and dependencies. |
| `openclaw health` | Run health checks and report readiness. |
| `openclaw logs --follow` | Stream logs (like `tail -f`) for live debugging. |

---

## 2. Gateway Control

| Command | Purpose |
|---------|---------|
| `openclaw gateway status` | Show gateway state (listening, ports, connections). |
| `openclaw gateway start` | Start the gateway service. |
| `openclaw gateway stop` | Stop the gateway service. |
| `openclaw gateway restart` | Restart the gateway (stop then start). |

---

## 3. Messaging

| Command | Purpose |
|---------|---------|
| `openclaw message send --channel whatsapp --target +447... --message "Hi"` | Send a WhatsApp message to the given number. |
| `openclaw message send --channel telegram --target 123456 --message "Hi"` | Send a Telegram message to the given user/chat ID. |
| `openclaw agent --to +447... --message "Run summary" --deliver` | Run an agent task for the target and deliver the result (e.g. to that contact). |

---

## 4. Cron & Automation

| Command | Purpose |
|---------|---------|
| `openclaw cron list` | List all cron jobs (IDs, schedule, agent, message). |
| `openclaw cron add --name ... --cron "0 7 * * *" --tz "Europe/London" --agent main --session isolated --message "..." --announce --channel telegram --to 7306010543` | Add a scheduled job: name, cron expression, timezone, agent, session type, message, and optional announce (e.g. Telegram). |
| `openclaw cron run <job-id> --expect-final` | Run a cron job by ID once and wait for completion. |
| `openclaw cron runs --id <job-id> --limit 10` | List recent runs for a job (last N runs). |
| `openclaw cron edit <job-id> ...` | Edit an existing cron job (schedule, message, etc.). |
| `openclaw cron rm <job-id>` | Remove a cron job by ID. |

---

## 5. Models

| Command | Purpose |
|---------|---------|
| `openclaw models status --plain` | Show model status in plain text (available, default, etc.). |
| `openclaw models aliases list` | List configured model aliases. |
| `openclaw models aliases add codex openai-codex/gpt-5.3-codex` | Add alias `codex` for model `openai-codex/gpt-5.3-codex`. |
| `openclaw models set codex` | Set the active/default model (by name or alias). |
| `openclaw models fallbacks list` | List fallback models. |
| `openclaw models fallbacks add GPT` | Add a model to the fallback list. |

---

## 6. Config

| Command | Purpose |
|---------|---------|
| `openclaw config get <path>` | Get config value at dot-path (e.g. `gateway.port`). |
| `openclaw config set <path> <value>` | Set config value at path. |
| `openclaw config unset <path>` | Remove config value at path. |

---

## 7. Security & Updates

| Command | Purpose |
|---------|---------|
| `openclaw security audit` | Run security audit (permissions, secrets, config). |
| `openclaw security audit --deep` | Deeper audit (extra checks). |
| `openclaw security audit --fix` | Run audit and apply safe fixes where possible. |
| `openclaw update status` | Check if OpenClaw or dependencies have updates. |
| `openclaw update` | Apply available updates. |

---

## 8. Sessions & Diagnostics

| Command | Purpose |
|---------|---------|
| `openclaw sessions` | List active sessions (human-readable). |
| `openclaw sessions --json` | List sessions in JSON for scripting. |
| `openclaw sessions --active 120` | Show sessions active in the last 120 seconds (or other window). |

---

## 9. Directory / ID Lookup

| Command | Purpose |
|---------|---------|
| `openclaw directory self --channel telegram` | Show this bot's identity (e.g. Telegram ID) for the channel. |
| `openclaw directory peers list --channel telegram` | List known peers (users/chats) on Telegram. |
| `openclaw directory groups list --channel discord` | List known groups on Discord (or other channel). |

---

## 10. Help Commands

| Command | Purpose |
|---------|---------|
| `openclaw help` | Show top-level help and list of commands. |
| `openclaw <command> --help` | Show help for a specific command (e.g. `openclaw cron --help`). |

---

## Tips & related utils

| What | Command or tip |
|------|-----------------|
| Version | `openclaw --version` or `openclaw version` (if available) — confirm CLI version. |
| Logs to file | `openclaw logs --follow > openclaw.log` — capture tail to a file. |
| Filter logs | Pipe to `grep` or `findstr`: e.g. `openclaw logs \| grep error`. |
| One-off agent | `openclaw agent --to <target> --message "..." --deliver` — run task and deliver result. |
| Cron run history | `openclaw cron runs --id <job-id> --limit 20` — inspect last 20 runs. |
| Config dump | Use `openclaw config get` with a base path (if supported) or script multiple `get` calls. |
| Safe updates | Run `openclaw update status` before `openclaw update`; consider a backup or staging run first. |
| Alias (shell) | `alias oc='openclaw'` — shorten to `oc status`, `oc health`, etc. |

---

## Placeholders & Conventions

- **`+447...`** — Replace with full WhatsApp/phone number (E.164).
- **`123456`** — Replace with Telegram user or chat ID.
- **`<job-id>`** — Cron job ID from `openclaw cron list`.
- **`<path>`** — Config key path (e.g. `gateway.port`, `models.default`).
- **`<value>`** — Config value to set (string or number as appropriate).