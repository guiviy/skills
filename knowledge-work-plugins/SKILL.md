# Anthropic Knowledge Work Plugins

Source: github.com/anthropics/knowledge-work-plugins (Apache-2.0). Open-source plugins that turn Claude into a specialist for a role, team, or company. Built for Claude Cowork, also compatible with Claude Code. GitHub weekly trending #3, week of 2026-10-25.

## Note on applicability

- These are Claude-side plugins, not Muse features. Record them so the user can install them in Claude Cowork or Claude Code on their own machine.

## The plugin set

- productivity: tasks, calendars, daily workflows, personal context. Connectors: Slack, Notion, Asana, Linear, Jira, Monday, ClickUp, Microsoft 365.
- sales: prospect research, call prep, pipeline review, outreach drafts, competitive battlecards. Connectors: Slack, HubSpot, Close, Clay, ZoomInfo, Notion, Jira, Fireflies, Microsoft 365.
- customer-support: ticket triage, response drafts, escalation packages, knowledge base articles. Connectors: Slack, Intercom, HubSpot, Guru, Jira, Notion, Microsoft 365.
- product-management: specs, roadmaps, user research synthesis, stakeholder updates, competitive tracking. Connectors: Slack, Linear, Asana, Monday, ClickUp, Jira, Notion, Figma, Amplitude, Pendo, Intercom, Fireflies.
- marketing: content drafts, campaign plans, brand voice enforcement, competitor briefs, performance reports. Connectors: Slack, Canva, Figma, HubSpot, Amplitude, Notion, Ahrefs, SimilarWeb, Klaviyo.
- legal: contract review, NDA triage, compliance, risk assessment, meeting prep, templated responses. Connectors: Slack, Box, Egnyte, Jira, Microsoft 365.
- finance: journal entries, reconciliation, financial statements, variance analysis, close management, audit support. Connectors: Snowflake, Databricks, BigQuery, Slack, Microsoft 365.
- data: SQL queries, visualization, statistical analysis, dashboards, validation. Connectors: Snowflake, Databricks, BigQuery, Definite, Hex, Amplitude, Jira.
- enterprise-search: one query across email, chat, docs, and wikis. Connectors: Slack, Notion, Guru, Jira, Asana, Microsoft 365.
- bio-research: preclinical research tools and databases. Connectors: PubMed, BioRender, bioRxiv, ClinicalTrials.gov, ChEMBL, Synapse, Wiley, Owkin, Open Targets, Benchling.
- cowork-plugin-management: create new plugins or customize existing ones.

## Install

- Cowork: install from claude.com/plugins.
- Claude Code:
  - `claude plugin marketplace add anthropics/knowledge-work-plugins`
  - `claude plugin install sales@knowledge-work-plugins` (swap sales for the plugin name wanted)

## Use it

- Once installed, plugins activate automatically. Skills fire when relevant; slash commands are available in the session, for example `/sales:call-prep` or `/data:write-query`.
- Every plugin follows the same file structure: `.claude-plugin/plugin.json` (manifest), `.mcp.json` (tool connections), `commands/` (slash commands), `skills/` (domain knowledge Claude draws on automatically).
- Every component is file-based markdown and JSON: no code, no infrastructure, no build steps.

## Make them yours

- Swap connectors: edit `.mcp.json` to point at the user's actual tool stack.
- Add company context: drop terminology, org structure, and processes into skill files.
- Adjust workflows: modify skill instructions to match how the team actually works.
- Build new plugins: use the `cowork-plugin-management` plugin or copy the structure above.
