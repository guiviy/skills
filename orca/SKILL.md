# Orca Agent Orchestrator

Source: github.com/stablyai/orca (MIT). The AI orchestrator for running a fleet of agents. GitHub weekly trending #5, week of 2026-10-25.

## Note on applicability

- Orca is a desktop app the user runs on their own machine, not a Muse feature. Record it so the user can install and drive it themselves.

## What it is

- Run Codex, Claude Code, OpenCode, Pi, and any other CLI agent side by side, each in its own isolated git worktree, tracked in one place.
- Works with any CLI agent: if it runs in a terminal, it runs in Orca.

## Install

- Desktop (macOS, Windows, Linux): download from onorca.dev, or via package manager: `brew install --cask stablyai/orca/orca` (macOS), `yay -S stably-orca-bin` (Arch Linux AUR).
- Mobile companion (iOS, Android): pair with the desktop app to monitor and steer agents from the phone. iOS via the App Store, Android via APK from onorca.dev.

## Use it

- Fan one prompt across several agents, each in its own isolated git worktree, compare the results, and merge the winner.
- Mobile companion: get notified when an agent finishes and send follow-ups from anywhere.
- Terminal splits: Ghostty-class terminals with WebGL rendering, infinite splits, and scrollback that survives restarts.
- Design mode: click any UI element in a real Chromium window to send its HTML, CSS, and a cropped screenshot straight into the agent's prompt.
- GitHub and Linear, native: browse PRs, issues, and project boards in-app; open a worktree from any task.
- SSH worktrees: run agents on a remote box with full file editing, git, and terminals, with auto-reconnect and port forwarding.
- Annotate AI diffs: comment on any diff line and ship the comments back to the agent.
- Drag files or images straight into an agent prompt; VS Code editor with autosave everywhere.
- Orca CLI: agents drive Orca too. Script workflows with `orca worktree create`, `snapshot`, `click`, and `fill`.
- Account switcher and usage tracking: see Claude and Codex usage and rate-limit resets, and hot-swap accounts without re-logging in.
