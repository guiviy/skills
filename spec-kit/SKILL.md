# Spec Kit — Spec-Driven Development Toolkit

Source: github.com/github/spec-kit (MIT, GitHub). An open source toolkit that gives AI coding agents structured processes, reusable templates, and documented outcomes. The headline process is Spec-Driven Development (SDD): write the spec first, then let the agent turn it into a plan, tasks, and working code — with the spec carried through every step instead of living only in the first prompt.

## When to use it

- Use it when starting a feature or a whole application with a coding agent, so the agent works from a written, reviewable spec instead of improvising from a one-line request.
- Use it for a project that will grow over many sessions: the constitution and the specs in `.specify/` become the durable record of what was decided and why.
- Use the bug-fixing process when a bug needs a diagnosed cause, a scoped fix, and a recorded verification — not just a quick patch.
- Use the idea-assessment process when deciding whether an idea deserves investment, before writing any code.
- Do not use it for a trivial one-file script or a throwaway experiment: the spec-plan-tasks overhead only pays off when the work is big enough to need a plan and a review trail.

## Three processes, one toolkit

| What you need | Process | Outcome |
| --- | --- | --- |
| Build a feature or application | Spec-Driven Development | A specification carried through planning, implementation, and convergence |
| Diagnose and repair broken behavior | Bug fixing | An assessed cause, scoped fix, and recorded verification |
| Decide whether an idea deserves investment | Idea assessment | An evidence-backed go, clarify, or stop decision |
| Adopt or create a different process | Your own process | A process adapted or assembled from reusable primitives |

These are independent entry points, not mandatory phases. SDD ships in core; bug fixing and assessment are bundled extensions installed when needed.

## Install

- Requirements: Python 3.11+, `uv`, and a supported AI coding agent, on Linux, macOS, or Windows.
- Install the CLI and create a project (example uses GitHub Copilot; pick the integration for the agent actually in use):
  - `uv tool install specify-cli`
  - `specify init my-project --integration copilot`
  - `cd my-project`
- For other agents, pinned releases, CI, and troubleshooting, see the installation docs. For an existing project with code already in it, see the existing-projects guide instead of `specify init`.
- Upgrade an existing installation with the upgrade guide; do not reinstall over a project that already has `.specify/` state.

## Spec-Driven Development flow

- Create a constitution once per project, then repeat the remaining steps for each feature. Run the skills in the agent's chat, not the terminal, one at a time, reviewing each result before continuing. The example below uses Copilot's default skill syntax; other agents use their equivalent invocation syntax.

```
/speckit-constitution Create principles focused on code quality, testing, and maintainability.
/speckit-specify Build a photo organizer with albums grouped by date and a tile preview of each album.
/speckit-plan Use Vite with vanilla JavaScript. Keep images local and store metadata in SQLite.
/speckit-tasks
/speckit-implement
/speckit-converge
```

- Repeat implement and converge until convergence reports `Converged`. Do not declare a feature done while convergence still reports gaps.
- Use the clarification, checklist, and consistency-analysis steps from the SDD walkthrough when the spec is ambiguous or the plan drifts from the spec.
- Review every stage's output before running the next command: the point of the process is that each artifact is checkable, so a bad spec caught at the specify stage costs minutes, while the same mistake caught after implementation costs a rewrite.

## Bug fixing flow

- Install the extension once, from the project directory: `specify extension add bug`.
- No SDD feature workflow is required first. Run the skills in the agent's chat, one at a time:

```
/speckit-bug-assess "Submitting an empty password crashes the login form." slug=login-crash
/speckit-bug-fix slug=login-crash
/speckit-bug-test slug=login-crash
```

- Review the reports in `.specify/bugs/login-crash/`. The final verdict is `verified`, `partial`, or `failed`; only `verified` confirms a successful fix.

## Idea assessment flow

- Install the extension once, from the project directory: `specify extension add assess`.
- No source code or SDD feature workflow is required. Run the skills in the agent's chat, one at a time:

```
/speckit-assess-intake "Let users work offline and sync when they reconnect." slug=offline-mode
/speckit-assess-research slug=offline-mode
/speckit-assess-define slug=offline-mode
/speckit-assess-shape slug=offline-mode
/speckit-assess-decide slug=offline-mode
```

- Review the artifacts in `.specify/assessments/offline-mode/` and the final decision: `go`, `needs-clarification`, or `kill`.
- Resolve unknowns by editing the existing Markdown artifacts directly or with the agent, rather than rerunning whole stages.
- If an idea gets a `go` decision and is worth building, establish a project constitution if needed, then pass the handoff summary from `decision.md` to `/speckit-specify`. Stopping with a documented reason is also a useful outcome.

## Customizing and extending

- Adapt a process with a preset (for example, requiring regulatory traceability in plans). Use a project-local override for a one-off template change.
- Bring your own process by adding its commands through an extension and exposing them to the coding agent through an integration.
- Automate prompts, commands, gates, and shell tasks in a workflow; use overlays to adapt an installed workflow without forking it, or add custom workflow steps when the built-in types are not enough.
- Share a reusable setup by packaging extensions, presets, workflows, and workflow steps in a bundle.
- Search the active catalogs with `specify extension search`, `specify preset search`, `specify workflow search`, and `specify bundle search`. Catalog listings are not audits or endorsements — review community source code before installing or running it.
- Start with the smallest change that meets the need; do not replace the whole process to fix one template.
- Inspect which layer supplies a command, template, or script with the artifact inventory, and work offline or behind a firewall with local sources and self-hosted catalogs.

## Notes and limits

- Spec Kit's commands are agent-chat skills (`/speckit-*`), not terminal commands: the CLI installs and manages them, the agent runs them in chat. Do not run `/speckit-specify` in a terminal and expect it to work.
- The constitution is the project's durable principles file — write it once, keep it short, and change it deliberately. A constitution that tries to cover every detail becomes noise the agent stops following.
- A spec is only useful if it is reviewed: never let the agent skip from specify straight to implement without a human reading the spec and the plan.
- Templates, commands, terminology, and methodology can all be adapted, but keep adaptations in presets or project-local overrides so the upstream toolkit stays upgradeable.
- Full documentation: https://github.github.io/spec-kit/ — CLI reference, process commands, SDD walkthrough and philosophy, how-to guides, customization guide, and community components.

## Websites

- [Spec Kit documentation](https://github.github.io/spec-kit/) — CLI reference, process commands, SDD walkthrough, and how-to guides. Carry the command name into the docs when its arguments are unclear.
- [GitHub — github/spec-kit](https://github.com/github/spec-kit) — source, releases, and issue tracker. Carry the release version into any bug report.
