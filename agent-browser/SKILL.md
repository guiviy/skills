# Agent Browser — Browser Automation CLI for AI Agents

Source: github.com/vercel-labs/agent-browser (Apache-2.0, Vercel Labs). A native Rust CLI that gives an AI agent a browser it can drive one command at a time from the shell: open pages, read them as a compact accessibility tree, and act on elements by reference. The browser stays open between commands, so each step is one short shell call.

## When to use it

- Use it when a task needs a real browser driven step by step: checking a live page, clicking through a form, verifying a deployed site renders correctly, scraping a page that needs JavaScript, or taking screenshots and PDFs.
- Use it instead of guessing from a static page fetch when the page is interactive, behind a login, or changes after JavaScript runs.
- Use it for repeatable browser checks (for example, verifying a deployed site after a push) rather than doing the same clicks by hand each time.
- Do not use it for bulk scraping or hammering a site: keep the rate close to what a human would do, respect robots rules and the site's terms, and stop on a 429 or a block.

## Install

- `npm install -g agent-browser`, then `agent-browser install` (downloads Chrome for Testing, first time only).
- Homebrew: `brew install agent-browser`, then `agent-browser install`.
- Linux: `agent-browser install --with-deps` also installs the browser's system libraries.
- No install: `npx agent-browser open example.com` works for a one-off run.
- Native binaries ship for macOS (ARM64, x64), Linux (ARM64, x64, glibc and musl), and Windows (x64); the npm package picks the right one.
- Run `agent-browser doctor` if the browser fails to launch, and `agent-browser upgrade` to update the CLI.

## The core loop

- Every command goes to a background daemon that starts on the first command and controls the browser over the Chrome DevTools Protocol. Each `--session` gets its own daemon and browser. By default the daemon closes a headless browser and exits after one hour without commands, saving state first when session persistence is configured.
- The loop is always: snapshot, act on a ref, read again to check the result, repeat.

```
agent-browser open example.com
agent-browser snapshot -i       # interactive elements, each with a ref like @e1
agent-browser click @e1         # act on a ref from that snapshot
agent-browser get url           # check the result
agent-browser close
```

- `snapshot` returns the page as compact text with a ref such as `@e1` for each element; `snapshot -i` lists interactive elements only.
- Refs describe the page at the time of the snapshot. After navigation or any DOM change, take a new snapshot before using refs again — a stale ref acts on the wrong element or fails.
- CSS selectors, text, XPath, and semantic locators also work, for example `agent-browser find role button click --name "Submit"`.
- A click fails, instead of reporting success, when the target is hidden, disabled, outside the viewport, or covered by another element such as a consent banner. Dismiss the covering element, take a new snapshot, and retry.

## Command groups

- Navigate and read: `open`, `back`, `forward`, `reload`, `read`, `snapshot`, `get`, `is`.
- Interact: `click`, `dblclick`, `fill`, `type`, `press`, `keyboard`, `hover`, `focus`, `select`, `check`, `uncheck`, `scroll`, `drag`, `upload`, `mouse`, `find`.
- Wait: `wait` — wait for an element, text, or navigation before acting; never assume a fixed sleep is enough on a slow page.
- Capture: `screenshot`, `pdf`, `record`, `download`, `diff` — screenshot and diff are the fastest way to verify a visual change after a deploy.
- Page and network state: `cookies`, `storage`, `network`, `set`, `clipboard`, `eval`, `pushstate`.
- Tabs, frames, and dialogs: `tab`, `frame`, `dialog`.
- Debug and performance: `console`, `errors`, `highlight`, `inspect`, `trace`, `profiler`, `vitals`, `react`, `a11y`.
- Sessions and auth: `session`, `state`, `auth`, `profiles`, `confirm`, `deny` — the auth vault, domain allowlists, and action policies live here; check the Security docs before automating any logged-in flow.
- Agents and integrations: `skills`, `mcp`, `webmcp`, `flow`, `chat`, `batch`, `plugin`.
- Observe: `stream`, `dashboard`.
- Setup: `install`, `upgrade`, `doctor`, `connect`, `close`.
- Run `agent-browser --help` for the full list, or `agent-browser <command> --help` for one command.

## Skills and MCP

- The CLI ships version-matched skills. Load the core skill before the first use and follow it — it always matches the installed CLI version:
  - `agent-browser skills get core` — the core loop, refs, waiting, and common workflows.
  - `agent-browser skills get core --ref commands` — one reference, such as the full command list.
  - `agent-browser skills get core --full` — adds the full command reference and templates.
  - `agent-browser skills list` — specialized skills (electron, slack, dogfood, and more).
- Install the discovery skill into an agent harness with `npx skills add vercel-labs/agent-browser`. The installed skill is a thin stub that tells the agent to load `agent-browser skills get core`, so its instructions always match the installed CLI.
- Or add this to the project's `AGENTS.md`:
  - `Use agent-browser for browser automation. Before the first use, run agent-browser skills get core and follow it.`
- For MCP clients, `agent-browser mcp` starts an MCP server over stdio.

## Browser engines and providers

- Chrome for Testing is the default browser. Attach to a running Chrome with `--cdp` or `--auto-connect`, select the Lightpanda or Obscura engine with `--engine`, drive Mobile Safari in the iOS Simulator, or use a cloud browser provider such as Browserbase, Browserless, Browser Use, Kernel, or AgentCore.

## Notes and limits

- Keep credentials out of shell history and logs: use the auth vault and profiles (`agent-browser auth`, `agent-browser profiles`) rather than typing passwords into `fill` from a recorded command line.
- A headless browser is still a real browser session: logging in, changing settings, or submitting a form has real effects on the account. Confirm the target site and the intended action before running a write step, and never automate a purchase or a destructive action without the user saying so.
- When a page blocks automation or shows a bot check, stop and report it rather than trying to work around the check.
- Full reference: https://agent-browser.dev — commands, configuration (`agent-browser.json`, flags, environment variables), security (auth vault, domain allowlists, action policies), and the changelog.

## Websites

- [agent-browser.dev](https://agent-browser.dev/) — introduction, guides, command reference, and configuration. Carry the command name into the docs when a flag's behavior is unclear, and check the changelog before relying on a new command.
- [GitHub — vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) — source, releases, and issue tracker. Carry the release version into any bug report.
