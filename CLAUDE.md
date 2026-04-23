# Passt

German-language speaking prep for relocation to Germany.
Android PWA. Solo-built via Claude Code.

READ `SKILL.md` BEFORE ANY NON-TRIVIAL CHANGE.

Key constraints (non-exhaustive — SKILL.md is canonical):
- Android-only PWA. No iOS work.
- German only. No multi-language.
- No streaks, no gamification, no AI tutor, no personalization.
- Every wizard generation saves audio permanently; never regenerate.
- Pool-first: trial users see existing islands; paid users generate new.
- €19/mo, 14-day trial, 4 trial islands from pool.
- Async generation pipeline from day one (Vercel function limits).

Stack: Next.js App Router, Vercel, Supabase (EU), Hetzner Object Storage,
OpenRouter for LLM + TTS, Stripe, Inngest for background jobs.

Build in vertical slices. Ship signup→wizard→listen end-to-end first.
Ask questions before writing code on ambiguous requirements.
