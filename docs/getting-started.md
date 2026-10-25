# First local run from source

Use Python 3.10 or newer. The following commands use macOS/Linux shell syntax.
Replace `YOUR-USERNAME` with your GitHub username after forking the repository.

## Get a checkout and activate an environment

```sh
git clone https://github.com/YOUR-USERNAME/python-slugify.git
cd python-slugify
python3 --version
python3 -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell, use `py -3` in place of `python3`, then activate with
`.venv\Scripts\Activate.ps1`. Once activated, the `python` commands below are the same.

## Install the project and development tools

Run these commands from the repository root:

```sh
python -m pip install -r dev.requirements.txt
python -m pip install -e '.[unidecode,anyascii]'
```

The editable install uses this checkout, so source changes are available without
reinstalling. The extras install the optional backends used in development;
they do not remove the base `text-unidecode` dependency.

## Try the command-line interface

```sh
python -m slugify "Hello, open source!"
# hello-open-source
python -m slugify "Café au lait"
# cafe-au-lait
python -m slugify --separator _ "Hello, open source!"
# hello_open_source
```

The default algorithm is `legacy`. Add `--algorithm modern` to explicitly use
the modern algorithm. Read the [migration guide](release-9/migration.md) before
changing slugs already stored by an application.

## Run a small test group, then the full suite

```sh
python -m pytest -q tests/test_modern_word_boundary.py
python -m pytest
```

The small group checks modern truncation, separators, and word ordering. A
successful run reports two passing tests; pytest versions may also report
subtests separately. The full suite includes legacy, modern, CLI, and error cases.

For a package build and type checking:

```sh
python -m build
python -m mypy
```

These commands build local artifacts and check types. They do not publish a package.
See the README for the full interpreter/backend matrix and artifact checks.

## Find your way around

- `slugify/`: public API, modern and legacy implementations, and CLI entry point.
- `tests/`: automated behavior and regression tests.
- `tools/`: compatibility and distribution verification utilities.
- `pyproject.toml`: dependencies, package metadata, and tool configuration.

## Common setup problems

- If Python is too old, install a supported interpreter and recreate the virtual environment.
- If `slugify` cannot be imported, activate the environment and repeat the editable install.
- If an explicitly selected optional backend is missing, install its corresponding extra.
- If the full tox matrix reports unavailable interpreters, install those interpreters or
  run only the environment you actually have. One local run does not verify the whole matrix.

Read [CONTRIBUTING.md](../CONTRIBUTING.md) before proposing upstream changes:
the project requires an issue and maintainer agreement before a pull request.
