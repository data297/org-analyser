# org-analyser

Org/repo codebase analysis pipeline: merged-PR counts, PR task-profile, codebase profiler, eval-kit, and sealed repo quality score — one command, one or many repos.

## Setup

```
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
cp config.example.yml config.yml
```


Edit `config.yml` → fill in `tokens:` (`github-data-token`, `gitlab_token`, `openai_key` — only whichever platform(s) you use). `config.yml` is gitignored, never committed.

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
