# Passt — Product Skill

## What this is

A German-language speaking-preparation PWA for people relocating to
Germany. Tenure: 3 months pre-move through 6 months post-move. Committed
users practice 15–60 minutes per day during commuting, hygiene, and other
dead time.

The name "Passt" is the everyday German word for "it fits / it works."
It is one of the first words users will hear and say in daily life in
Germany. The product name is the point.

## Who this is for

Adults relocating to Germany with a concrete timeline. They need
functional spoken German for specific life situations: Anmeldung, banking,
healthcare, job interviews, workplace small talk, daily errands. They are
motivated by real deadlines, not by streaks or gamification.

## Method

Users practice "language islands" — small sets of 10–20 sentences covering
one practical scenario. Each island is drilled via audio saturation,
shadowing, and active recall until the sentences are automatic.

## Architecture — locked decisions

- Android PWA only. iOS Safari is deliberately out of scope.
- German only. Do not generalize.
- Every sentence has audio generated once and saved to EU object storage.
  Audio is never regenerated for the same sentence.
- All islands in the system are shareable by construction. No personal
  data ever enters the system. The wizard is the privacy firewall.
- Deterministic local SRS state per sentence (new/learning/weak/stable).
  No server-side personalization. No user profiling. No recommendation
  engine beyond simple pool matching.
- Pool-first content: trial users see islands from the pool. Only paying
  users generate new ones. Paid generations enrich the pool over time.
- Generation pipeline is async from day one. User submits wizard, gets
  fast response, background worker processes LLM + TTS, user's library
  shows "generating..." until ready. Vercel function timeouts make this
  mandatory, good architecture makes it correct.

## Commercial

- €19/month subscription via Stripe.
- 14-day free trial with 4 islands drawn from the pool.
- Trial islands remain accessible read-only after trial expires.
- Paid users unlock wizard access; their generated islands enrich the pool.

## Screens — there are only three

1. **Library:** play card for this week's island, reverse-chronological
   list of all user's islands, "+ New island" button. Trial users see
   their 4 assigned pool islands here.
2. **Island Practice:** four tabs — Listen, Shadow, Recall, Read. One
   island at a time.
3. **Wizard (paid only):** 4 constrained fields + 1 short free-text
   field, then LLM preview with one regenerate, then async background
   audio generation.

Plus an account/settings shell. Not a journey screen.

## The wizard — privacy-critical

Fields:
- Situation: dropdown of ~15 predefined scenarios. No free text.
- Formality: casual / neutral / formal (Sie). Three options only.
- Level: A1-A2 / B1 / B2+. Three options only.
- One-line context: free text, hard-capped at 60 characters. Placeholder
  instructs generic phrasing.

The LLM system prompt must reject personal details (names, specific
employers, addresses, dates) and generate generic transferable sentences
regardless of user input. An LLM pre-pass should detect and strip
personal specifics before generation. This is non-negotiable.

## What this product deliberately is NOT

- Not a general language learning app. German relocation only.
- Not gamified. No streaks, XP, leagues, badges, achievements.
- Not conversational. No AI tutor, no freeform chat, no dialogue simulator.
- Not personalized. No content recommendation based on user behavior.
- Not social. No sharing, friends, leaderboards, community.
- Not a grammar teacher. No grammar lessons, no explanations.
- Not a replacement for real conversation or tutors. It's a speaking
  preparation tool.

When adding features, check against this list. If the feature resembles
anything above, reject it.

## Forbidden patterns

- Do not propose streaks, XP, leagues, levels, or gamification of any
  kind. This product is built for adults with real goals.
- Do not propose adding an AI conversation or tutor mode. That is
  Langua's and Praktika's territory and is deliberately excluded.
- Do not propose personalized recommendations or user profiling.
- Do not store any personal data beyond auth basics (email, hashed
  password) and subscription state.
- Do not suggest supporting additional languages, additional platforms,
  or additional audiences without explicit approval.
- Do not propose a freemium model. Trial, then paid.

## Stack

- Next.js 14+ App Router on Vercel (Hobby for build/beta, Pro at launch)
- Supabase (EU region) for auth + Postgres
- Hetzner Object Storage (EU) for audio files
- OpenRouter for LLM text generation (Mistral Large or equivalent)
- OpenRouter for TTS (Qwen audio initially, ElevenLabs as fallback if
  German quality is poor)
- Stripe for subscriptions
- Inngest (free tier) or Trigger.dev for async generation jobs

## Build order — vertical slices

1. Pipeline works end-to-end: hardcoded user, wizard → LLM → TTS →
   storage → Listen mode plays it. No auth, nothing else.
2. Auth + persistence: Supabase auth, DB schema, library screen.
3. Practice modes: Shadow, Recall, Read with local SRS state.
4. Payments: Stripe, 14-day trial, pool assignment for trial users,
   wizard gating for paid users.
5. PWA polish: manifest, service worker, offline audio caching.

Ship slice 1 before building slice 2. Do not layer horizontally.
