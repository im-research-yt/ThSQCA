# Contributing to ThSQCA

Thank you for your interest in contributing to `ThSQCA`.

## Reporting bugs and requesting features

Please use the [GitHub issue tracker](https://github.com/im-research-yt/ThSQCA/issues)
to report bugs or request features. When reporting a bug, please include:

- A minimal reproducible example (a small dataset and the code that triggers the issue)
- The output of `sessionInfo()`
- What you expected to happen, and what happened instead

## Contributing code

1. Fork the repository and create a new branch for your change.
2. Make your changes, following the existing code style.
3. Add or update tests in `tests/testthat/` covering your change.
4. Run `devtools::check()` locally and make sure the package passes
   `R CMD check` with no errors or warnings.
5. Update documentation (roxygen2 comments, and the vignette if relevant)
   as needed.
6. Open a pull request describing the motivation for the change and
   summarizing what it does.

## Code of conduct

Please be respectful and constructive in all interactions related to this
project, including issues, pull requests, and discussions.

## Questions

For questions about using the package that are not bug reports, please
open a [discussion](https://github.com/im-research-yt/ThSQCA/discussions)
or issue.
