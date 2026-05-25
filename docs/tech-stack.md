# Tech Stack — Candidate, Not Decided

> Status: **research-informed leaning, nothing committed.** This is a greenfield
> "for fun" project. Treat every row below as a default to challenge, not a
> decision. Free-tier limits and prices below were gathered May 2026 and shift
> often — re-verify the two or three numbers that would actually change a choice
> before committing to them.

## Constraints (from the vision)

- Cheapest viable; ideally $0/mo at hobby scale.
- Most flexible / least lock-in we can get away with.
- Solo dev. Don't over-engineer. Simplest viable solution wins.
- Needs eventually: AI/LLM analysis, vector search, scheduled ingestion jobs,
  caching, a web client.

## Current leaning (one candidate, held loosely)

| Layer | Candidate | One-line reason | Confidence |
|---|---|---|---|
| Frontend + API + light cron | Next.js on Vercel Hobby | One deploy target for UI + API routes | medium |
| Heavy/long ingestion jobs | GitHub Actions (scheduled) | Free, no function timeout, runs any cadence | medium-high |
| DB (relational + vector) | Postgres + `pgvector` (Neon) | One DB for rows *and* embeddings; avoids a separate vector DB | high on "one Postgres", medium on Neon-vs-Supabase |
| LLM analysis | Claude API — Haiku default, Sonnet for hard scoring | Pay-per-use, no idle cost | medium-high |
| Embeddings | Voyage (`voyage-4-lite`) or OpenAI small | Generous free tier; cheap after | low-medium (vendor TBD) |
| Caching | None yet | No traffic to cache; YAGNI | high (defer) |
| Language | TypeScript end-to-end | One language/repo for a solo dev | medium |

Rough cost at hobby scale: **~$0/mo**, the only variable being Claude API tokens
(small if ingestion uses Batch API + prompt caching + caching results in Postgres).

## Live tradeoffs to resolve later (don't pre-decide)

- **Neon vs Supabase.** Neon scales to zero and auto-resumes (~sub-second), no
  manual unpause. Supabase pauses free projects after ~7 days idle (needs a
  keep-alive ping) but is batteries-included (Auth, Storage, `pg_cron`). Pick on
  whether bundled backend > hands-off idling.
- **Next.js vs React+Vite vs Nuxt/Vue.** Next collapses front+back+cron into one
  deploy; Vite is leaner but needs a separate API host; Vue/Nuxt is fine but the
  Anthropic/Vercel ecosystem leans React. No strong reason to pick yet.
- **All-TS vs TS web + Python pipeline.** Default all-TS (one toolchain). Only
  reach for Python if a Python-only ML/NLP library becomes load-bearing.
- **Vercel commercial-use clause.** Hobby tier disallows commercial use; if this
  ever monetizes (affiliate links count), that's the trigger to reassess hosting.

## Deliberately deferred (YAGNI)

Redis/caching, a managed/separate vector DB (Pinecone/Qdrant), a Python pipeline,
paid hosting / precise sub-daily cron, auth, multi-tenant, observability — none
until real usage demands them.

## The one thing worth getting right early

If/when ingestion is built, wire it through **Claude Batch API (−50%) + prompt
caching (−90% on repeated input) + caching parsed results in Postgres** so the
same paper is never re-analyzed. Cheap to adopt early; it's the only line item
that scales with usage.
