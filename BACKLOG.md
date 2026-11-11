# BACKLOG

One list for everything: work in progress, bugs, open questions, ideas.
Refer to items by number ("take #7" / «берём #7»); new items via
"add to backlog: …" / «добавь в бэклог: …». Numbers are permanent — never reused
or renumbered; finished items move to "Done" at the bottom.

**Status:** `active` being worked on · `todo` ready to start · `waiting` needs
input/info from someone (or paused) · `blocked` can't proceed until something else
happens · `idea` direction known, not scheduled.
**Priority:** P1 now / risky if left · P2 next · P3 later.

## Index

| # | Title | Status | Prio | Area |
|---|---|---|---|---|
| 1 | Fill in the skeleton: CLAUDE.md, architecture, key facts | todo | P1 | repo |
| 2 | Import existing code into the repo (secrets out first) | todo | P1 | repo |
| 3 | Collect known tasks, bugs and decisions into BACKLOG / ADRs | todo | P1 | repo |

---

## Details

### #1 Fill in the skeleton — todo, P1
Replace every `{{…}}` placeholder: `CLAUDE.md` (what the project is, repo map,
facts that are easy to get wrong, style), `docs/architecture.md`, `STATUS.md`,
`docs/process.md`, `docs/how-we-work.ru.md`. Add project-specific rules to
`.gitleaks.toml`.
**Done when:** `grep -rn '{{' .` finds nothing outside the ADR template.

### #2 Import existing code — todo, P1
Collect code and configs from wherever they live now. Remove secrets **before**
importing; leave out venvs, caches, build outputs, raw dumps. Old locations get
frozen (ADR 0001); anything missing is listed as a backlog item.

### #3 Collect known tasks, bugs and decisions — todo, P1
From project memory, project docs and past chats of this project: open issues →
backlog items, decisions → ADRs (who decided; Claude's unconfirmed suggestions →
`proposed`), gotchas → `docs/learnings.md`.

---

## Done

_(moved here with the date when finished)_
