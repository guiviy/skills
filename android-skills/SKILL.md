# Android Skills

Source: github.com/android/skills (Apache-2.0). A dedicated repository of AI-optimized, modular instructions and resources that help agents follow Android best practices and guidance from developer.android.com. GitHub weekly trending #7, week of 2026-10-25.

## Why it matters for this user

- The user has a paused Android video player project (YingHe, built with Kotlin, mpv, ExoPlayer). If that work ever resumes, these skills cover the Android workflows where evaluations show LLMs underperform, such as R8 analysis, CameraX, Media3 cast integration, Wear, TV migration, profiling, and AGP upgrades.
- Skills follow the open agent-skills standard: markdown SKILL.md files that ground an LLM with a technical specification of a task.

## Install

- Via Android CLI, install a specific skill into the current directory: `android skills add r8-analyzer --project=.`
- Install all Android skills to directories for all detected agents: `android skills add --all`
- Without existing agent directories and without `--agent`, skills install for Gemini and Antigravity at `~/.gemini/antigravity/skills`.
- Options: `--all` (all skills; omitted, only the `android-cli` skill installs), `--agent` (comma-separated list of agents; omitted, installs for all detected agents), `--project` (project root path).
- Update: `android skills update --all` or `android skills update wear-compose-m3`.
- As an agent plugin, Claude Code: `/plugin marketplace add android/skills`, then `/plugin install android-skills@android-skills`. Codex: `codex plugin marketplace add android/skills`, then `codex plugin add android-skills@android-skills`.

## Use it

- Skills activate automatically when relevant. Prompt the agent with the task, for example "Make my app UI edge-to-edge", and the agent finds and uses the skill if installed.
- No build steps: skills are markdown files, not code.
- AI can make mistakes, so always double-check results, especially for app behavior, permissions, and release configuration.
