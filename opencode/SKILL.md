# OpenCode AI Coding Agent

Source: github.com/anomalyco/opencode (MIT). The open-source AI coding agent. GitHub weekly trending #4, week of 2026-10-25.

## Note on naming and applicability

- The project renamed itself from OpenCode to opencode; README references to OpenCode and opencode mean the same project.
- OpenCode is a terminal agent the user runs on their own machine, not a Muse feature. Record it so the user can install and drive it themselves.

## Install

- `curl -fsSL https://opencode.ai/v2/install | bash`
- Or via package managers: `npm i -g @opencode/cli@latest`, `brew install anomalyco/tap/opencode-v2` (macOS and Linux, recommended, always up to date), `brew install opencode` (official formula, updated less often), `sudo pacman -S opencode` (Arch stable), `mise use -g opencode`, `nix run github:anomalyco/opencode`.
- The install script installs to `$HOME/.opencode/bin`. Remove versions older than 0.1.x before installing.
- Desktop app (Beta): download from opencode.ai/download (macOS Apple Silicon and Intel, Windows, Linux .deb/.rpm/.AppImage); macOS via `brew install --cask opencode-desktop`.

## Use it

- Open a terminal in a project directory and run `opencode`, then chat with it about the code.
- Two built-in agents, switched with the `Tab` key:
  - build: default, full-access agent for development work.
  - plan: read-only agent for analysis and code exploration; denies file edits by default and asks permission before running bash commands. Ideal for exploring unfamiliar codebases or planning changes.
- A `general` subagent handles complex searches and multistep tasks internally; invoke it with `@general` in messages.
- Full configuration reference lives in the docs at opencode.ai.
