# Research Pipeline — Sources, Scoring, Legal

> Status: **proposed v0 design, not built.** The scoring formulas below are a
> defensible starting point, not ground truth. Expect to tune weights once real
> data exists. Don't treat any single number as authoritative.

## Data sources (biomedical literature)

Start minimal. Recommended v0 trio — all free, no scraping:

- **Europe PMC** (primary) — metadata + abstracts + open-access full text +
  cross-links, in one keyless REST API.
- **PubMed / NCBI E-utilities** (companion) — MeSH terms and the
  publication-type field ("Meta-Analysis", "Randomized Controlled Trial", …),
  which directly drive study-type classification. Free; 3 req/s, 10 with a key.
- **Crossref** (integrity) — hosts the Retraction Watch database; query by DOI to
  flag retracted/corrected papers.

Defer to v1+: OpenAlex (citation graph), ClinicalTrials.gov (sample size,
registered-but-unpublished signal), CORE/Unpaywall (extra free full text).

**Examine.com:** no public API yet (in development), and it's a paid editorial
product. **Do not scrape it.** Use it only as a human reference for methodology,
or pursue licensing via their request form. Build our own grades from primary
literature.

## Trust index (per study, 0–100)

Key principle: **the LLM only does structured extraction; the score is a
deterministic formula over its JSON output.** Keeps scores reproducible and
auditable, not vibes from a model.

1. **Study-type weight (~40 pts)** — from PubMed publication-type, no LLM needed.
   Meta-analysis/SR 40 · RCT 32 · cohort 22 · case-control 16 · cross-sectional
   12 · case report 6 · animal 4 · in-vitro 2.
2. **Sample size (~20 pts)** — log-bucketed.
3. **Methodology rigor (~20 pts)** — LLM extracts: randomized, blinded
   (double > single), placebo-controlled, pre-registered, COI/funding disclosed.
4. **Influence/venue (~15 pts)** — citations-per-year (OpenAlex, later), capped to
   avoid rewarding hype; recent papers get a neutral floor.
5. **Integrity gate** — retracted → trust 0 / excluded; correction / expression of
   concern → −15.

## Confidence / efficacy (per supplement × specific claim)

Always scope to a **specific claim** ("creatine → cognition"), never "is X good".

- Trust-weighted vote on effect direction (benefit / null / harm).
- Trust-weighted mean effect size (separate "works strongly" from "significant but
  trivial").
- Consistency: do high-trust studies agree? Disagreement caps confidence.
- Ceiling by best available evidence: animal/in-vitro-only caps at "Very Low"
  regardless of count.
- Publication-bias heuristic flag (small-study-only, industry-funded-only,
  registered-but-unpublished) → confidence haircut + surfaced caveat.

**Output a GRADE-style band (Very Low / Low / Moderate / High)** plus a one-line
direction-and-magnitude statement. Keep any internal number internal — bands are
more honest than false-precision scores. Default skeptic prior: most
heavily-hyped supplements start low (~2/10), creatine-tier consensus items can
earn higher.

Known v0 weakness to disclose: publication-bias handling is heuristic, not a
proper Egger's-test-style analysis.

## Anecdotal evidence (clearly secondary)

Sources: r/Nootropics, r/Supplements, longevity forums, SSC/ACX surveys. Use only
to **flag candidates worth checking against the literature** — never to raise an
evidence band. Weight long-term, no-financial-incentive reports; require N
independent reports; label "anecdotal" distinctly from "research-backed" in any UI.
Caveats: selection/survivorship bias, placebo, undisclosed shilling. Reddit's API
free tier is non-commercial only — affiliate links make us commercial, so budget
the paid tier or don't store/redistribute Reddit content.

## Legal / risk (real, keep in view)

- **FDA:** structure/function claims OK ("supports focus"); **no disease claims**
  ("treats anxiety"). Include the DSHEA disclaimer on labeling-style content.
- **FTC** (bigger risk): efficacy claims must be substantiated by competent,
  reliable scientific evidence; don't let confidence scores imply medical claims.
- **Affiliate (Amazon Associates):** required verbatim disclosure, clear and
  conspicuous; fines are per-violation. Affiliate revenue = commercial for ToS
  everywhere.
- **Mitigations:** site-wide "educational, not medical advice" disclaimer; always
  cite the actual studies (transparency is the defense); structure/function
  wording; never advise stopping prescribed meds; exclude retracted studies.

## v0 pipeline (minimal, cheap)

1. Seed ~10–20 supplement×claim pairs (creatine→cognition, melatonin→sleep onset,
   ashwagandha→stress, …).
2. Ingest from Europe PMC + PubMed; flag retractions via Crossref.
3. Per-study trust index = deterministic formula over LLM-extracted JSON.
4. Per-claim confidence = trust-weighted direction/magnitude/consistency → GRADE
   band + one-line statement.
5. Present with citations, evidence band, safe wording, disclaimers.
6. Defer citations graph, trials registry, anecdotes, Examine licensing.
