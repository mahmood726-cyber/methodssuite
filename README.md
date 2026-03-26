# Methods Suite

A unified open-access toolkit of 8 browser-based meta-analysis tools connected by the Meta-Analysis Interchange Format (MAIF).

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Overview

Methods Suite is the umbrella project for 8 interoperable browser-based meta-analysis tools spanning the full evidence synthesis workflow -- from protocol compliance (PRISMA Checker) and risk of bias assessment (RoB Assessor), through power planning (MA Power), pooling (Pooling Suite), publication bias detection (PubBias Suite), meta-regression (MetaRegression Workbench), Bayesian analysis (Bayesian MA), and component decomposition (Component NMA). All tools share a common JSON interchange format (MAIF v1.0) that enables seamless data flow across the toolkit.

## Toolkit Components

| Tool | Description | Tests |
|------|-------------|-------|
| [PRISMA Checker](https://github.com/mahmood726-cyber/prisma-checker) | PRISMA 2020 compliance (27 items, 6 extensions) | 25 |
| [RoB Assessor](https://github.com/mahmood726-cyber/rob-assessor) | Risk of bias (RoB 2 + ROBINS-I) | 25 |
| [MA Power](https://github.com/mahmood726-cyber/ma-power) | Power and sample size for meta-analysis | 25 |
| [Pooling Suite](https://github.com/mahmood726-cyber/pooling-suite) | 10 tau-squared estimators, 3 CI methods, influence diagnostics | 25 |
| [PubBias Suite](https://github.com/mahmood726-cyber/pub-bias-suite) | 12 publication bias methods, 3 funnel plot types | 25 |
| [Meta-Regression](https://github.com/mahmood726-cyber/meta-regression) | Mixed-effects meta-regression with permutation tests | 25 |
| [Bayesian MA](https://github.com/mahmood726-cyber/bayesian-ma) | Bayesian random-effects with prior sensitivity | 25 |
| [Component NMA](https://github.com/mahmood726-cyber/component-nma) | Additive component network meta-analysis | 25 |

## MAIF: Meta-Analysis Interchange Format

MAIF v1.0 is a JSON schema that provides a common data format for exchanging study-level data between tools. Each tool enriches the dataset additively -- only `studies` with `yi` and `sei` are required; all other fields are optional extensions added by specific tools.

### Core Principle: Additive Enrichment

```
PRISMA Checker  --> adds .prisma (compliance score)
RoB Assessor    --> adds .studies[].rob (domain judgments)
Pooling Suite   --> adds .results.pooling (tau-squared, I-squared, pooled estimates)
PubBias Suite   --> adds .results.pubBias (Egger's, trim-and-fill, etc.)
Meta-Regression --> adds .results.regression (coefficients, R-squared)
Component NMA   --> adds .studies[].components (treatment decomposition)
Bayesian MA     --> adds .results.bayesian (posterior summaries)
MA Power        --> reads .studies to compute power from actual data
```

### Minimum Required Fields

```json
{
  "version": "1.0",
  "studies": [
    { "id": "Smith 2020", "yi": -0.35, "sei": 0.12 }
  ]
}
```

Every tool in the suite has "Import MAIF" and "Export MAIF" buttons. Export from one tool, import into the next.

### Schema

The full JSON Schema is available at [`schema/ma-interchange.schema.json`](schema/ma-interchange.schema.json).

## Typical Workflow

1. **Plan**: Use PRISMA Checker to verify protocol compliance and MA Power to estimate required sample size
2. **Assess**: Use RoB Assessor to perform risk of bias assessment (export MAIF with RoB annotations)
3. **Pool**: Import MAIF into Pooling Suite for primary analysis with 10 estimators
4. **Check bias**: Export MAIF to PubBias Suite for 12-method publication bias assessment
5. **Explore**: Import into Meta-Regression to test moderators, or Bayesian MA for prior sensitivity
6. **Decompose**: For multi-component interventions, use Component NMA

## Quick Start

1. Download any tool's single HTML file
2. Open in any modern browser
3. No installation, no server, no dependencies -- works fully offline
4. Use MAIF export/import to move data between tools

## Validation

- 200/200 total Selenium tests pass across 8 tools (25 per tool)
- Individual tools cross-validated against R packages: metafor, meta, bayesmeta, netmeta, puniform

## Citation

If you use this toolkit, please cite:

> Ahmad M. Methods Suite: An integrated open-access toolkit for browser-based meta-analysis with MAIF interoperability. 2026. Available at: https://github.com/mahmood726-cyber/methods-suite

## Author

**Mahmood Ahmad**
Royal Free Hospital, London, United Kingdom
ORCID: [0009-0003-7781-4478](https://orcid.org/0009-0003-7781-4478)

## License

MIT
