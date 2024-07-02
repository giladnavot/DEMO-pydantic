---
title: Usage of Makefile in the Project
---
This document provides a detailed explanation of how the `Makefile` is used in the DEMO-pydantic repository. It covers the various commands and their purposes within the project's build process.

<SwmSnippet path="/Makefile" line="4">

---

# Checking for PDM Installation

The `Makefile` begins by checking if PDM (Python's package manager) is installed. If PDM is not installed, it prompts the user to install it.

```
.PHONY: .pdm  ## Check that PDM is installed
.pdm:
	@pdm -V || echo 'Please install PDM: https://pdm.fming.dev/latest/#installation'

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="8">

---

# Checking for pre-commit Installation

Next, the `Makefile` checks if `pre-commit` is installed. If not, it prompts the user to install it. `pre-commit` is a tool used to manage and maintain pre-commit hooks.

```
.PHONY: .pre-commit  ## Check that pre-commit is installed
.pre-commit:
	@pre-commit -V || echo 'Please install pre-commit: https://pre-commit.com/'

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="12">

---

# Installing the Package, Dependencies, and pre-commit for Local Development

The `install` command is used to install the package, its dependencies, and pre-commit for local development. It uses PDM to install all the dependencies and sets up pre-commit hooks.

```
.PHONY: install  ## Install the package, dependencies, and pre-commit for local development
install: .pdm .pre-commit
	pdm info
	pdm install --group :all
	pre-commit install --install-hooks

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="18">

---

# Syncing Lockfiles with Requirements Files

The `refresh-lockfiles` command is used to sync lockfiles with requirements files. It updates the lockfiles for all groups.

```
.PHONY: refresh-lockfiles  ## Sync lockfiles with requirements files.
refresh-lockfiles: .pdm
	pdm update --update-reuse --group :all

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="22">

---

# Rebuilding Lockfiles from Scratch

The `rebuild-lockfiles` command is used to rebuild lockfiles from scratch, updating all dependencies in the process.

```
.PHONY: rebuild-lockfiles  ## Rebuild lockfiles from scratch, updating all dependencies
rebuild-lockfiles: .pdm
	pdm update --update-eager --group :all

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="26">

---

# Auto-formatting Python Source Files

The `format` command is used to auto-format Python source files. It uses the `ruff` tool to check and format the source files.

```
.PHONY: format  ## Auto-format python source files
format: .pdm
	pdm run ruff check --fix $(sources)
	pdm run ruff format $(sources)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="31">

---

# Linting Python Source Files

The `lint` command is used to lint Python source files. It uses the `ruff` tool to check and format the source files.

```
.PHONY: lint  ## Lint python source files
lint: .pdm
	pdm run ruff check $(sources)
	pdm run ruff format --check $(sources)

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="36">

---

# Using Codespell for Spellchecking

The `codespell` command is used to perform spellchecking on the codebase. It uses the `pre-commit` tool to run `codespell` on all files.

```
.PHONY: codespell  ## Use Codespell to do spellchecking
codespell: .pre-commit
	pre-commit run codespell --all-files

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="40">

---

# Performing Type-checking

The `typecheck` command is used to perform type-checking on the codebase. It uses the `pre-commit` tool to run type-checking on all files.

```
.PHONY: typecheck  ## Perform type-checking
typecheck: .pre-commit .pdm
	pre-commit run typecheck --all-files

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="65">

---

# Running All Tests

The `test` command is used to run all tests, skipping the type-checker integration tests. It uses `pytest` to run the tests and `coverage` to measure code coverage.

```
.PHONY: test  ## Run all tests, skipping the type-checker integration tests
test: .pdm
	pdm run coverage run -m pytest --durations=10

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="69">

---

# Running Benchmarks

The `benchmark` command is used to run all benchmarks. It uses `pytest` to run the benchmarks and `coverage` to measure code coverage.

```
.PHONY: benchmark  ## Run all benchmarks
benchmark: .pdm
	pdm run coverage run -m pytest --durations=10 --benchmark-enable tests/benchmarks

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="73">

---

# Running Tests and Generating a Coverage Report

The `testcov` command is used to run tests and generate a coverage report, skipping the type-checker integration tests. It uses `coverage` to measure code coverage and generate the report.

```
.PHONY: testcov  ## Run tests and generate a coverage report, skipping the type-checker integration tests
testcov: test
	@echo "building coverage html"
	@pdm run coverage html
	@echo "building coverage lcov"
	@pdm run coverage lcov

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="80">

---

# Running Only the Tests from the Documentation

The `test-examples` command is used to run only the tests from the documentation. It finds all Python files in the `docs/examples` directory and runs them.

```
.PHONY: test-examples  ## Run only the tests from the documentation
test-examples: .pdm
	@echo "running examples"
	@find docs/examples -type f -name '*.py' | xargs -I'{}' sh -c 'pdm run python {} >/dev/null 2>&1 || (echo "{} failed")'

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="100">

---

# Running the Standard Set of Checks Performed in CI

The `all` command is used to run the standard set of checks performed in CI. It includes linting, type-checking, spellchecking, and generating a coverage report.

```
.PHONY: all  ## Run the standard set of checks performed in CI
all: lint typecheck codespell testcov

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="103">

---

# Clearing Local Caches and Build Artifacts

The `clean` command is used to clear local caches and build artifacts. It removes various temporary and build files from the project.

```
.PHONY: clean  ## Clear local caches and build artifacts
clean:
	rm -rf `find . -name __pycache__`
	rm -f `find . -type f -name '*.py[co]'`
	rm -f `find . -type f -name '*~'`
	rm -f `find . -type f -name '.*~'`
	rm -rf .cache
	rm -rf .pytest_cache
	rm -rf .ruff_cache
	rm -rf htmlcov
	rm -rf *.egg-info
	rm -f .coverage
	rm -f .coverage.*
	rm -rf build
	rm -rf dist
	rm -rf site
	rm -rf docs/_build
	rm -rf docs/.changelog.md docs/.version.md docs/.tmp_schema_mappings.html
	rm -rf fastapi/test.db
	rm -rf coverage.xml

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="124">

---

# Generating the Docs

The `docs` command is used to generate the project documentation. It uses `mkdocs` to build the documentation.

```
.PHONY: docs  ## Generate the docs
docs:
	pdm run mkdocs build --strict

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
