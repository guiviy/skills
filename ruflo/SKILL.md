# Ruflo Agent Harness

Source: github.com/ruvnet/ruflo (MIT), originally Claude Flow. An agent meta-harness for Claude Code and Codex: 100+ specialized agents, coordinated swarms, self-learning memory, federated comms across machines, and security guardrails. GitHub weekly trending #2, week of 2026-10-25.

## Note on applicability

- Ruflo is a Claude Code / Codex-side harness, not a Muse feature. Record it so the user can run it on their own machine; Muse itself does not install or run Ruflo.

## Install path A: Claude Code plugins

- Requires Claude Code 2.1.287 or later. Inside Claude Code:
  - `/plugin marketplace add ruvnet/ruflo`
  - `/plugin install ruflo-core@ruflo`
  - `/plugin install ruflo-console@ruflo`
  - `/plugin install ruflo-mods@ruflo`
  - `/reload-plugins`
  - `/ruflo` opens the agent cockpit.
- Core provides foundation tools, Console opens the cockpit, Mods add routing and policy enforcement inside Claude Code.
- Caution: mods run with the account's permissions and are not sandboxed, so review their code before installing.

## Install path B: CLI and MCP

- Run the setup wizard in a project terminal: `npx ruflo@latest init wizard`
- For MCP-enabled clients, configure command `npx` with arguments `["-y", "ruflo@latest", "mcp", "start"]`, or run `npx -y ruflo@latest mcp start` directly.
- The console also runs standalone, without Claude Code: `npx ruflo tui` (add `--plain` to print one frame for scripts). It shows Overview, Cost, Swarm, Events, and Missions.

## Use it

- After `init`, just use Claude Code normally: the hooks system routes tasks, learns from successful patterns, and coordinates agents in the background. There is no need to learn all its MCP tools or CLI commands first.
- One concrete first task: in the console, go to Settings, Claude control, choose write + ask, then ask Claude to create a mission (for example, add a dark mode toggle), approve the creation, and verify the mission appears in Missions and Overview.
- Control levels: read, write, manage, or full, with ask or auto approval. Default is read + ask. Raising the level is the user's choice; network, spending, and destructive actions require confirmation even in auto. Take back control pauses Claude's console tools.
- Optional ruOS Desktop: connect ChatGPT or Claude via MCP at the ruOS MCP endpoint; `npx ruvector` gives agents local vector memory.

## Mental model

- Agent = Model + Harness. The model writes; the harness gives it tools, memory, loops, sandboxes, and controls. Ruflo is that execution layer around Claude Code and Codex.
