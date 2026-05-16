# Notion integration

If a Notion workspace is configured (look for `Notion:*` tools available in the session), prefer Notion for shared, durable, cross-session artifacts. Code-adjacent things stay in the repo where they version with the code.

The role output formats in this skill are deliberately designed to be **Notion-page-shaped *or* markdown-file-shaped** — they read the same way in both venues. So the choice of venue doesn't change the output, only where it lives.

## When to reach for Notion vs the repo

| Artifact | Default home | Why |
| --- | --- | --- |
| PRD | **Notion page** (link from repo) | Discussion-friendly, stakeholders can comment, survives across repos |
| Task / ticket | **Notion task** (`Notion:create-task`) | Belongs in your task board, not the repo |
| TDR (Technical Decision Record) | **Notion page** | Long-lived, referenced from many places |
| Roadmap / prioritization | **Notion database** | Multi-dimensional querying that flat markdown can't match |
| Postmortem | **Notion page** | Stakeholder-readable, links to incident timeline |
| README | **Repo** | Cold-start guide, must live with code |
| CHANGELOG | **Repo** | Tied to commits and releases |
| JSDoc / inline comments | **Repo** | Tied to code |
| API reference | **Repo** (`/docs/api.md`) | Versions with the API surface |
| Event spec (`/docs/events.md`) | **Repo** | Versions with the code that fires events |

## Common patterns

### Starting a new feature (PM → Architect → Build)

1. **PM** writes the PRD as a Notion page via `Notion:create-page`. Use the PM output format from `references/pm.md` as the page body.
2. Copy the Notion URL into the PR description and any related task.
3. Create the task with `Notion:create-task`, linking back to the PRD page.
4. When ready to execute, run `Notion:tasks:plan` against the task URL to draft an implementation plan, then `Notion:tasks:build` to execute it.

### Picking up a task from Notion

If the user hands you a Notion task URL, run `Notion:tasks:plan` first to surface the spec, success criteria, and any linked PRD/TDR. Don't start building until the plan is reviewed.

### After landing non-trivial code

Generate a Notion doc explaining the change with `Notion:tasks:explain-diff` and link it from the task. This is how the next person (often future-you) understands *why* the diff exists — the commit message tells you what, the Notion doc tells you why.

### Architect decisions

TDRs go as Notion pages under a "Decisions" parent. Use the Architect output format from `references/architect.md` as the page body. Cross-link related decisions so the dependency graph is browsable.

### Finding things

- `Notion:find` — quick title lookup when you know roughly what it's called.
- `Notion:search` — broader queries across the workspace.
- `Notion:database-query` — when you need to filter a database (e.g. all P0 tasks for this sprint).

## Fallback rule

If the Notion MCP is not available in the session, every role falls back to producing markdown in the response (or repo files where appropriate). Nothing in this skill *requires* Notion — it just prefers it when present.

## What not to put in Notion

- **Secrets or credentials.** Notion is a search index for your team — assume anything in it is broadly visible.
- **Anything that needs to version with code.** It'll drift the moment the code changes.
- **High-churn ephemeral state.** Use TodoWrite or the conversation for in-flight task tracking. Notion is for things you'd want to read again next month.
