## Submission notes

This is ThSQCA 2.0.9, a bug-fix release submitted 8 days after 2.0.8 (accepted on 2026-09-26). I am sorry for the short interval and for the number of recent updates.

While auditing the report and chart functions after 2.0.8, I found several defects that can silently produce wrong or empty output. The methodology paper behind the package has just been published, so more people are likely to use the package now, and I would rather not leave these defects in the CRAN version. The solutions and fit measures returned by the sweep functions (`otSweep()`, `ctSweepS()`, `ctSweepM()`, `dtSweep()`) are unchanged; the defects are in the reports, charts and helper functions:

* The "Necessity Analysis" table of `generate_report()` was computed for `Y` even when the outcome was negated (`~Y`), while the solution was computed for `~Y`.
* `generate_report()` produced no per-combination sections for results of `ctSweepM()` and `dtSweep()`.
* Configuration charts, `compute_fiss_core()` and `format_qca_term()` mishandled variable names that are the beginning of other names, or that contain non-ASCII characters or regular-expression characters.
* The sweep functions reported "No solution" without any message when `QCA::minimize()` raised an error (for example for condition names with non-ASCII characters). They now warn about such names, and about settings that failed because of a QCA error. Errors that only mean that there is nothing to minimize are not reported.

Details are in NEWS.md. No new dependencies were added, and no exported function or argument was removed or renamed.

## Test environments

* Local: Windows 11 x64, R 4.6.1 (`R CMD check --as-cran` on the built tarball)
* win-builder: R-devel (2026-09-30 r90605)

## R CMD check results

0 errors | 0 warnings | 1 note

* checking CRAN incoming feasibility ... NOTE
  Number of updates in past 6 months: 8

  This is informational. The recent releases (2.0.x) were corrective releases that fixed defects found after the earlier versions were published. I do not plan further frequent updates and will batch future changes into less frequent releases.
