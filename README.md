# {{PROJECT_NAME}}

{{One-line description.}}

- **Как мы работаем (памятка для владельца):** [`docs/how-we-work.ru.md`](docs/how-we-work.ru.md)
- **Start here (for Claude):** [`CLAUDE.md`](CLAUDE.md) — map of the repo and working rules
- **Current work:** [`STATUS.md`](STATUS.md)
- **All tasks:** [`BACKLOG.md`](BACKLOG.md)
- **How it fits together:** [`docs/architecture.md`](docs/architecture.md)
- **Gotchas:** [`docs/learnings.md`](docs/learnings.md)
- **Why things are the way they are:** [`docs/decisions/`](docs/decisions/)

## Secrets

Nothing secret goes into this repo — no passwords, tokens or API keys.
They live in {{credential store: Vault / env vars / app credentials}}.

Install the pre-commit hook once after cloning:

```bash
pipx install pre-commit      # or: pip install --user pre-commit
pre-commit install
pre-commit run --all-files   # first full scan
```

The hook runs [gitleaks](https://github.com/gitleaks/gitleaks) with the rules in
`.gitleaks.toml` on every commit. Edits made in the GitHub web editor bypass it.
