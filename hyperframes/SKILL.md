# HyperFrames HTML-to-Video

Source: github.com/heygen-com/hyperframes (Apache-2.0). An open-source framework for turning HTML, CSS, media, and seekable animations into deterministic MP4 videos. GitHub weekly trending #6, week of 2026-10-25.

## Why it matters for this user

- The user is an AI short-video creator. HyperFrames covers the videos that do not need generative AI: intros, kinetic captions, data visualizations, chart races, map animations, product launches, PR walkthroughs, docs-to-video, and reusable motion graphics for automated content pipelines.
- It runs on the user's own machine or via a coding agent, not on the Muse VM, and it renders deterministically: the same input produces the same video.

## Requirements

- Node.js 22+ and FFmpeg.

## Install for a coding agent

- Claude Code plugin: `claude plugin marketplace add heygen-com/hyperframes`, then `claude plugin install hyperframes@hyperframes`. Enable auto-update in Claude Code under /plugin, Marketplaces, hyperframes, Enable auto-update. Then use `/hyperframes:hyperframes`.
- Standalone skills (including OpenCode): `npx skills add heygen-com/hyperframes` (interactive picker; the Core Skills group is all that is needed) or `npx hyperframes skills update` (installs exactly the core set from current main, preferred for agents and non-interactive runs).
- Core set: `/hyperframes` (read first for any video request; holds the render contract and work loop), `/hyperframes-cli` (CLI by job), `/hyperframes-registry` (about 450 ready-made blocks and components as editable source), `/media-use` (music, sound effects, images, voiceover, generated video, transcripts, cutouts, color grades).
- On demand: `/figma` (bring Figma into a video), `/slideshow` (navigable decks, not MP4), `/remotion-to-hyperframes` (import Remotion, Motion Canvas, Lottie, Three.js, canvas work, verified frame by frame).

## Use it manually with the CLI

- `npx hyperframes init my-video`
- `cd my-video`
- `npx hyperframes preview` (preview in browser with live reload)
- `npx hyperframes render` (render to MP4)
- A prompt that works with an agent: "Using `/hyperframes`, create a 10-second product intro with a fade-in title, a background video, and subtle background music."
- The production loop the skills teach agents: plan the video, write valid HTML, wire seekable animations, add media, lint, preview, render.

## How a video is defined

- A video is HTML. A composition is a div with `data-composition-id`, `data-start`, `data-width`, and `data-height`.
- Clips carry `data-start`, `data-duration`, and `data-track-index`: video elements, headings, audio elements (with `data-volume`).
- Animations use GSAP, CSS, Lottie, Three.js, Anime.js, or WAAPI through a frame adapter; a GSAP timeline is paused and registered on `window.__timelines` under the composition id so the renderer can seek it.
- The renderer seeks each frame in headless Chrome and encodes with FFmpeg, so output is deterministic.

## Registry and design system

- Before hand-building any named visual, check the registry: transitions, overlays, titles, kinetic type, captions, charts, maps, 3D, camera moves. Install with `hyperframes add` and browse the catalog.
- `frame.md` is the design-system translation layer: it takes a web design spec and rewrites it for the frame (same tokens, same rules, composed for a camera rather than a browser). Browse and remix published frames at hyperframes.dev/design.
