---
name: cookiecutter-data-science
description: Scaffold a new data science / ML project instantly with Cookiecutter Data Science (CCDS v2, drivendataorg). Use when the user says "nouveau projet data", "new data science project", "ccds", "cookiecutter data science", "scaffold ML project", "structure projet data", or wants to restructure an existing messy notebook project into the standard CCDS layout.
---

# Cookiecutter Data Science (CCDS v2)

Repo: https://github.com/drivendataorg/cookiecutter-data-science — Docs: https://cookiecutter-data-science.drivendata.org/

Generates a standardized, reproducible data science project skeleton (data/raw→processed, notebooks, src module, Makefile, tests, docs).

## Workflow

1. **Collect only what's needed** (use defaults below for anything the user didn't specify — don't interrogate):
   - `project_name` (required), `description`, output directory (default: current working dir).
2. **Generate non-interactively** — zero install needed thanks to `uvx`:

```bash
uvx --from cookiecutter-data-science ccds https://github.com/drivendataorg/cookiecutter-data-science --no-input -o "<OUTPUT_DIR>" \
  project_name="<name>" \
  author_name="<from `git config user.name`>" \
  description="<desc>" \
  python_version_number=3.11 \
  environment_manager=uv \
  dependency_file=pyproject.toml \
  pydata_packages=basic \
  testing_framework=pytest \
  linting_and_formatting=ruff \
  open_source_license=MIT \
  docs=mkdocs \
  include_code_scaffold=Yes
```

   ⚠️ The template URL as first positional arg is **mandatory** when passing `--no-input`/extra context (otherwise ccds treats `project_name=...` as the template path and fails).
   Folder name = `project_name` lowercased, spaces → `_`.

3. **Post-setup** (inside the new folder):
```bash
git init && git add -A && git commit -m "Initial CCDS scaffold"
make create_environment   # or: uv venv && uv pip install -e .
make requirements
```
   On Windows without `make`, run the equivalent commands from the Makefile directly (read it first).

4. Report the tree and next steps (put raw data in `data/raw/`, notebooks named `1.0-<initials>-description.ipynb`, reusable code in `<module>/`).

## Options (first value = ccds default; my recommended defaults are in the command above)

| Key | Choices |
|---|---|
| `environment_manager` | virtualenv, conda, pipenv, uv, pixi, poetry, none |
| `dependency_file` | requirements.txt, pyproject.toml, environment.yml, Pipfile, pixi.toml |
| `pydata_packages` | none, basic (numpy, pandas, matplotlib, sklearn, jupyter…) |
| `testing_framework` | none, pytest, unittest |
| `linting_and_formatting` | ruff, flake8+black+isort |
| `open_source_license` | "No license file", MIT, BSD-3-Clause |
| `docs` | mkdocs, none |
| `include_code_scaffold` | Yes, No (dataset.py, features.py, modeling/train.py, predict.py, plots.py, config.py) |
| `dataset_storage` | none / s3 / azure / gcs — cloud sync via `make sync_data_up/down`. Needs nested dict → use interactive `ccds` if the user wants cloud storage. |

Pick a coherent combo: `uv`+`pyproject.toml`, `conda`+`environment.yml`, `pipenv`+`Pipfile`, `pixi`+`pixi.toml`, `poetry`+`pyproject.toml`.

## Resulting layout

```
├── Makefile            <- make data / lint / format / test / clean / requirements
├── README.md
├── data/{external,interim,processed,raw}   <- raw is IMMUTABLE, never edit
├── docs/               <- mkdocs
├── models/             <- trained models, predictions
├── notebooks/          <- naming: <num>-<initials>-<desc>.ipynb
├── pyproject.toml
├── references/         <- data dictionaries, manuals
├── reports/figures/
├── tests/
└── <module_name>/      <- config.py, dataset.py, features.py, plots.py, modeling/{train,predict}.py
```

## CCDS principles to apply when coding in such a project

- Raw data is immutable; every transform is code (`dataset.py` → `data/interim|processed`).
- Notebooks = exploration & communication; move reusable logic into `<module_name>/` and import it (`%load_ext autoreload` + `%autoreload 2`).
- Analysis is a DAG: pipeline must be rerunnable from `data/raw` via `make data` / scripts.
- Secrets in `.env` (gitignored), loaded with `python-dotenv` — never commit credentials.
- `data/` and `models/` are gitignored by default; share large data via cloud storage, not git.

## Restructuring an existing project

If the user wants to migrate an existing repo: generate the scaffold in a sibling folder, then move files into place (raw data → `data/raw`, notebooks renamed, helper functions → module). Confirm the move plan before touching their files.

## Variants

- Interactive prompts: `uvx --from cookiecutter-data-science ccds`
- Latest unreleased template: add `-c master`
- Old v1 template: `uvx cookiecutter -c v1 https://github.com/drivendataorg/cookiecutter-data-science`
- Permanent install: `uv tool install cookiecutter-data-science` → then just `ccds`
