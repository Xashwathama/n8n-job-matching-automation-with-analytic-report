# n8n Job Matching Automation with Visual Analytics

An end-to-end **n8n job matching workflow** that turns a resume into ranked job opportunities and an analytics-rich email report.

## What it does

```text
Resume
  ↓
PDF Extraction
  ↓
AI Resume Parser
  ↓
Dynamic Job Discovery
  ↓
Job Normalization + Deduplication
  ↓
Deterministic Match Engine
  ↓
Recency + Relevance Scoring
  ↓
Data Validation
  ↓
Top 5 Ranking
  ↓
KPI Analytics
  ↓
Visual Report
  ↓
Email Delivery
```

### Core features

- Parses a resume into structured candidate data with Gemini.
- Builds job-search URLs dynamically from target roles and locations.
- Scrapes LinkedIn job listings through Apify.
- Normalizes inconsistent scraper fields and removes duplicate jobs.
- Uses an explainable deterministic matching engine instead of an opaque LLM score.
- Scores skills, experience, title, location, job type, and education.
- Adds **relevance** and **recency** to the final ranking.
- Validates scoring data before ranking.
- Selects the top 5 opportunities.
- Generates KPI metrics, charts, and an HTML email report.

## Final scoring

The base fit score is composed of:

| Dimension | Weight |
|---|---:|
| Skills | 40% |
| Experience | 20% |
| Job title | 15% |
| Location | 10% |
| Job type | 5% |
| Education | 10% |

The final score then combines:

- **Base fit: 70%**
- **Relevance: 15%**
- **Recency: 15%**

Recency is bucketed from very recent postings to older/unknown postings, while relevance combines target-role title overlap and candidate/job skill overlap.

## Repository structure

- `workflow/job-matching-workflow.json` — sanitized n8n workflow export.
- `examples/sample-resume.json` — synthetic resume input.
- `docs/architecture.md` — architecture and scoring notes.
- `.env.example` — configuration placeholders.
- `.gitignore` — prevents common secrets and personal files from being committed.

## Import and setup

1. Import `workflow/job-matching-workflow.json` into your n8n instance.
2. Create/connect your own credentials for:
   - Google Drive
   - Google Gemini
   - Apify
   - Gmail
3. Open the corresponding nodes and select your credentials.
4. Replace the placeholder resume file ID.
5. Replace the placeholder Apify actor ID if you use a different scraper.
6. Replace the recipient email in the Gmail node.
7. Test manually before activating the workflow.

**Important:** credentials are intentionally removed from this public export. They must be reconnected after import.

## Privacy

This repository contains no personal resume, private Google Drive ID, Gmail recipient, OAuth credential ID, API token, or private execution data.

Use your own credentials and resume when running the workflow.

## Example output

A successful run produces ranked jobs with:

- match score
- score breakdown
- matched and missing skills
- recency score
- relevance score
- match tier
- job URL
- KPI summary
- visual charts
- HTML email report

## License

MIT
