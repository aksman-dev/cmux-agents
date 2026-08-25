# cmux-agents

Live TUI dashboard of Claude Code / Codex / pi agents running in [cmux](https://cmux.io).

Shows every agent, its state (**needs you** / running / done / idle), and lets you:

- jump to an agent's terminal with **Enter**, a mouse click, or a number key (1–9, or type two digits)
- close an agent's terminal tab (**x** or click `✕tab`) or its whole workspace (**w** or click `✕ws`), with a y/N confirm
- refresh with **r**, quit with **q** (auto-refreshes every 3s)

## Data sources

- `cmux tree --all` — surface ↔ tty ↔ workspace/window mapping
- `ps` — live claude/codex/pi processes (tty → session id)
- `cmux rpc feed.list` — per-session events (pending permissions, stop)
- `cmux rpc notification.list` — waiting/completed notifications per surface
- `~/.pi/agent/sessions/` — pi session files (pi has no cmux feed integration)

## Install

```sh
install -m 755 cmux-agents ~/.local/bin/cmux-agents
```

Requires Python 3 (stdlib only) and the `cmux` CLI on `PATH` (or set `CMUX_BIN`).

## Usage

```sh
cmux-agents          # interactive TUI (single instance; re-running jumps to the existing one)
cmux-agents --once   # plain-text snapshot, no TUI
```

The TUI pins its workspace in cmux and enforces a single instance via a lock
file at `~/.local/state/cmux/cmux-agents.lock`.
