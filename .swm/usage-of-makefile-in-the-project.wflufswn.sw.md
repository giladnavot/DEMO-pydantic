---
title: Usage of Makefile in the Project
---
This document provides a detailed explanation of how the <SwmPath>[Makefile](/Makefile)</SwmPath> is used in the DEMO-pydantic repository. It will cover the purpose and functionality of each command in the <SwmPath>[Makefile](/Makefile)</SwmPath>.

<SwmSnippet path="/Makefile" line="4">

---

# Checking for Required Tools

The <SwmPath>[Makefile](/Makefile)</SwmPath> begins by checking if the required tools, PDM and <SwmToken path="/Makefile" pos="8:5:7" line-data=".PHONY: .pre-commit  ## Check that pre-commit is installed">`pre-commit`</SwmToken>, are installed. If not, it provides the user with the installation instructions.

```
.PHONY: .pdm  ## Check that PDM is installed
.pdm:
	@pdm -V || echo 'Please install PDM: https://pdm.fming.dev/latest/#installation'

.PHONY: .pre-commit  ## Check that pre-commit is installed
.pre-commit:
	@pre-commit -V || echo 'Please install pre-commit: https://pre-commit.com/'
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="12">

---

# Installing Dependencies

The <SwmToken path="/Makefile" pos="12:4:4" line-data=".PHONY: install  ## Install the package, dependencies, and pre-commit for local development">`install`</SwmToken> command is used to install the package, its dependencies, and <SwmToken path="/Makefile" pos="12:20:22" line-data=".PHONY: install  ## Install the package, dependencies, and pre-commit for local development">`pre-commit`</SwmToken> for local development. It uses PDM to manage the Python dependencies and <SwmToken path="/Makefile" pos="12:20:22" line-data=".PHONY: install  ## Install the package, dependencies, and pre-commit for local development">`pre-commit`</SwmToken> to manage the Git hooks.

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

# Updating Lockfiles

The <SwmToken path="/Makefile" pos="18:4:6" line-data=".PHONY: refresh-lockfiles  ## Sync lockfiles with requirements files.">`refresh-lockfiles`</SwmToken> and <SwmToken path="/Makefile" pos="22:4:6" line-data=".PHONY: rebuild-lockfiles  ## Rebuild lockfiles from scratch, updating all dependencies">`rebuild-lockfiles`</SwmToken> commands are used to update the lockfiles. The former syncs the lockfiles with the requirements files, while the latter rebuilds the lockfiles from scratch, updating all dependencies.

```
.PHONY: refresh-lockfiles  ## Sync lockfiles with requirements files.
refresh-lockfiles: .pdm
	pdm update --update-reuse --group :all

.PHONY: rebuild-lockfiles  ## Rebuild lockfiles from scratch, updating all dependencies
rebuild-lockfiles: .pdm
	pdm update --update-eager --group :all
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="26">

---

# Formatting and Linting

The <SwmToken path="/Makefile" pos="26:4:4" line-data=".PHONY: format  ## Auto-format python source files">`format`</SwmToken> and <SwmToken path="/Makefile" pos="31:4:4" line-data=".PHONY: lint  ## Lint python source files">`lint`</SwmToken> commands are used to <SwmToken path="/Makefile" pos="26:8:10" line-data=".PHONY: format  ## Auto-format python source files">`Auto-format`</SwmToken> and lint the Python source files respectively. They use the <SwmToken path="/Makefile" pos="28:5:5" line-data="	pdm run ruff check --fix $(sources)">`ruff`</SwmToken> tool to perform these tasks.

```
.PHONY: format  ## Auto-format python source files
format: .pdm
	pdm run ruff check --fix $(sources)
	pdm run ruff format $(sources)

.PHONY: lint  ## Lint python source files
lint: .pdm
	pdm run ruff check $(sources)
	pdm run ruff format --check $(sources)
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="36">

---

# Spellchecking

The <SwmToken path="/Makefile" pos="36:4:4" line-data=".PHONY: codespell  ## Use Codespell to do spellchecking">`codespell`</SwmToken> command is used to perform spellchecking using the Codespell tool.

```
.PHONY: codespell  ## Use Codespell to do spellchecking
codespell: .pre-commit
	pre-commit run codespell --all-files
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="40">

---

# Type Checking

The <SwmToken path="/Makefile" pos="40:4:4" line-data=".PHONY: typecheck  ## Perform type-checking">`typecheck`</SwmToken> command is used to perform <SwmToken path="/Makefile" pos="40:10:12" line-data=".PHONY: typecheck  ## Perform type-checking">`type-checking`</SwmToken>. It uses the <SwmToken path="/Makefile" pos="40:4:4" line-data=".PHONY: typecheck  ## Perform type-checking">`typecheck`</SwmToken> hook from the <SwmToken path="/Makefile" pos="41:4:6" line-data="typecheck: .pre-commit .pdm">`pre-commit`</SwmToken> tool.

```
.PHONY: typecheck  ## Perform type-checking
typecheck: .pre-commit .pdm
	pre-commit run typecheck --all-files
```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="44">

---

# Running Tests

Several commands are used to run different types of tests. For example, <SwmToken path="/Makefile" pos="44:4:6" line-data=".PHONY: test-mypy  ## Run the mypy integration tests">`test-mypy`</SwmToken> runs the mypy integration tests, <SwmToken path="/Makefile" pos="61:4:6" line-data=".PHONY: test-pyright  ## Run the pyright integration tests">`test-pyright`</SwmToken> runs the pyright integration tests, and <SwmToken path="/Makefile" pos="44:4:4" line-data=".PHONY: test-mypy  ## Run the mypy integration tests">`test`</SwmToken> runs all tests, skipping the <SwmToken path="/Makefile" pos="65:19:21" line-data=".PHONY: test  ## Run all tests, skipping the type-checker integration tests">`type-checker`</SwmToken> integration tests.

```
.PHONY: test-mypy  ## Run the mypy integration tests
test-mypy: .pdm
	pdm run coverage run -m pytest tests/mypy --test-mypy

.PHONY: test-mypy-update  ## Update the mypy integration tests for the current mypy version
test-mypy-update: .pdm
	pdm run coverage run -m pytest tests/mypy --test-mypy --update-mypy

.PHONY: test-mypy-update-all  ## Update the mypy integration tests for all mypy versions
test-mypy-update-all: .pdm
	rm -rf tests/mypy/outputs
	pip install --force mypy==1.0.1 && make test-mypy-update
	pip install --force mypy==1.1.1 && make test-mypy-update
	pip install --force mypy==1.2.0 && make test-mypy-update
	pip install --force mypy==1.4.1 && make test-mypy-update
	pip install --force mypy==1.5.0 && make test-mypy-update

.PHONY: test-pyright  ## Run the pyright integration tests
test-pyright: .pdm
	pdm run bash -c 'cd tests/pyright && pyright'

```

---

</SwmSnippet>

<SwmSnippet path="/Makefile" line="103">

---

# Cleaning Build Artifacts

The <SwmToken path="/Makefile" pos="103:4:4" line-data=".PHONY: clean  ## Clear local caches and build artifacts">`clean`</SwmToken> command is used to clear local caches and build artifacts. It removes various temporary files and directories that are created during the build process.

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

# Generating Documentation

The <SwmToken path="/Makefile" pos="124:4:4" line-data=".PHONY: docs  ## Generate the docs">`docs`</SwmToken> command is used to generate the documentation. It uses the <SwmToken path="/Makefile" pos="126:5:5" line-data="	pdm run mkdocs build --strict">`mkdocs`</SwmToken> tool to build the documentation.

```
.PHONY: docs  ## Generate the docs
docs:
	pdm run mkdocs build --strict
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
