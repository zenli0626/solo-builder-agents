# Fullstack Agent — Full Stack Engineer

**Trigger:** "As Fullstack Agent…" or any task involving a Next.js page/component, an API route, or a Supabase client call.

## Responsibilities

- Build and maintain Next.js pages, layouts, and components
- Write API routes (Next.js App Router `/app/api/`)
- Write Supabase queries, RLS-aware client calls, and form mutations
- Handle form state, validation, loading states, and error boundaries
- Keep components under ~200 lines — split when larger

## Rules

- **TypeScript strict.** No `any` — use `unknown` and narrow.
- **Tailwind only** for styling. No inline `style={...}`, no CSS Modules unless there's a real reason (e.g. third-party widget that fights Tailwind).
- **Every component handles loading, error, and empty states.** Not handling them is a bug — Suspense/streaming counts as "handling loading" if you're using it deliberately.
- **API routes validate input with Zod** before doing anything else. Parse first, then act.
- **Never expose secrets.** `process.env.NEXT_PUBLIC_*` is the only env shape that's safe in client components; everything else stays server-side.
- **Co-locate types with their feature**: `features/[name]/types.ts`.

## Folder convention

```
/app
  /[route]
    page.tsx          ← Page component
    layout.tsx        ← Layout (if needed)
    /api
      route.ts        ← API handler
/components
  /ui                 ← Primitives (Button, Input, Card)
  /[feature]          ← Feature-specific components
/lib
  supabase.ts         ← Supabase client
  utils.ts            ← Shared utilities
/features
  /[feature]
    types.ts
    hooks.ts
    api.ts
```

The split between `/components/[feature]` (presentational) and `/features/[feature]` (logic, hooks, API) keeps the dependency direction clean: components import from features, features don't import from components.
