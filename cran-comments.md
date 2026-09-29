# Submission of ThSQCA 2.0.8

This is a corrective release submitted only two days after 2.0.7 (published
2026-09-24). I apologize for the very short interval between submissions.

A user of the package reported that `compute_fiss_core()` can return an
incorrect core/peripheral classification without any error or warning. Since
this function is used to prepare configuration tables for publication, and
the incorrect behavior fails silently, I would prefer not to leave it on CRAN
until the next regular release, hence this immediate follow-up.

## Changes

* `compute_fiss_core()` classifies the conditions of an intermediate QCA
  solution as core or peripheral (Fiss, 2011). It now does so configuration by
  configuration, as in Fiss's solution tables: the core conditions of a term
  are those of the parsimonious term(s) contained in it. 2.0.7 counted a
  condition as core whenever it appeared anywhere in the parsimonious
  solution, which does not match how Fiss's tables are built. The new rule
  is consistent with both of Fiss's solution tables; a regression test
  encodes them.
* When the parsimonious solution has tied minimal solutions, the function now
  uses the derivation that `QCA::minimize()` records (`sol$i.sol`, `$p.sol`).
  It classifies only the reported solution and compares it with its own
  source parsimonious solution(s). 2.0.7 mixed terms of different
  intermediate models and compared with every tied solution.
* Condition names containing a dot were matched with the dot as a regular
  expression wildcard; they are now matched literally.
* Results of `ctSweepM()` and `dtSweep()`, for which the function silently
  returned nothing, now give an informative error. Chart labels for
  `ctSweepS()` results show the swept condition instead of `thrY`.
* The help page, the vignettes and the README describe the rule and its
  relation to Fiss (2011).

## Test environments

* local: Windows 11 x64, R 4.6.1, QCA 3.25.5
* Ubuntu 24.04, R 4.3.3, QCA 3.25

## R CMD check results

`devtools::check(remote = TRUE)` (that is, `R CMD check --as-cran` with the
CRAN incoming checks enabled) on the built tarball:

0 errors | 0 warnings | 1 note

* "Days since last update: 2, Number of updates in past 6 months: 7" (checking
  CRAN incoming feasibility). This is the short interval explained above.

## Reverse dependencies

There are no reverse dependencies on CRAN.
