# Architecture

## Pipeline

1. **Resume Processing** — downloads a user-selected resume, extracts PDF text, and converts it into structured JSON.
2. **Job Discovery** — creates up to nine role/location search URLs and sends them to the configured Apify actor.
3. **Normalization** — maps common scraper field variants into a consistent schema and deduplicates listings.
4. **Match Engine** — calculates deterministic dimension scores with controlled skill vocabulary, seniority matching, location matching, job type matching, and education matching.
5. **Recency & Relevance** — adjusts the base fit using a 70/15/15 final-score blend.
6. **Validation + Ranking** — filters invalid scoring rows and deterministically ranks the best five jobs.
7. **Analytics** — computes KPIs and chart-ready data.
8. **Report Delivery** — creates QuickChart visuals, assembles HTML, and sends the report by Gmail.

## Design principles

- Explainability over black-box scoring.
- Missing data receives controlled neutral values instead of automatic zeroes.
- Skill aliases are normalized to canonical names.
- Implied skills are conservative and configurable.
- Ranking has deterministic tie-breakers.
- External credentials are never committed to the repository.

## Customization

The main matching configuration lives inside the **Calculate Job Match** Code node:

- `WEIGHTS`
- `NEUTRAL`
- `SKILL_VOCAB`
- `IMPLIED_SKILLS`
- seniority/title mappings
- location aliases
- education mappings

Recency thresholds and the 70/15/15 blend are in **Recency & Relevance**.
