---
title: 'ThSQCA: An R Package for Reproducible Threshold-Sweep Workflows in Qualitative Comparative Analysis'
tags:
  - R
  - qualitative comparative analysis
  - configurational comparative methods
  - social science methodology
  - reproducibility
authors:
  - name: Yuki Toyoda
    orcid: 0009-0007-4477-1383
    affiliation: 1
affiliations:
  - name: Graduate School of Innovation Management, Hosei University, Japan
    index: 1
date: 23 September 2026
bibliography: paper.bib
---

# Summary

Qualitative Comparative Analysis (QCA) is a research method for identifying combinations of conditions that are jointly sufficient for an outcome, widely used across the social sciences [@ragin1987; @ragin2008; @schneider2012]. In crisp-set QCA, continuously measured variables are converted into binary set-membership scores using a substantively chosen threshold, and a standard analysis fixes these thresholds once, reporting a single solution. `ThSQCA` is an R package that instead treats the threshold itself as an object of analysis: it repeats the same QCA workflow across a specified range of outcome thresholds, condition thresholds, or both, and organizes the resulting solutions, fit measures, and configuration changes into a common, comparable, and reportable format. `ThSQCA` builds on the `QCA` package [@thiem2013; @dusa2019] for truth table construction and Boolean minimization, and is complementary to the robustness-diagnostic functions in `SetMethods` [@oana2018; @oana2024]. The package is available on CRAN and archived with a permanent DOI [@toyoda2026a; @toyoda2026zenodo]; its methodological motivation, threshold-sweep QCA, is developed in a companion article [@toyoda2026b].

# Statement of need

In applied crisp-set QCA, a single threshold decision, such as the score above which a case counts as showing "high" usage intention or "high" service quality, can substantially change which configurations are found sufficient [@krogslund2015; @skaaning2011; @verkuilen2005]. Two practical needs follow from this. First, researchers often want to check how far a single-threshold conclusion extends: does the solution persist as the outcome threshold moves, or is it an artifact of one cut point? Second, in many applications the threshold is not merely a robustness nuisance but a meaningful target level in its own right, since the configuration required to make a customer merely willing to use a brand differs from the configuration required to make that customer a strongly committed user, and both questions carry independent, practical interest. The primary users are applied QCA researchers who need to assess calibration-threshold dependence systematically while retaining the established `QCA` workflow.

Carrying out this kind of threshold sweep by hand requires custom code that repeatedly re-dichotomizes the data, re-runs the truth table and minimization steps at each cut point, tracks which configurations appear or disappear, and reconciles differing solution counts and no-solution cells across iterations. Because this comparison work is typically implemented ad hoc, its correctness and completeness vary by study, and independent reproduction is difficult; a review of 106 published QCA articles found that only 28 could be fully reproduced [@rohlfing2021]. `ThSQCA` addresses this gap with a single, tested, and documented workflow for exploring how a QCA solution structure changes across a threshold range.

# State of the field

The `QCA` package [@thiem2013; @dusa2019] provides the standard computational engine for truth table construction and Boolean minimization in R, and is the dependency that `ThSQCA` builds on rather than replaces. `SetMethods` [@oana2018] provides a comprehensive robustness-test protocol that evaluates sensitivity to calibration choices, consistency cutoffs, and frequency thresholds [@oana2024]. Its calibration procedures are organized primarily around an initial solution, identifying the range over which that solution remains unchanged while other analytic settings are held fixed. `ThSQCA` addresses a complementary analytical objective: rather than using threshold variation principally to establish the robustness range of an initial solution, it retains and compares the complete sequence of threshold-specific solutions across a user-specified threshold space, including changes in solution composition, multiple equivalent solutions, and no-solution cells, and it supports simultaneous sweeps over multiple condition thresholds and joint condition-outcome threshold grids.

Neither `QCA` nor `SetMethods` provides an integrated workflow for mapping, normalizing, visualizing, and reporting the complete set of threshold-specific solutions across these multi-dimensional threshold sweeps, and there was no methodological reason to duplicate their truth table and minimization logic in a new package. `ThSQCA` was therefore implemented as a separate workflow layer with four sweep functions: outcome-threshold sweeps (`otSweep()`), single- and multi-condition-threshold sweeps (`ctSweepS()`, `ctSweepM()`), and joint outcome-and-condition sweeps (`dtSweep()`), together with functions for classifying core and peripheral conditions across a sweep following @fiss2011, and for generating a reproducible Markdown report of an entire sweep (`generate_report()`).

# Software design

`ThSQCA` deliberately does not implement its own Boolean minimization. Instead, it delegates truth table construction and minimization to `QCA::truthTable()` and `QCA::minimize()`, and adds a workflow layer that controls threshold iteration, result normalization, comparison, visualization, and reporting. This division of labor keeps the minimization engine independently maintained by `QCA`, so that improvements or corrections to `QCA` propagate automatically to `ThSQCA`, while threshold-sweep-specific data structures, visualizations, and reporting can evolve without changing the semantics of the underlying analysis. `ThSQCA`'s function arguments follow `QCA` conventions (`incl.cut`, `n.cut`, `pri.cut`, `include`, `dir.exp`), so that a researcher who already knows `QCA` does not need to learn a second parameter system.

A central design problem is that different thresholds in a sweep can yield different numbers of minimal solutions, or no solution at all, and existing single-threshold tools have no obligation to normalize this. `ThSQCA` resolves it by aggregating the resulting product terms, solution consistency, solution coverage, and solution counts from every threshold into a single structured object with a consistent schema, and by explicitly detecting and reporting multiple equivalent solutions through an `extract_mode` argument ("first", "all", or "essential"), which lets a researcher distinguish essential prime implicants common to every equivalent solution from selective prime implicants specific to only some of them.

`ThSQCA`'s unit tests validate its sweep results against direct, independently written loops over `QCA::truthTable()` and `QCA::minimize()`, confirming that the package's aggregation logic reproduces the underlying `QCA` computations rather than introducing new analytical behavior. This design, delegating the underlying computation to an existing engine and adding a dedicated layer of sensitivity analysis on top of it, follows the same pattern as recent sensitivity-analysis packages such as `konfound` [@narvaiz2024], which evaluates the robustness of already-fitted regression models without re-implementing the fitting procedure itself.

# Research impact statement

`ThSQCA` and its predecessor, `TSQCA`, have been publicly distributed through CRAN since January 2026 [@toyoda2026tsqcazenodo], with the package undergoing multiple version updates during that period; the current release, v2.0.6, is also tagged in the GitHub repository. The package was renamed from `TSQCA` to `ThSQCA` during peer review of the companion article, after a reviewer noted that `TSQCA` could be confused with time-series QCA; the CRAN and Zenodo archival records under the earlier name document the package's release history prior to the rename. The package includes a vignette and a bundled example dataset (`sample_data`) that provide reproducible examples of the principal threshold-sweep workflows.

`ThSQCA` has been used to implement and reproduce the threshold-sweep analyses reported in a companion methodological article [@toyoda2026b], accepted for publication in *Quality & Quantity*. In that application to consumer survey data on brand usage intention (*n* = 336, four conditions), the package was used to evaluate how sufficient configurations changed across outcome and condition thresholds. It reproduced the complete sequence of threshold-specific QCA solutions and summarized changes in solution structure, fit, and coverage without requiring separate analysis scripts for each threshold setting (\autoref{fig:threshold}).

The package is designed to integrate directly with established QCA workflows by delegating truth-table construction and minimization to the `QCA` package while providing reproducible threshold iteration, solution tracking, visualization, and reporting. Together with its CRAN distribution, archived releases, documentation, tests, and reproducible examples, this provides a foundation for its use in applied crisp-set QCA research beyond the original methodological study.

![Example of a cross-threshold configuration summary produced from a `ThSQCA` outcome-threshold sweep, following the notation of @fiss2011. \label{fig:threshold}](figure1.png)

# AI usage disclosure

The author used Anthropic's Claude and OpenAI's ChatGPT for assistance with preliminary English-language editing, manuscript organization, restructuring this paper to JOSS's required section format, and the preparation and review of selected R code, including the continuous integration workflow and package documentation added to the repository. No generative AI was used to design the package's statistical methodology, to author its core minimization logic (which is delegated entirely to `QCA`), or to generate the results reported in the companion article [@toyoda2026b]. All AI-assisted content, including this disclosure, was reviewed and verified by the author, who takes full responsibility for the package and this article.

# Acknowledgements

This work was supported by JSPS KAKENHI (Grant Number JP20K01998).

# References
