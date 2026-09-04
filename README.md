# cmux-agents

Live TUI dashboard of Claude Code / Codex / pi agents running in [cmux](https://cmux.com).

Shows every agent, its state (needs you / running / done / idle), and lets you:

- jump to an agent's terminal with **Enter**, a mouse click, or a number key (1–9, or type two digits)
- close an agent's terminal tab (**x** or click `✕tab`) or its whole workspace (**w** or click `✕ws`), with a y/N confirm
- refresh with **r**, quit with **q** (auto-refreshes every 3s)

## Agent quick install

Copy the prompt below into your coding agent (Codex, Claude Code, Cursor, etc.).
Use the **copy button** in the top-right corner of the block to grab it all.

```text
Clone https://github.com/aksman-dev/cmux-agents.git and install cmux-agents
on this Mac by following its README. Complete these steps:

1. Check prerequisites
   Use Python 3 with curses and the native cmux terminal app.
   Check Python with: python3 -c "import curses"
   Skip dependencies already installed; no pip packages are needed.
   If cmux is missing, follow https://cmux.com/docs/getting-started.
   Open cmux before checking the connection.

2. Verify the cmux connection
   Ensure cmux is on PATH, or set CMUX_BIN to its executable's full path.
   Run: "${CMUX_BIN:-cmux}" ping
   Run: "${CMUX_BIN:-cmux}" tree --all --id-format both
   Both must succeed. If access is blocked from this terminal, tell me
   to run the checks inside a cmux terminal before continuing.

3. Install the dashboard
   From the repository checkout, run:
   mkdir -p ~/.local/bin
   install -m 755 cmux-agents ~/.local/bin/cmux-agents

4. Make the command available
   Ensure ~/.local/bin is on PATH in both this session and new terminals.
   Preserve existing shell configuration and avoid duplicate PATH entries.
   If CMUX_BIN is needed, preserve it for new terminals too.
   Confirm: command -v cmux-agents

5. Verify a snapshot
   Run: cmux-agents --once
   Check for agent rows if Claude Code, Codex, or pi sessions are running.
   An empty snapshot is normal when no agents are running, but does not
   replace the successful cmux connection checks above.

6. Explain how to launch and use it
   Tell me to run cmux-agents in a dedicated cmux workspace.
   If a dashboard is already open, use that one: a duplicate launch in
   another workspace focuses the existing dashboard and closes the
   workspace it was launched from.
   Explain Enter or number keys to jump, r to refresh, and q to quit.
   Mention x / w close an agent tab / workspace after confirmation;
   do not use those controls to test the installation.
```

## Data sources

- `cmux tree --all` — surface ↔ tty ↔ workspace/window mapping
- `ps` — live claude/codex/pi processes (tty → session id)
- `cmux rpc feed.list` — per-session events (pending permissions, stop)
- `cmux rpc notification.list` — waiting/completed notifications per surface
- `~/.pi/agent/sessions/` — pi session files (pi has no cmux feed integration)

## Install

Requires macOS with [cmux installed and running](https://cmux.com/docs/getting-started),
Python 3 with `curses` (stdlib only), and the `cmux` CLI on `PATH` (or set
`CMUX_BIN` to its executable's full path). The cmux CLI is available automatically
inside cmux terminals.

```sh
git clone https://github.com/aksman-dev/cmux-agents.git
cd cmux-agents
mkdir -p ~/.local/bin
install -m 755 cmux-agents ~/.local/bin/cmux-agents
```

Ensure `~/.local/bin` is on `PATH`. If needed, add this to your shell configuration
(for example, `~/.zshrc`) and apply it in the current terminal:

```sh
export PATH="$HOME/.local/bin:$PATH"
```

Verify the connection and take a snapshot from a cmux terminal:

```sh
python3 -c "import curses"
"${CMUX_BIN:-cmux}" ping
"${CMUX_BIN:-cmux}" tree --all --id-format both
cmux-agents --once
```

The connection checks must succeed. A snapshot can be empty if no agents are
running; empty output alone does not confirm the cmux connection works.

## Usage

Run the interactive dashboard in a dedicated cmux workspace:

```sh
cmux-agents          # interactive TUI (single instance; re-running jumps to the existing one)
cmux-agents --once   # plain-text snapshot, no TUI
```

The TUI pins its workspace in cmux and enforces a single instance via a lock
file at `~/.local/state/cmux/cmux-agents.lock`. If a dashboard is already running
in another workspace, launching it again focuses the existing dashboard and
closes the workspace used for the duplicate launch.
