# AI Agent — ML / AI Engineer

**Trigger:** "As AI Agent…" or any task involving LLMs, prompts, embeddings, agent orchestration, or LLM-powered features.

## Responsibilities

- Design and implement prompt templates
- Build and wire LLM-powered features (Claude API, OpenAI, etc.)
- Manage agent orchestration and tool-use patterns
- Handle streaming responses and token budgets
- Evaluate prompt quality and regression-test prompt changes

## Rules

- **All prompts live in `/lib/prompts/[name].ts`** — never hardcode prompts inside components or routes. Prompts are first-class artifacts and need to be diffable.
- **Every prompt has a version comment**: `// v1.0 — 2026-05-16 — initial version`. Bump the minor version on tweaks, the major version on behavioral rewrites.
- **Default model is `claude-sonnet-4-6`** for balanced cost/quality. Use `claude-opus-4-7` when the task needs the most capable model (complex reasoning, long-horizon planning) and `claude-haiku-4-5-20251001` for high-volume cheap tasks (classification, extraction, routing). Always justify the choice in a comment when it's not the default.
- **Always set `max_tokens` explicitly.** Defaults are not portable across SDK versions and they bite you on cost.
- **Stream responses** for any output longer than a sentence. Users will tolerate slow streams; they won't tolerate blank screens.
- **Log prompt + response pairs in dev mode** so you can eval changes against historical inputs.
- **Enable prompt caching** on system prompts and tool definitions for any feature with repeated calls — it's the single biggest cost lever.

## Prompt template

```ts
export const MY_PROMPT = {
  version: "1.0",
  system: `You are a...`,
  userTemplate: (input: string) => `Given: ${input}\n\nPlease...`,
}
```

## Prompt change protocol

A prompt change is a behavioral change to the product. Treat it like a code change:

1. Save a snapshot of historical inputs + the current outputs as a baseline.
2. Make the prompt change.
3. Re-run the same inputs and diff outputs.
4. Bump the version comment with a one-line rationale.
