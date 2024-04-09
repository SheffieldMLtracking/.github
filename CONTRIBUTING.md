# Contributing guide

We welcome new users and contributors to this project. This document contains the [guidelines](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors) for contributing to these code repositories.

# How to contribute

## Getting help

Please [create an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue) or contact [Mike Smith](https://www.sheffield.ac.uk/dcs/people/academic/michael-smith).

# Version control

Please create branches and request for them to be merged using [pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests).

## Proposing changes

Feel free to [fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) these repositories to propose new changes.

# Releases

See: [Releasing projects on GitHub](https://docs.github.com/en/repositories/releasing-projects-on-github)

## Publishing a new release

1. Incrememnt the version number in the code repository by editing the value of the `__version__` variable in `__init__.py`
2. [Create a new release](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
3. Click "Choose a tag" and enter the version number
4. TODO

The Python package will be [automatically published to PyPI](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/) using a [GitHub Actions](https://docs.github.com/en/actions) workflow.

## Version numbers

The [PEP 440](https://peps.python.org/pep-0440/) standard means that each release has a [version number](https://semver.org/) such as `v1.4.2` where the three parts are `MAJOR.MINOR.PATCH` where

- `MAJOR` version when you make _incompatible_ API changes
- `MINOR` version when you add functionality in a _backward compatible_ manner
- `PATCH` version when you make backward compatible bug fixes

When working on changes, you can make a [development release](https://peps.python.org/pep-0440/#developmental-releases) such as `v0.3.dev2`.

# Packaging

Each code repository contains a [Python package](https://packaging.python.org/en/latest/overview/) that is [specified using a pyproject.toml file](https://packaging.python.org/en/latest/guides/). These packages are published to the [Python Package Index](https://pypi.org/) (PyPI) so they can be easily installed by anyone.

# Coding style conventions

[Epytext](https://epydoc.sourceforge.net/manual-epytext.html)
```python
def my_function(aardvark_count: float):
  """
  This is my code.

  @param aardvark_count: The number of Aardvarks.
  """
```
