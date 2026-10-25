# ai-memory Cross-Agent Memory

Source: github.com/akitaonrails/ai-memory (MIT). Long-term memory for AI coding agents, built so work survives switching agents: quit Claude Code mid-task, start OpenAI Codex in the same directory, continue without re-explaining the architecture, the failed approaches, or the open questions. GitHub weekly trending #9, week of 2026-10-25.

## Core idea

- Memory follows the user across agents (20+ harnesses: Claude Code, Codex, Cursor, Gemini CLI, OpenCode, Grok, Devin, Kimi, Kiro, and others), across projects, and across machines.
- The source of truth is a git-backed wiki of ordinary `.md` files: greppable, editable by hand, openable in Obsidian, rsyncable. The database is a derived index rebuildable from the files. No vector store to maintain, no content locked in a binary blob.
- Capture is silent and automatic: lifecycle hooks record prompts, tool calls, and session boundaries, sanitized at a typed privacy boundary before anything is stored, and the server consolidates them into readable pages. Nothing has to be asked to be remembered.
- The default path uses zero LLM calls: capture, search, and handoffs all work with no API key.
- Memory ages without an LLM: per-tier decay curves, extractive compaction, dedup, contradiction flagging, and access-weighted retention (memory that is used decays slower). Nothing is hard-deleted: the original stays in git and `restore-page` brings it back. Rewriting or dropping content stays off by default until turned on.
- An opt-in LLM "dream" pass can rewrite whole clusters of cold notes into coherent pages while idle, cancels as soon as the user returns, and never deletes a source. It is off by default.
- Handoffs are a typed protocol: each has an owner and is claimed exactly once.

## Install

- Linux and macOS (two commands, no root):
  - `curl -fsSL https://github.com/akitaonrails/ai-memory/releases/latest/download/install.sh | sh`
  - `ai-memory setup`
- The installer downloads the release for the OS and CPU, verifies it against the release's published SHA-256 checksum (refuses to install without a match), and links `~/.local/bin/ai-memory`.
- `ai-memory setup` creates the data directory with a bearer token in an owner-only file, starts a per-user service (systemd `--user` or a launchd LaunchAgent), and wires MCP, hooks, and Agent Skills for every agent it finds (or the ones named with `--agent`). It shows the plan and asks first: answer `y`, or type the numbers of the agents to wire. `--plan` prints the plan and changes nothing, `--web` shows the plan in the browser, `--yes` skips the question.
- Windows (PowerShell, no admin): `irm https://github.com/akitaonrails/ai-memory/releases/latest/download/install.ps1 | iex`, then `ai-memory setup`.
- Claude Code and Codex can also wire it as a marketplace plugin (the binary is still installed as above):
  - `claude plugin marketplace add akitaonrails/ai-memory && claude plugin install ai-memory@ai-memory`
  - `codex plugin marketplace add akitaonrails/ai-memory && codex plugin add ai-memory@ai-memory`
- Docker: a wrapper script plus a container bound to loopback only (`127.0.0.1:49374`); native installs that run `ai-memory init` get a bearer token by default.

## Everyday use

- Prefer starting harnesses with `ai-memory run`: it auto-installs that harness's hooks and MCP if missing, wires the right project scope, and adds cross-harness session continuity. If the server is unreachable, `run` warns and launches anyway with local capture spooling; run `ai-memory recover` after the server returns.
  - `ai-memory run claude`
  - `ai-memory run codex --yolo`
  - `ai-memory run --profile work claude`
- Resume: `ai-memory continue` (newest managed checkout), `ai-memory resume` (pick from workstreams in this checkout), `ai-memory resume --search auth`, `ai-memory resume --all`.
- After setup, restart the agents and ask one of them "What do you remember about this project?" to see it answer through ai-memory.
- Two agents in the same project at once, or teammates on one server, work out of the box; knowledge is shared per project, personal handoffs stay personal, multi-user auth and an audit log are built in.
- On a real harness switch, the receiving agent is told to consult `AI_MEMORY_PREVIOUS_SESSION_HISTORY`, a private temporary markdown projection of the previous harness's visible history, which disappears when the managed child exits.
