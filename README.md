# pi-todo

Persistent task ledger with **sub-agent delegation** for [pi](https://pi.dev). Plan multi-step tasks, track progress, and farm out work to sub-agents — each with only the context they need.

## Features

- **Replace-not-append** — agent submits the full list each time, no drift
- **Sub-agent delegation** — mark items `delegated` with an agent type and focused context. Sub-agents get lean context, not full history
- **Auto-archive** — completed lists move to history automatically
- **History browsing** — review past task lists
- **Status bar badge** — `▶ 3/8` in the footer
- **Dedup detection** — warns on duplicate items
- **Max items cap** — 30 items max, prevents bloat

## Delegation workflow

```
1. Agent plans → todo_write with 8 items
2. Identifies parallel items → marks 'delegated' + agent type + focused context
3. Calls subagent with just the task context (not full conversation)
4. Sub-agent returns → agent marks completed with result summary
```

Each sub-agent sees **only what it needs** — the item's context field, not the full chat history. Keeps pipelines lean and parallelizable.

## Install

```bash
pi install git:github.com/dvictor357/pi-todo
```

## Usage

### Agent tool

| Tool | Does |
|------|------|
| `todo_write` | Submit the full list. Supports statuses: `pending`, `in_progress`, `completed`, `delegated` |
| `todo_history` | Browse archived lists |

### Item fields

| Field | Description |
|-------|-------------|
| `content` | Short imperative description |
| `status` | `pending` → `in_progress` → `completed`, or `delegated` for sub-agent work |
| `agent` | Sub-agent type (e.g. `librarian`, `solana-dev`) |
| `context` | Focused instructions for the sub-agent |
| `result` | Brief summary of what the sub-agent did |

### Commands

| Command | Does |
|---------|------|
| `/todo` | Show current task ledger |
| `/todo clear` | Archive current list and start fresh |
| `/todo history [N]` | Browse past N lists (default 5) |
| `/todo delegate <idx> [--agent name] [--context notes]` | Quick-delegate an item |
| `/todo <idx>` | Show item detail |

### Example

```
▶ Build memory extension       (in_progress)
⇢ Research library X → librarian (delegated)
☑ Create package.json          (completed)
☑ Write README                 (completed)
☐ Push to GitHub               (pending)
```

## Storage

```
~/.pi/agent/tmp/todos/
├── <cwd-hash>.json           # Current list
└── archive/
    └── <cwd-hash>-<ts>.json  # Completed / cleared lists
```

## Requirements

- **pi** `>=0.79`

## License

MIT
