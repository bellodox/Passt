# Passt

German-language speaking prep for relocation to Germany. Android PWA.

**Read [`SKILL.md`](./SKILL.md) before contributing.** It is the canonical
source for locked product and architectural decisions.

## Stack

Next.js App Router on Vercel, Supabase (EU), Hetzner Object Storage,
OpenRouter (LLM + TTS), Stripe, Inngest.

## Prerequisites

- Node.js 20 LTS (pinned in `.nvmrc`; any `>=20` works).
- pnpm 10.x (`corepack enable` recommended).

## Quickstart

```bash
pnpm install
pnpm dev           # http://localhost:3000
```

No API keys or `.env.local` are needed for the current scaffold.
Environment variables for later slices are documented in `.env.example`.

## Scripts

| Script              | Purpose              |
| ------------------- | -------------------- |
| `pnpm dev`          | Next.js dev server   |
| `pnpm build`        | Production build     |
| `pnpm start`        | Run production build |
| `pnpm lint`         | ESLint               |
| `pnpm typecheck`    | `tsc --noEmit`       |
| `pnpm format`       | Prettier write       |
| `pnpm format:check` | Prettier check       |

## Build order

Features ship in vertical slices (see `SKILL.md` § Build order). This
scaffold is slice 0 — buildable shell with product-context files. Slice
1 lands the `wizard → LLM → TTS → storage → Listen` pipeline end-to-end.
