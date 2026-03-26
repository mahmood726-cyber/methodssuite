# Meta-Analysis Interchange Format (MAIF) v1.0

A common JSON schema for exchanging study-level data between browser-based meta-analysis tools.

## Core Principle: Additive Enrichment

Each tool adds its own data to the shared format without requiring fields from other tools:

```
PRISMA Checker → adds .prisma (compliance score)
RoB Assessor   → adds .studies[].rob (domain judgments)
Pooling Suite   → adds .results.pooling (tau², I², pooled estimates)
Pub Bias Suite  → adds .results.pubBias (Egger's, trim-and-fill, etc.)
Meta-Regression → adds .results.regression (coefficients, R²)
Component NMA   → adds .studies[].components (treatment decomposition)
Bayesian MA     → adds .results.bayesian (posterior summaries)
MA Power        → reads .studies to compute power from actual data
```

## Minimum Required Fields

Only `studies` with `id`, `yi`, and `sei` are required. Everything else is optional.

```json
{
  "version": "1.0",
  "studies": [
    { "id": "Smith 2020", "yi": -0.35, "sei": 0.12 }
  ]
}
```

## Usage

Every tool in the toolkit has an "Import MAIF" and "Export MAIF" button. Export from one tool, import into the next.
