# Graphify Code Knowledge Graph

Source: github.com/Graphify-Labs/graphify (Apache-2.0). Type `/graphify` in an AI coding assistant and it maps an entire project (code, docs, PDFs, images, videos) into a knowledge graph to query instead of grepping through files. GitHub weekly trending #8, week of 2026-10-25.

## What it does

- Code maps for free, fully local: code is parsed with tree-sitter AST, deterministic, no LLM, nothing leaves the machine. Docs, PDFs, images, and video use the assistant's model or a configured API key for a semantic pass.
- Every edge is explained: each connection is tagged `EXTRACTED` (explicit in the source) or `INFERRED` (resolved by graphify), so what was read directly is distinguishable from what was inferred.
- Not a vector index: no embeddings, no vector store. A real graph to traverse: ask a question, trace the path between two things, or explain one concept.
- Out of the box: god nodes (most-connected concepts), communities (graph split into subsystems via Leiden, with LLM-free labels), cross-file links (`calls` / `imports` / `inherits` / `mixes_in`) resolved across about 40 languages, query/path/explain commands, rationale and doc refs (`# NOTE:` / `# WHY:` comments and ADR/RFC citations as first-class nodes), and local-first operation.

## Install

- Prerequisites: Python 3.10+ and uv (recommended) or pipx.
- Official PyPI package is `graphifyy` (double-y). Other `graphify*` packages on PyPI are not affiliated. The CLI command is still `graphify`.
- `uv tool install graphifyy` (recommended, isolated env; if `graphify` is not found after, run `uv tool update-shell`), or `pipx install graphifyy`, or `pip install graphifyy`.
- Then run `graphify install` once. It detects the assistant and registers the skill, so there is no per-assistant setup.
- Project-scoped install: `graphify install --project` (writes under the current directory, for example `.claude/skills/graphify/SKILL.md` plus a `references/` sidecar, and prints a `git add` hint for committable files). Per-platform variants accept the same flag, for example `graphify claude install --project` or `graphify codex install --project`.
- Supported assistants include Claude Code, Cursor, Codex, Gemini CLI, GitHub Copilot, OpenCode, and 20+ more, on macOS, Linux, and Windows.

## Use it

- In the AI assistant: `/graphify .`
- That produces three files in `graphify-out/`:
  - `graph.html`: open in any browser, click nodes, filter, search.
  - `GRAPH_REPORT.md`: highlights, key concepts, surprising connections, suggested questions.
  - `graph.json`: the full graph, queryable anytime without re-reading files.
- Query the graph directly:
  - `graphify explain "APIRouter"` explains one concept with its connections.
  - `graphify path "FastAPI" "ModelField"` traces the shortest path between two things, hop by hop.
  - `graphify query "<question>"` returns a scoped subgraph for a plain-language question.
- The persisted graph includes `graph.schema_version` and `graph.graphify_version` so integrations can detect incompatible format changes.
- Graphify Cloud is the always-on hosted version (monorepo and cross-repo support, formal verification, code review, Sentry/Jira connectors); the local CLI above is free and needs no account.
