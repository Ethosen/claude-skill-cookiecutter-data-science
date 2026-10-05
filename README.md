# Claude Code Skill — Cookiecutter Data Science

A [Claude Code](https://claude.com/claude-code) skill that scaffolds a new data science / ML project in seconds using [Cookiecutter Data Science v2](https://github.com/drivendataorg/cookiecutter-data-science) (CCDS) by DrivenData.

Just say *"new data science project called churn-model"* and Claude generates the full standardized layout (`data/raw → processed`, `notebooks/`, source module, `Makefile`, tests, docs), initializes git and sets up the environment. No prompts, nothing to install beforehand.

## What it does

- Generates the project non-interactively with `uvx` (no permanent install of `ccds` needed)
- Sensible modern defaults: `uv` + `pyproject.toml`, `ruff`, `pytest`, basic pydata packages, MIT, mkdocs (all overridable)
- Runs post-setup: `git init`, environment creation, dependency install (Windows-friendly when `make` is missing)
- Applies CCDS principles when coding in the project (immutable raw data, logic out of notebooks, reproducible pipeline, secrets in `.env`)
- Can migrate an existing messy project into the CCDS layout (asks before moving files)

## Requirements

- [Claude Code](https://claude.com/claude-code)
- [uv](https://docs.astral.sh/uv/) (provides `uvx`)
- Python 3.10+

## Install

```bash
git clone https://github.com/Ethosen/claude-skill-cookiecutter-data-science.git
cp -r claude-skill-cookiecutter-data-science/cookiecutter-data-science ~/.claude/skills/
```

On Windows (PowerShell):

```powershell
git clone https://github.com/Ethosen/claude-skill-cookiecutter-data-science.git
Copy-Item -Recurse claude-skill-cookiecutter-data-science\cookiecutter-data-science $HOME\.claude\skills\
```

Restart Claude Code (or start a new session) and the skill is available.

## Usage

- `/cookiecutter-data-science`
- or naturally: *"scaffold a new ML project named fraud-detection with conda"*, *"restructure this notebook folder into CCDS"*

## Generated layout

```
├── Makefile            <- make data / lint / format / test / clean
├── data/{external,interim,processed,raw}
├── docs/
├── models/
├── notebooks/
├── pyproject.toml
├── references/
├── reports/figures/
├── tests/
└── <module_name>/      <- config.py, dataset.py, features.py, plots.py, modeling/
```

## Credits

All the template work belongs to [DrivenData](https://github.com/drivendataorg/cookiecutter-data-science) (MIT). This repo only packages it as a Claude Code skill.

## License

MIT
