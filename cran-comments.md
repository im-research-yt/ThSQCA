# Submission of ThSQCA 2.0.7

This is a minor release. It comes about two months after 2.0.6 (published on
2026-07-23). It does not change any computed result: it improves messages,
printed labels and report text, corrects the documentation of how membership
scores can be swept, and reorganizes the tutorial vignette.

## Changes

Output and messages:

* The calls to `QCA::truthTable()` and `QCA::minimize()` inside the sweep loops
  now go through an internal helper, `quiet_try()`, so that sweeps no longer
  print one stray blank line for every cell without a solution. Warnings are
  passed on unchanged.
* A `pre_calibrated` variable that contains memberships of exactly 0.5 now
  produces one warning per sweep that names the variable, instead of QCA's
  generic warning repeated for every cell.
* Printed and summarised results use the labels OTS, DTS, CTS (single) and
  CTS (multiple). The S3 class names are unchanged.
* `generate_report()` now shows the outcome name supplied by the user (it
  previously showed the internal column name `Y`), and the verification code
  at the end of a report reproduces the solution type actually used. A
  leading space in some condition names in the necessity table was removed.
* The warning for a variable that is both in `pre_calibrated` and in a sweep
  list now says how to sweep it; the warning for multiple minimal solutions no
  longer says "intermediate" for complex and parsimonious runs.

Documentation:

* The documentation no longer recommends sweeping variables only on their raw
  scale: membership scores can be swept like any other numeric variable.
  The rule itself is unchanged.
* The tutorial vignette was reorganized and extended (data preparation,
  choosing a solution type, multiple minimal solutions, reporting, FAQ).
* Terminology was aligned with the companion methodology paper
  (Threshold-Sweep QCA; CTS, OTS, DTS). No function names or arguments
  changed.
* `citation("ThSQCA")` now also lists the accompanying preprint
  (<doi:10.31235/osf.io/yb8xs_v1>).

New regression tests cover the output changes (all tests pass locally).

## Test environments

* local: Windows 11 x64, R 4.6.1, QCA 3.25.5

## R CMD check results

`devtools::check(remote = TRUE)` (that is, `R CMD check --as-cran` with the
CRAN incoming checks enabled) on the built tarball:

0 errors | 0 warnings | 0 notes

## Reverse dependencies

There are no reverse dependencies on CRAN.
