# Health Supplement AI

A research-backed recommendation engine for supplements / nootropics / health
products. Ingest published research → score study trust → aggregate into a
per-claim confidence band → surface recommendations. See `docs/vision.md`.

## Status

**Greenfield and exploratory.** No application code yet — only docs. Treat
architecture and stack as *under evaluation*, not decided (`docs/tech-stack.md`).
Don't lock in a direction or build heavy scaffolding until a choice is actually
made; prefer reversible, minimal steps. This is a "for fun" project run by a
skeptic — bias hard toward simple and cheap.

## Domain principles (stable — these hold regardless of stack)

- **Evidence-first, skeptical by default.** Assume most hyped supplements are
  weak (~2/10 prior); make products *earn* a higher band. Consensus items
  (e.g. creatine) can.
- **Scoring is deterministic and auditable.** The LLM only does structured
  extraction (returns JSON); trust/confidence scores are formulas over that JSON,
  never the model's gut feel. Anyone should be able to trace a score to its inputs.
- **Score specific claims, not products.** "Creatine → cognition", never
  "is creatine good".
- **Report honest uncertainty.** Prefer GRADE-style bands (Very Low → High) over
  false-precision numbers. Always cite the underlying studies.
- **Anecdotes are secondary.** Use them only to surface candidates worth checking
  against literature — never to raise an evidence band. Label them distinctly.
- **Stay legal.** Structure/function wording only (no disease claims); educational,
  not medical advice; disclose affiliate relationships; exclude retracted studies.
- **Don't scrape paywalled/ToS-protected sources** (e.g. Examine.com). Use free,
  open APIs or licensed data.

See `docs/research-pipeline.md` for sources, the trust-index formula, confidence
bands, and legal notes.

## Stack (candidate — see docs/tech-stack.md, held loosely)

Leaning TypeScript end-to-end: Next.js on Vercel + Postgres/`pgvector` + Claude
API (Haiku default) + an embeddings model; heavy ingestion via GitHub Actions;
no Redis until needed. **None of this is committed** — alternatives are live.

## Conventions

- No build/lint/test commands yet — add them here once tooling exists.
- When working with any library/SDK/API, fetch current docs via Context7 first;
  free-tier limits and pricing move fast and the research docs may be stale.
