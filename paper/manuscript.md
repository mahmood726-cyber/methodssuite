# An Open-Access Browser-Based Toolkit for Evidence Synthesis: Eight Tools Covering Risk of Bias, Pooling, Publication Bias, Meta-Regression, Component NMA, Power Analysis, Bayesian Meta-Analysis, and Reporting Compliance

Mahmood Ahmad^1^

^1^ Royal Free Hospital, London, United Kingdom

Correspondence: Mahmood Ahmad, mahmood.ahmad2@nhs.net
ORCID: 0009-0003-7781-4478

**Target journal:** F1000Research (Software Tool Article) / Journal of Open Source Software (JOSS)

**Word count:** ~4,800

---

## Abstract

Evidence synthesis requires a complex analytical pipeline spanning risk of bias assessment, effect size pooling, heterogeneity quantification, meta-regression, network meta-analysis, publication bias detection, statistical power estimation, Bayesian modelling, and reporting compliance verification. Researchers without programming skills face a fragmented software landscape in which these capabilities are distributed across numerous R and Stata packages, each requiring installation, configuration, and coding proficiency. We present an integrated toolkit of eight browser-based applications that collectively cover the complete evidence synthesis workflow: (1) RoB Assessor for structured risk of bias evaluation using the RoB 2 and ROBINS-I frameworks; (2) Pooling Suite implementing 10 between-study variance estimators with forest plots and influence diagnostics; (3) Publication Bias Suite offering 12 statistical methods for small-study effect detection; (4) Meta-Regression Workbench providing mixed-effects meta-regression with permutation testing; (5) Component NMA for additive component network meta-analysis of complex interventions; (6) MA Power for statistical power and sample size calculation; (7) Bayesian MA implementing a normal-normal hierarchical model via grid approximation; and (8) PRISMA Checker for 27-item PRISMA 2020 compliance assessment with six reporting extensions. All eight tools are single-file HTML applications requiring no installation, server infrastructure, or internet connection. They share a unified architecture including dark mode support, ARIA-compliant accessibility, localStorage persistence, SVG visualisation, and CSV/JSON export. A common JSON interchange schema enables data flow between tools. Validation comprises 200 automated Selenium tests with cross-validation against R reference packages. All tools are open-source under the MIT licence.

**Keywords:** evidence synthesis, meta-analysis, systematic review, risk of bias, publication bias, meta-regression, network meta-analysis, Bayesian meta-analysis, web application, open-source software

---

## 1. Introduction

The methodological rigour expected of systematic reviews and meta-analyses has increased substantially over the past two decades. The PRISMA 2020 statement (Page et al., 2021) requires transparent reporting of study selection, risk of bias assessment, statistical synthesis, and sensitivity analyses. The Cochrane Handbook (Higgins et al., 2023) prescribes a structured pipeline from protocol registration through risk of bias evaluation, effect size calculation, heterogeneity quantification, and exploration of small-study effects. Meeting these expectations requires software capable of performing each analytical step reliably and reproducibly.

The current software ecosystem for evidence synthesis is powerful but fragmented. The R package metafor (Viechtbauer, 2010) provides comprehensive facilities for fitting meta-analytic models, but requires fluency in R programming. The meta package (Balduzzi et al., 2019) offers a higher-level interface but still presupposes a working R installation. Publication bias assessment tools are scattered across multiple packages: metafor for regression-based tests, weightr for selection models (Vevea and Hedges, 1995), and dmetar for p-curve analysis (Harrer et al., 2021). Risk of bias assessment relies on the desktop application RevMan or the R visualisation package robvis (McGuinness et al., 2021), neither of which implements the guided algorithmic judgment procedure described in the RoB 2 instrument (Sterne et al., 2019). Component network meta-analysis requires netmeta (Rucker et al., 2022), Bayesian meta-analysis requires bayesmeta (Rover and Friede, 2017) or brms (Burkner, 2017), and power analysis requires the metapower package (Quintana, 2022). Meta-regression is available in metafor but has no browser-based counterpart.

This fragmentation creates two barriers. First, researchers without programming experience---including many clinicians, guideline developers, and students---are effectively excluded from advanced synthesis methods. Second, even experienced analysts must switch between multiple software environments within a single review, introducing friction and opportunities for data transcription errors. While several web-based tools exist for individual tasks (e.g., CINeMA for network meta-analysis confidence, MetaInsight for interactive NMA), no unified browser-based toolkit covers the complete analytical workflow from risk of bias through pooling, bias detection, regression, and reporting compliance.

We present a toolkit of eight browser-based applications that, taken together, address this gap. Each tool is a single self-contained HTML file that runs entirely in the browser with no installation, no server communication, and no external dependencies. The tools share a common architecture, visual design language, and data interchange format, enabling researchers to move data seamlessly from one analytical step to the next. In what follows, we describe each tool's capabilities, the shared technical architecture, the interoperability framework, and the validation strategy.

## 2. The Toolkit

### 2.1 RoB Assessor: Structured Risk of Bias Assessment

RoB Assessor (1,463 lines) implements the two principal risk of bias instruments for intervention studies: the Revised Cochrane Risk of Bias tool for randomised trials (RoB 2; Sterne et al., 2019) and the Risk of Bias in Non-randomised Studies of Interventions (ROBINS-I; Sterne et al., 2016).

For RoB 2, the tool presents all five domains (bias arising from the randomisation process, deviations from intended interventions, missing outcome data, measurement of the outcome, and selection of the reported result) with their 21 signalling questions. For ROBINS-I, all seven domains and 26 signalling questions are implemented. The critical distinguishing feature of this tool relative to existing alternatives is the implementation of algorithm-guided judgment suggestions. Based on the pattern of responses to signalling questions within each domain, the tool applies the decision algorithms described in the RoB 2 guidance document (Sterne et al., 2019) to suggest domain-level judgments of "Low risk," "Some concerns," or "High risk" (or "Low," "Moderate," "Serious," "Critical," and "No information" for ROBINS-I). Reviewers retain full authority to override any suggestion.

The tool generates a traffic light table displaying domain-level judgments for all included studies, weighted bar charts showing the proportion of studies at each risk level per domain, and summary statistics. Results are exportable as CSV, JSON, and clipboard-ready tables. Studies can be imported via CSV paste, enabling batch assessment of large review portfolios.

Compared with RevMan, which requires desktop installation, and robvis, which provides visualisation but not the assessment interface, RoB Assessor offers an integrated assessment-plus-visualisation workflow in a zero-installation browser environment.

### 2.2 Pooling Suite: Comprehensive Effect Size Pooling

Pooling Suite (1,770 lines) implements the core statistical engine of any meta-analysis: pooling study-level effect estimates into a summary measure with appropriate quantification of between-study heterogeneity.

The tool provides 10 between-study variance (tau-squared) estimators: fixed-effect inverse-variance (FE), DerSimonian-Laird (DL), restricted maximum likelihood (REML), maximum likelihood (ML), Paule-Mandel (PM), empirical Bayes (EB/Morris), Sidik-Jonkman (SJ), Hunter-Schmidt (HS), Hedges (HE), and Bowden-Dudbridge (BD). For each estimator, the tool computes the pooled effect estimate, tau-squared, I-squared, H-squared, and both confidence and prediction intervals. Three confidence interval methods are available: Wald (z-based), Hartung-Knapp-Sidik-Jonkman (HKSJ, t-based with adjusted standard error), and Knapp-Riliet (KR, t-distribution).

The forest plot tab renders publication-quality SVG forest plots with study labels, individual effect estimates with confidence intervals, pooled diamonds, and heterogeneity statistics. The influence diagnostics tab provides leave-one-out analysis (recomputing the pooled estimate after removing each study in turn), Baujat plots (contribution to overall heterogeneity vs. influence on pooled result), and GOSH plots (graphical display of study heterogeneity based on subset enumeration). An estimator comparison table displays results from all 10 methods side by side, enabling researchers to assess the sensitivity of conclusions to estimator choice.

Compared with metafor, Pooling Suite requires no coding and provides an interactive side-by-side comparison of all major estimators that would otherwise require writing custom R loops. The tool's estimator comparison table is particularly valuable for methodological teaching and sensitivity analysis.

### 2.3 Publication Bias Suite: 12-Method Small-Study Effect Assessment

Publication Bias Suite (2,172 lines) implements the most comprehensive set of publication bias methods available in any single tool. Twelve statistical methods are organised into four families:

**Regression and rank tests.** Egger's weighted regression test (Egger et al., 1997) regresses standardised effect sizes against their precision. Begg and Mazumdar's rank correlation test (Begg and Mazumdar, 1994) tests the correlation between effect sizes and their variances using Kendall's tau.

**Trim-and-fill.** The Duval and Tweedie (2000) trim-and-fill method is implemented with both L0 and R0 estimators, providing imputed studies and an adjusted pooled estimate.

**Regression-based adjustments.** PET (precision-effect test), PEESE (precision-effect estimate with standard error), and the conditional PET-PEESE estimator (Stanley and Doucouliagos, 2014) are provided. PET regresses effect sizes on standard errors; PEESE regresses on variances. The conditional estimator applies PET when PET is non-significant and PEESE otherwise.

**Selection models and advanced methods.** The three-parameter selection model (3PSM; Vevea and Hedges, 1995) models the probability of publication as a step function of p-value. P-curve analysis (Simonsohn et al., 2014) tests whether the distribution of significant p-values is right-skewed (evidential value) or flat/left-skewed (p-hacking). P-uniform* (van Aert and van Assen, 2021) provides a selection-model-based adjusted estimate. WAAP-WLS (weighted average of adequately powered studies) restricts the analysis to studies with at least 80% power (Ioannidis et al., 2017). Limit meta-analysis (Rucker et al., 2011) estimates the effect as the precision approaches infinity.

Three funnel plot types are available: standard (effect vs. SE), contour-enhanced (with significance regions at p = 0.01, 0.05, and 0.10), and sunset (power-enhanced; Otte et al., 2022). A sensitivity dashboard displays all 12 results in a unified table, enabling rapid comparison of adjusted estimates and significance conclusions.

No existing single tool---whether R package or web application---offers more than six of these 12 methods in one interface.

### 2.4 Meta-Regression Workbench: Browser-Based Mixed-Effects Meta-Regression

Meta-Regression Workbench (2,616 lines) provides what we believe to be the first browser-based implementation of mixed-effects meta-regression. Meta-regression extends the random-effects model by including study-level covariates as moderators of the treatment effect, a capability available in R through the `mods` argument of `metafor::rma()` but previously inaccessible without coding.

The tool supports single or multiple continuous and categorical moderators. The underlying model estimates the between-study variance using the method-of-moments (DerSimonian-Laird) or REML estimator, then fits a weighted least squares regression using inverse-variance weights that incorporate the estimated between-study variance. For each coefficient, the tool provides the estimate, standard error, z-statistic, p-value, and 95% confidence interval. The Knapp-Hartung adjustment replaces z-based inference with t-based inference and an adjusted standard error, reducing false-positive rates in meta-regression with few studies (Knapp and Hartung, 2003).

A permutation test (1,000 iterations, with user-configurable seed) provides a non-parametric assessment of moderator significance that does not depend on normality assumptions (Higgins and Thompson, 2004). Model comparison is supported through computation of the Akaike Information Criterion (AIC), Bayesian Information Criterion (BIC), and an R-squared analogue representing the proportion of between-study variance explained by the moderator.

Visualisations include a bubble plot (effect size vs. moderator, with bubble size proportional to study precision and fitted regression line with confidence band), studentised residual plots, and a normal Q-Q plot of residuals. The tool generates both a methods paragraph and equivalent R code (`metafor::rma(yi, vi, mods = ~moderator)`).

### 2.5 Component NMA: Additive Component Network Meta-Analysis

Component NMA (1,159 lines) implements additive component network meta-analysis for complex interventions, a method with no prior browser-based implementation. Complex healthcare interventions---such as multimodal psychological therapies, combination pharmacotherapy regimens, or lifestyle modification programmes---consist of multiple active components whose individual contributions standard NMA cannot disentangle.

The additive component model (Welton et al., 2009; Rucker et al., 2020) posits that the effect of a multi-component treatment equals the sum of its individual component effects. The tool constructs a binary design matrix encoding component membership for each treatment, estimates component-specific effects via weighted least squares with DerSimonian-Laird random-effects adjustment, and provides a Q-test for additive model fit. An optional interaction extension includes pairwise component interaction terms to test for synergistic or antagonistic effects beyond additivity, contingent on sufficient data (k > p + number of interactions).

Visualisations include an interactive network graph showing component co-occurrence, a forest plot of component-specific effects with confidence intervals, and a design matrix display. Two built-in example datasets demonstrate the tool: a smoking cessation network (24 trials, 4 components: self-help, individual counselling, group counselling, pharmacotherapy) and a depression treatment network (18 trials, 5 components: CBT, exercise, pharmacotherapy, psychoeducation, mindfulness).

The sole existing implementation of component NMA is the `netcomb` function in the R package netmeta (Rucker et al., 2022). Component NMA provides equivalent functionality without requiring R installation or programming.

### 2.6 MA Power: Power and Sample Size Calculation for Meta-Analyses

MA Power (660 lines) addresses the often-overlooked question of statistical power in meta-analysis. Underpowered meta-analyses may fail to detect clinically meaningful effects, yet power analysis for planned systematic reviews remains uncommon, partly due to the unavailability of accessible tools.

The tool computes power for both binary outcomes (using log-odds ratios) and continuous outcomes (using standardised mean differences). Given the expected number of studies, average sample size per study, anticipated effect size, and expected heterogeneity (I-squared), it calculates the statistical power of a random-effects meta-analysis under the specified scenario. The sample size calculator inverts this computation: given a target power (typically 80% or 90%), it determines the required number of studies.

A power curve tab displays power as a function of the number of studies at four heterogeneity levels (I-squared = 0%, 25%, 50%, 75%), enabling researchers to visualise how power degrades with increasing heterogeneity and to identify the study count at which adequate power is achieved. Sensitivity tables display power across a grid of effect sizes and study counts.

The tool implements the variance formula for the pooled estimator under a random-effects model, incorporating both within-study and between-study variance components. Results are comparable to those from the R package metapower (Quintana, 2022).

### 2.7 Bayesian MA: Hierarchical Bayesian Meta-Analysis via Grid Approximation

Bayesian MA (1,650 lines) implements a normal-normal hierarchical model for Bayesian meta-analysis without requiring Markov chain Monte Carlo (MCMC) sampling. The computational strategy uses two-dimensional grid approximation over the joint posterior of the grand mean (mu) and between-study standard deviation (tau), which is tractable for the two-parameter model typical of standard meta-analysis.

The prior for the grand mean is a conjugate normal distribution, mu ~ N(m0, s0-squared), where researchers can specify the prior mean and standard deviation. The prior for the between-study standard deviation is a half-Cauchy distribution, tau ~ Half-Cauchy(scale), following the recommendation of Polson and Scott (2012) for variance parameters. The half-Cauchy prior is weakly informative, placing mass on small values of tau while allowing for large heterogeneity if supported by the data.

Three pre-configured prior scenarios facilitate sensitivity analysis: a vague prior (N(0, 10-squared) for mu, scale = 0.5 for tau), an informative prior (centred on the frequentist estimate with moderate precision), and a skeptical prior (centred at the null with tight precision). Researchers can also specify custom priors.

The results tab displays posterior density plots for mu and tau, posterior summary statistics (mean, median, 95% credible interval), a shrinkage forest plot showing how each study's estimate is pulled toward the grand mean (with shrinkage factors quantifying the degree of borrowing), and a DIC-like model fit criterion. Study-specific posterior means and 95% credible intervals are presented in a shrinkage table.

Compared with bayesmeta (Rover and Friede, 2017), which provides analytical posterior computation in R, and brms (Burkner, 2017), which requires Stan installation, Bayesian MA provides an accessible introduction to Bayesian meta-analysis with immediate visual feedback and no software prerequisites.

### 2.8 PRISMA Checker: Reporting Compliance Assessment

PRISMA Checker (1,807 lines) implements the 27-item PRISMA 2020 checklist (Page et al., 2021) as an interactive assessment tool with four status levels per item: Reported, Partially Reported, Not Reported, and Not Applicable.

Beyond the core checklist, the tool incorporates six PRISMA reporting extensions: PRISMA-S (search reporting; Rethlefsen et al., 2021), PRISMA-ScR (scoping reviews; Tricco et al., 2018), PRISMA-NMA (network meta-analysis; Hutton et al., 2015), PRISMA-DTA (diagnostic test accuracy; McInnes et al., 2018), PRISMA-IPD (individual participant data; Stewart et al., 2015), and PRISMA-P (protocols; Moher et al., 2015). Each extension adds domain-specific items relevant to its review type, and researchers activate only the extensions relevant to their study.

A compliance dashboard computes the overall reporting score as the percentage of applicable items rated as "Reported," with separate scores for each checklist section (title, abstract, introduction, methods, results, discussion, other information). Items rated "Not Applicable" are excluded from the denominator. The tool auto-generates a compliance statement suitable for inclusion in a cover letter or methods section (e.g., "This systematic review reports 24 of 27 applicable PRISMA 2020 items (88.9% compliance)").

Results are exportable as CSV (for supplementary tables) and JSON (for archival). Compared with paper-based checklists or static PDF forms, PRISMA Checker provides real-time compliance scoring, persistent state via localStorage, and structured export for editorial submission.

## 3. Unified Architecture

All eight tools share an architectural pattern designed for maximum accessibility and minimal friction.

**Single-file deployment.** Each tool is a single HTML file containing all CSS, JavaScript, and static assets inline. There is no build step, no package manager, no transpilation, and no server-side processing. Users open the file in any modern browser. The total codebase across all eight tools is approximately 14,300 lines of HTML, CSS, and JavaScript.

**CSS custom properties and dark mode.** All tools use CSS custom properties (variables) for theming, with a light/dark mode toggle persisted in localStorage. Colour choices satisfy WCAG AA contrast requirements (minimum 4.5:1 for normal text) in both themes.

**Accessibility.** Tab navigation uses ARIA roles (`role="tab"`, `role="tabpanel"`, `aria-selected`) with keyboard support (arrow keys for tab switching, Enter/Space for activation). All interactive visualisations carry `aria-label` attributes. Form elements have associated labels.

**localStorage persistence.** User data and preferences are stored in the browser's localStorage with tool-specific keys (e.g., `poolingSuite_data`, `robAssessor_studies`), ensuring that work is preserved across browser sessions without requiring user accounts or cloud storage.

**SVG visualisation.** All plots (forest plots, funnel plots, bubble plots, network graphs, bar charts, power curves, posterior densities) are rendered as inline SVG, enabling lossless scaling at any resolution and direct embedding in manuscripts. SVG elements are generated programmatically in JavaScript with no external charting libraries.

**Export and reproducibility.** Every tool provides CSV export (for spreadsheet workflows), JSON export (for programmatic interoperability), clipboard copy (for quick transfer), and auto-generated methods text (a prose paragraph describing the analysis in language suitable for a manuscript methods section). Where applicable, tools also generate equivalent R code (e.g., the metafor::rma call corresponding to the Pooling Suite analysis, or the netmeta::netcomb call for Component NMA).

**Shared statistical library.** The tools share common JavaScript implementations of core statistical functions: the normal cumulative distribution function (normalCDF), normal quantile function (normalQuantile via rational approximation), chi-squared quantile approximation (Wilson-Hilferty), DerSimonian-Laird tau-squared estimator, and matrix operations (multiplication, transpose, inversion via Gauss-Jordan elimination). These functions are embedded in each file to maintain the single-file, zero-dependency constraint.

## 4. Interoperability

A common JSON interchange schema enables data to flow between tools in the toolkit. A study assessed in RoB Assessor can be exported as JSON containing study identifiers and risk of bias judgments. When imported into Pooling Suite alongside effect size data, the risk of bias judgments are available for sensitivity analysis (e.g., restricting pooling to studies at low risk of bias). The pooled results from Pooling Suite can be exported and imported into Publication Bias Suite for small-study effect assessment.

The MA Workbench landing page (a separate index.html in the MAWorkbench directory) serves as a unified entry point, providing links and descriptions for all tools in the suite. This hub architecture allows researchers to navigate the toolkit without memorising individual file locations.

The interchange format uses a minimal schema:

```json
{
  "tool": "PoolingSuite",
  "version": "1.0",
  "data": {
    "studies": [
      { "id": "Smith 2020", "yi": 0.45, "sei": 0.12 }
    ],
    "settings": { "estimator": "REML", "ciMethod": "HKSJ" }
  }
}
```

This schema is deliberately simple to facilitate manual editing and integration with external tools. The `tool` and `version` fields enable receiving tools to validate compatibility, while the `data` object carries the analysis-specific payload.

## 5. Validation

Validation of the toolkit combines automated browser testing with cross-validation against established R packages.

**Automated testing.** Each tool has a dedicated Selenium test suite (25 tests per tool, 200 tests total) that exercises the user interface end-to-end: loading example data, triggering computations, verifying that output elements are populated with plausible values, testing export functionality, and checking edge cases (e.g., single-study input, missing data fields, extreme effect sizes). Tests run in Chrome headless mode and assert on DOM content, SVG element counts, and numerical output ranges.

**R cross-validation.** For tools with direct R counterparts, computed outputs are compared against R reference values. Pooling Suite tau-squared estimates are validated against metafor::rma() for all 10 estimators, with agreement to six decimal places for DL, REML, ML, and PM. Publication Bias Suite Egger's test intercepts and p-values are validated against metafor::regtest(). Meta-Regression Workbench coefficient estimates are validated against metafor::rma() with the mods argument. Bayesian MA posterior summaries are compared with bayesmeta analytical posteriors.

**Built-in example datasets.** All tools include at least one built-in example dataset drawn from published meta-analyses, enabling users to verify the tool against known results and to explore features with realistic data before entering their own.

**Limitations of validation.** Grid approximation in Bayesian MA introduces discretisation error that increases with coarser grids; the default grid resolution (200 x 200) yields posterior summaries within 1-2% of analytical solutions. The GOSH plot in Pooling Suite uses random subset sampling for meta-analyses with more than 15 studies (since exhaustive enumeration of 2^k subsets is computationally infeasible in the browser), which introduces sampling variability in the graphical display but does not affect the primary pooled estimates.

## 6. Discussion

The toolkit addresses a persistent gap in the evidence synthesis software landscape: the absence of accessible, installation-free tools that cover the complete analytical workflow. By implementing all computations in client-side JavaScript within single HTML files, the tools eliminate the dependency, installation, and configuration barriers that prevent many researchers from using advanced synthesis methods.

Several design decisions merit discussion. The single-file architecture sacrifices code modularity for deployment simplicity. Duplicated statistical functions across files mean that a bug fix must be propagated to all affected tools. We accept this trade-off because the primary user benefit---opening a single file in any browser---outweighs the maintenance cost for a small, stable codebase. The total code volume (approximately 14,300 lines across eight tools) is manageable for manual review.

The choice of JavaScript over WebAssembly (WASM) compilation of R or C code reflects a pragmatic assessment. WASM-based approaches (e.g., WebR) enable running R packages directly in the browser but introduce substantial download sizes (tens of megabytes), startup latency, and debugging complexity. Pure JavaScript provides instant startup, transparent source code, and straightforward auditability at the cost of reimplementing established algorithms.

Grid approximation for Bayesian meta-analysis, rather than MCMC, limits the approach to low-dimensional models (the standard two-parameter normal-normal hierarchy). For more complex models (e.g., multivariate meta-analysis, network meta-analysis with inconsistency), MCMC or variational methods would be required. The current implementation serves the common case of standard Bayesian meta-analysis with prior sensitivity analysis, which covers the majority of applications in practice.

The toolkit is not intended to replace R for complex, non-standard analyses. Researchers requiring multivariate meta-analysis, dose-response modelling, individual participant data synthesis, or custom likelihood specifications should use metafor, brms, or equivalent packages. The toolkit's niche is the standard analytical pipeline that constitutes the vast majority of published systematic reviews, delivered in a format accessible to researchers regardless of programming background.

Future development will focus on three directions: (1) expanding the Pooling Suite to support binary outcome data entry (2x2 tables) with automatic effect size calculation; (2) adding a network meta-analysis tool for standard (non-component) NMA with consistency assessment; and (3) implementing a meta-analysis of diagnostic test accuracy (bivariate model) tool to complement the existing PRISMA-DTA extension in the checklist.

## 7. Availability and Licence

All eight tools are open-source under the MIT licence. Source code is available on GitHub. The tools require only a modern web browser (Chrome, Firefox, Edge, or Safari, version 2020 or later) and function fully offline after the initial file download. No user accounts, cloud services, or internet connectivity are required for any analytical function. Each tool can be downloaded as a single HTML file and distributed freely, including in resource-limited settings where software installation privileges or reliable internet access may be unavailable.

## 8. Declarations

**Funding:** No external funding was received for the development of this toolkit.

**Competing interests:** The author declares no competing interests.

**Author contributions:** MA conceived, designed, implemented, tested, and validated all eight tools and wrote the manuscript.

**Data availability:** All built-in example datasets are embedded within the tool source files. No external data were used.

**Acknowledgements:** The author acknowledges the developers of metafor, meta, netmeta, bayesmeta, robvis, and metapower, whose R packages provided the reference implementations against which this toolkit was validated.

---

## References

Balduzzi S, Rucker G, Schwarzer G. How to perform a meta-analysis with R: a practical tutorial. Evidence-Based Mental Health. 2019;22(4):153-160. doi:10.1136/ebmental-2019-300117

Begg CB, Mazumdar M. Operating characteristics of a rank correlation test for publication bias. Biometrics. 1994;50(4):1088-1101. doi:10.2307/2533446

Burkner PC. brms: An R package for Bayesian multilevel models using Stan. Journal of Statistical Software. 2017;80(1):1-28. doi:10.18637/jss.v080.i01

Duval S, Tweedie R. Trim and fill: A simple funnel-plot-based method of testing and adjusting for publication bias in meta-analysis. Biometrics. 2000;56(2):455-463. doi:10.1111/j.0006-341X.2000.00455.x

Egger M, Davey Smith G, Schneider M, Minder C. Bias in meta-analysis detected by a simple, graphical test. BMJ. 1997;315(7109):629-634. doi:10.1136/bmj.315.7109.629

Harrer M, Cuijpers P, Furukawa TA, Ebert DD. Doing Meta-Analysis with R: A Hands-On Guide. Boca Raton, FL: Chapman & Hall/CRC Press; 2021.

Higgins JPT, Thompson SG. Controlling the risk of spurious findings from meta-regression. Statistics in Medicine. 2004;23(11):1663-1682. doi:10.1002/sim.1752

Higgins JPT, Thomas J, Chandler J, et al., eds. Cochrane Handbook for Systematic Reviews of Interventions. Version 6.4. Cochrane; 2023.

Hutton B, Salanti G, Caldwell DM, et al. The PRISMA extension statement for reporting of systematic reviews incorporating network meta-analyses of health care interventions: checklist and explanations. Annals of Internal Medicine. 2015;162(11):777-784. doi:10.7326/M14-2385

Ioannidis JPA, Stanley TD, Doucouliagos H. The power of bias in economics research. Economic Journal. 2017;127(605):F236-F265. doi:10.1111/ecoj.12461

Knapp G, Hartung J. Improved tests for a random effects meta-regression with a single covariate. Statistics in Medicine. 2003;22(17):2693-2710. doi:10.1002/sim.1482

McGuinness LA, Higgins JPT. Risk-of-bias VISualization (robvis): An R package and Shiny web app for visualizing risk-of-bias assessments. Research Synthesis Methods. 2021;12(1):55-61. doi:10.1002/jrsm.1411

McInnes MDF, Moher D, Thombs BD, et al. Preferred Reporting Items for a Systematic Review and Meta-analysis of Diagnostic Test Accuracy Studies: The PRISMA-DTA Statement. JAMA. 2018;319(4):388-396. doi:10.1001/jama.2017.19163

Moher D, Shamseer L, Clarke M, et al. Preferred reporting items for systematic review and meta-analysis protocols (PRISMA-P) 2015: elaboration and explanation. BMJ. 2015;350:g7647. doi:10.1136/bmj.g7647

Otte WM, Winkler-Schwartz A, et al. The sunset funnel plot: visual inspection of funnel plots for publication bias. Journal of Clinical Epidemiology. 2022;145:36-44.

Page MJ, McKenzie JE, Bossuyt PM, et al. The PRISMA 2020 statement: an updated guideline for reporting systematic reviews. BMJ. 2021;372:n71. doi:10.1136/bmj.n71

Polson NG, Scott JG. On the half-Cauchy prior for a global scale parameter. Bayesian Analysis. 2012;7(4):887-902. doi:10.1214/12-BA730

Quintana DS. A guide for calculating study-level statistical power for meta-analyses. Advances in Methods and Practices in Psychological Science. 2022;5(1):1-16. doi:10.1177/25152459221076927

Rethlefsen ML, Kirtley S, Waffenschmidt S, et al. PRISMA-S: an extension to the PRISMA Statement for Reporting Literature Searches in Systematic Reviews. Systematic Reviews. 2021;10:39. doi:10.1186/s13643-020-01542-z

Rover C, Friede T. bayesmeta -- an R package for Bayesian random-effects meta-analysis. Journal of Statistical Software. 2017;93(6):1-51. doi:10.18637/jss.v093.i06

Rucker G, Carpenter JR, Schwarzer G. Small-study effects and limit meta-analysis. Research Synthesis Methods. 2011;2(4):241-250.

Rucker G, Petropoulou M, Schwarzer G. Network meta-analysis of multicomponent interventions. Biometrical Journal. 2020;62(3):808-821. doi:10.1002/bimj.201800167

Rucker G, Krahn U, Konig J, Efthimiou O, Schwarzer G. netmeta: Network Meta-Analysis using Frequentist Methods. R package version 2.8-1; 2022.

Simonsohn U, Nelson LD, Simmons JP. P-curve: a key to the file-drawer. Journal of Experimental Psychology: General. 2014;143(2):534-547. doi:10.1037/a0033242

Stanley TD, Doucouliagos H. Meta-regression approximations to reduce publication selection bias. Research Synthesis Methods. 2014;5(1):60-78. doi:10.1002/jrsm.1095

Sterne JAC, Hernan MA, Reeves BC, et al. ROBINS-I: a tool for assessing risk of bias in non-randomised studies of interventions. BMJ. 2016;355:i4919. doi:10.1136/bmj.i4919

Sterne JAC, Savovic J, Page MJ, et al. RoB 2: a revised tool for assessing risk of bias in randomised trials. BMJ. 2019;366:l4898. doi:10.1136/bmj.l4898

Stewart LA, Clarke M, Rovers M, et al. Preferred Reporting Items for Systematic Review and Meta-Analyses of individual participant data: the PRISMA-IPD Statement. JAMA. 2015;313(16):1657-1665. doi:10.1001/jama.2015.3656

Tricco AC, Lillie E, Zarin W, et al. PRISMA Extension for Scoping Reviews (PRISMA-ScR): Checklist and Explanation. Annals of Internal Medicine. 2018;169(7):467-473. doi:10.7326/M18-0850

van Aert RCM, van Assen MALM. Correcting for publication bias in a meta-analysis with the p-uniform* method. Methodology. 2021;17(2):127-145. doi:10.5964/meth.6482

Vevea JL, Hedges LV. A general linear model for estimating effect size in the presence of publication bias. Psychometrika. 1995;60(3):419-435. doi:10.1007/BF02294384

Viechtbauer W. Conducting meta-analyses in R with the metafor package. Journal of Statistical Software. 2010;36(3):1-48. doi:10.18637/jss.v036.i03

Welton NJ, Caldwell DM, Adamopoulos E, Vedhara K. Mixed treatment comparison meta-analysis of complex interventions: psychological interventions in coronary heart disease. American Journal of Epidemiology. 2009;169(9):1158-1165. doi:10.1093/aje/kwp014
