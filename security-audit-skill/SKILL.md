# Cloudflare Security Audit Skill

Source: github.com/cloudflare/security-audit-skill (MIT). A coding-agent skill that turns an agent into a security auditor. GitHub weekly trending #1, week of 2026-10-25.

## When to use it

- Auditing a codebase the user owns (their Cloudflare Worker, scripts, app code) for real vulnerabilities.
- Only on code the user owns or is authorized to test. This is a code audit skill, not a live-target attack tool.

## Install

- `npx skills add https://github.com/cloudflare/security-audit-skill --skill security-audit`
- Add `--global` for a user-level install. Run `npx skills --help` for agent-selection and non-interactive options.

## Run an audit

- Start the coding agent in (or pointed at) the codebase, then ask for an audit:
  - `security audit this codebase`
  - `find security vulnerabilities in ./src`
  - `do a security review, output to ~/audits/my-project`
- The skill activates automatically on trigger phrases like security audit, find vulnerabilities, pen-test the code.
- Full audit mode is the default for a direct codebase audit; security questions and focused vulnerability work use guidance mode unless report artifacts are requested.
- Unspecified output directory defaults to `~/security-audit-skill/<repo-name>/run-<N>`. The workflow writes inside the target repo only when the user explicitly selects a directory that version control ignores.

## The six phases

1. Reconnaissance: map architecture, trust boundaries, input surfaces, prior evidence, and deterministic coverage in `architecture.md` and `coverage-ledger.json`.
2. Coverage-led hunting: isolated hunters work from ledger units; coverage critics find gaps.
3. Candidate validation: every unique candidate goes to a fresh verifier that tries to disprove it.
4. Structured output: write `confirmed`, `needs_validation`, and `rejected` records to `findings.json`, validated against `report-schema.json`.
5. Independent record verification: fresh agents verify final source claims; material replacements get another independent verifier.
6. Target-neutral reporting: derive `REPORT.md`, `FINDINGS-DETAIL.md`, and `NEEDS-VALIDATION.md` from verified records and the coverage ledger.
- Run `validate-coverage-ledger.cjs` after creating the ledger and after each update; run `validate-findings.cjs` in phase 4 and after every phase 5 replacement.

## Verdict rules that matter

- Only confirm established boundary failures. A source-grounded blocked lead stays `needs_validation` with its exact unresolved fact, and carries no severity.
- The agent that checks a finding is never the agent that found it.
- Severity requires impact (likelihood x impact), not deviation from a checklist.
- Defense-in-depth gaps are not vulnerabilities: if layer A prevents the attack, the absence of layer B is a hardening note.
- Multiple runs improve coverage; a single run found roughly half of what repeated runs found in total.

## Requirements

- A coding agent with a model that supports tool use and parallel sub-agents.
- Node.js for the zero-dependency findings and coverage-ledger validators.
- An OS-enforced sandbox for target-controlled builds, tests, processes, browsers, emulators, fuzzers, and fixtures (no external networking, sanitized allowlisted environment, resource limits, writes only to assigned scratch paths). Without these controls, the workflow keeps the lead as `needs_validation` instead of executing target code.
