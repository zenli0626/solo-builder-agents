# solo-builder-agents

A [Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills) that turns a single solo builder into 11 named role agents for Next.js + Supabase + Vercel + Railway projects.

When you say **"As PM Agent…"** or **"As Fullstack Agent…"** (or just ask to plan / build / ship / test / document a feature), Claude loads that role's responsibilities, rules, and output format and stays in scope until you hand off.

## The 11 roles

| Role | Owns |
| --- | --- |
| **PM** | PRDs, user stories, acceptance criteria, scope cuts |
| **Architect** | System design, TDRs, coding standards, cross-domain approvals |
| **Fullstack** | Next.js pages/components, API routes, Supabase queries |
| **AI** | LLM prompts, agent logic, streaming, prompt versioning |
| **Data** | Supabase schema, migrations, RLS, SQL |
| **DevOps** | Vercel, Railway, CI, env vars, deploys |
| **QA** | Vitest, Playwright, edge cases, coverage |
| **SRE** | Sentry, uptime, SLOs, postmortems |
| **Analytics** | Events, KPIs, funnels, dashboards |
| **Growth** | SEO, onboarding, conversion, A/B tests |
| **Docs** | README, API reference, changelog, JSDoc |

Each role has its own reference file in [`references/`](references/) — only the active role's rules load into context, so the system scales without bloating prompts.

## Install

Clone into Claude Code's personal skills directory:

```bash
git clone https://github.com/zenli0626/solo-builder-agents.git ~/.claude/skills/solo-builder-agents
```

That's it — the skill is auto-discovered next time Claude Code starts a session. Confirm with `/skills` (or just trigger it).

To update later:

```bash
cd ~/.claude/skills/solo-builder-agents && git pull
```

## Triggering

The skill activates on any of:

- **Explicit role**: "As PM Agent, write a PRD for…", "As Data Agent, design a schema for…"
- **Implicit task**: "help me plan a feature", "ship this to Vercel", "add tests for the export endpoint"
- **High-risk framing**: schema changes, auth, payments, data migrations, API contract changes — the skill's high-risk gate kicks in automatically

You don't need to memorize the role names. Claude infers the role from the task and tells you which one it's wearing before producing output.

## Customizing for your project

This skill is intentionally generic — it brings the **role system**, not project specifics. To adapt it:

1. **Keep the skill installed as-is** so updates from upstream stay easy to pull.
2. **Add a project-level `CLAUDE.md`** in your repo with project name, stack overrides, and any house rules. The skill explicitly defers to a repo's own `CLAUDE.md` when one exists.
3. **Fork this repo** if you want to maintain your own opinionated variant (e.g. swapping Tailwind for vanilla-extract, or Vercel for Fly).

## What's in the box

```
solo-builder-agents/
├── SKILL.md                    ← workflow, handoff protocol, global rules
└── references/
    ├── pm.md
    ├── architect.md
    ├── fullstack.md
    ├── ai.md
    ├── data.md
    ├── devops.md
    ├── qa.md
    ├── sre.md
    ├── analytics.md
    ├── growth.md
    └── docs.md
```

521 lines total. SKILL.md stays under 120 lines so the always-loaded portion is cheap; role files are loaded on demand.

## Conventions worth knowing

- **Commit prefix per role**: `pm: …`, `fullstack: …`, `data: …`, etc. — keeps the git log legible when one person wears all the hats.
- **High-risk gate**: schema changes, auth, payments, data migrations, and API contract changes all route through Plan Mode + extended thinking before execution.
- **No silent cross-boundary work**: if a task touches two roles, the primary and supporting roles get named up front.

## License

MIT
