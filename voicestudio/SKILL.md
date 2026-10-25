# VoiceStudio

Open-source, fully-local voice studio — a local alternative to ElevenLabs.
Voice cloning, voice design, video dubbing, dictation, transcription, and
audiobook creation in 646 languages. Runs on your own hardware; remote
services are optional.

Upstream: https://github.com/debpalash/VoiceStudio (AGPL-3.0).
Docs: https://github.com/debpalash/VoiceStudio/tree/main/docs

## When to use

- Give a video a voice: dub a clip with timed speech, generate narration or
  dialogue audio for a short-video episode.
- Keep one fixed voice across episodes: clone a character voice once, reuse
  it for every line instead of re-picking a TTS preset each time.
- Batch work: stories, audiobooks, and batch generation jobs.
- Work offline: everything local, no account, no per-character billing.
- Pair with lip-sync: VoiceStudio produces the audio, then the lip-sync
  workflow (MuseTalk draft, LatentSync final) syncs the mouth to it.

## Install

Electron is the only desktop app (the Tauri shell ended at 0.5.3; do not
install old Tauri builds).

```sh
# macOS / Linux, latest Electron release
curl -fsSL https://voicestudio.sh/install | sh

# Windows, in PowerShell
irm https://voicestudio.sh/install | iex
```

Other routes: download from
https://github.com/debpalash/VoiceStudio/releases/latest
(macOS `.dmg`, Windows `.exe`, Linux `.AppImage` or `.deb`); Docker guide
in `docs/install/docker.md`; build from source with
`git clone`, `bun install`, `bun run setup:api`, `bun run dev`.
For a coding agent, the upstream agent guide is `docs/install/agent.md`,
and upstream ships agent skills via `npx skills add debpalash/VoiceStudio`
(`voicestudio` for audio workflows, `voicestudio-maintainer` for repo work).

## Hardware

- NVIDIA GPU (Windows / Linux): CUDA acceleration. Apple Silicon: Metal.
- No dedicated GPU: fully usable on CPU, slower; setup installs the small
  CPU build of PyTorch (about 5 GB free disk).
- Windows on ARM: experimental, CPU only. Intel Mac: app UI only, connect
  to a remote backend.

## Core workflow

Workspaces: Clone, Dub, Design, Models.

1. Open **Voice cloning**, pick a voice or add a clean reference recording.
2. Enter the text and generate. Install the required model when prompted;
   hardware needs vary by engine, see `docs/performance.md`.
3. Default engine is VoiceStudio, powered by k2-fsa/OmniVoice. Other
   engines: see `docs/feature-catalog.md` and `docs/engines/README.md`.
4. For video: use the **Dubbing** workspace for timed speech.
5. For repeat jobs: save reusable recipes, use batch jobs or watch folders.
6. For agents: Local API (`docs/speech-platform.md`) and MCP
   (`docs/mcp.md`); examples in `examples/README.md`.

## Notes and limits

- Phone apps (iOS / Android) are preview only, run offline
  (`docs/mobile.md`).
- License: AGPL-3.0 covers the app. Model weights, tokenizers, voices,
  and generated-output conditions are separate; a paid app licence does
  not grant those rights.
- Responsible use, per upstream: clone voices only with permission. Only
  clone your own voice or a voice you are authorized to use; never use
  cloning to impersonate someone else.
- Troubleshooting and model downloads: `docs/install/troubleshooting.md`,
  `docs/downloading-models.md`.
