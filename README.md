# org-analyser

Org/repo codebase analysis pipeline: merged-PR counts, PR task-profile, codebase profiler, eval-kit, and sealed repo quality score — one command, one or many repos.

## Setup

Needs Python 3.10+ — [python.org/downloads](https://www.python.org/downloads/) (or `brew install python3` on macOS, `pyenv install 3.12` via [pyenv](https://github.com/pyenv/pyenv)).

```
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip   # need pip>=21.3 for editable installs (PEP 660)
pip install -e .
cp config.example.yml config.yml
```


Edit `config.yml` → fill in `tokens:` (only whichever platform(s) you use). `config.yml` is gitignored, never committed.

- `github-data-token` — [github.com/settings/tokens](https://github.com/settings/tokens) (classic, `repo` scope)
- `gitlab_token` — [gitlab.com/-/user_settings/personal_access_tokens](https://gitlab.com/-/user_settings/personal_access_tokens) (`read_api` scope)
- `openai_key` — [platform.openai.com/api-keys](https://platform.openai.com/api-keys)

## Run

```
org-analyser --github-repo owner/repo --workers 1        # single repo
org-analyser --github-org your-org --workers 10          # whole org
org-analyser --gitlab-group your-group --workers 10       # whole GitLab group
org-analyser --local-repos-dir ./my-repos --workers 4      # local checkouts
```

Output lands in `outputs/org-analyser-runs/<run-name>/` (manifest, per-repo reports, sealed quality score, zip).

`org-analyser --help` for all flags.

## Debug

- **`SSL: CERTIFICATE_VERIFY_FAILED`** — fixed via `certifi`; rerun after `pip install -e .` picks up the dependency.
- **Auth / 404 / "Could not resolve to a Repository"** — check the token in `config.yml` has access to that org/repo, and the `owner/repo` name is correct.
- **Config not picked up** — confirm you're running from the repo root (`config.yml` must sit next to `cli.py`), or set `ORG_ANALYSER_CONFIG=/path/to/config.yml`.
- **No target error** — pass one of `--github-org` / `--github-repo` / `--gitlab-group` / `--gitlab-project` / `--local-repos-dir`, or set one under `config.yml`.
- Logs print to stdout during the run; check the run's `manifest.json` for a per-repo pass/fail summary.
