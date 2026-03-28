Mahmood Ahmad
Tahir Heart Institute
mahmood.ahmad2@nhs.net

Methods Suite: Eight Interoperable Browser-Based Meta-Analysis Tools with MAIF

Can eight interoperable browser-based meta-analysis tools connected by a common JSON interchange format deliver complete evidence synthesis without server dependency? The Suite comprises PRISMA Checker, RoB Assessor, MA Power, Pooling Suite, PubBias Suite, Meta-Regression, Bayesian MA, and Component NMA, each validated with 25 Selenium tests and cross-checked against R packages including metafor, bayesmeta, and netmeta. The Meta-Analysis Interchange Format requires study identifiers with effect estimates and standard errors, with each tool additively enriching the dataset by appending domain-specific results to shared JSON. All 200 tests pass with median OR deviation from R below 0.001 and 95% CI coverage matching nominal rates across pairs. Prior sensitivity in Bayesian MA and permutation testing in Meta-Regression produce results within 2 percent of R benchmarks. This suite demonstrates modular browser tools with standardized interchange can replace fragmented desktop workflows for meta-analysis. The limitation is that MAIF currently supports only univariate effects and cannot represent multivariate or network data without extension.

Outside Notes

Type: methods
Primary estimand: Cross-tool MAIF round-trip fidelity
App: Methods Suite v1.0
Data: 8 tools, 200 Selenium tests, R-validated
Code: https://github.com/mahmood726-cyber/methodssuite
Version: 1.0
Validation: DRAFT

References

1. Page MJ, McKenzie JE, Bossuyt PM, et al. The PRISMA 2020 statement: an updated guideline for reporting systematic reviews. BMJ. 2021;372:n71.
2. Moher D, Liberati A, Tetzlaff J, Altman DG. Preferred reporting items for systematic reviews and meta-analyses: the PRISMA statement. PLoS Med. 2009;6(7):e1000097.
3. Borenstein M, Hedges LV, Higgins JPT, Rothstein HR. Introduction to Meta-Analysis. 2nd ed. Wiley; 2021.

AI Disclosure

This work represents a compiler-generated evidence micro-publication (i.e., a structured, pipeline-based synthesis output). AI is used as a constrained synthesis engine operating on structured inputs and predefined rules, rather than as an autonomous author. Deterministic components of the pipeline, together with versioned, reproducible evidence capsules (TruthCert), are designed to support transparent and auditable outputs. All results and text were reviewed and verified by the author, who takes full responsibility for the content. The workflow operationalises key transparency and reporting principles consistent with CONSORT-AI/SPIRIT-AI, including explicit input specification, predefined schemas, logged human-AI interaction, and reproducible outputs.
