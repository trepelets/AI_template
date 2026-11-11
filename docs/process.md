# Process — how work on {{PROJECT_NAME}} is organised

Rules for Claude (and anyone else) working on this project. The human-oriented
version in Russian is `docs/how-we-work.ru.md` — keep the two in sync.

## Commands (Russian and English are equivalent)

| English | Russian | Meaning |
|---|---|---|
| "take #N" | «берём #N» | Start backlog item N |
| "remind me about #N" / "brief me on #N" | «напомни о #N» | Read-only briefing on item N (see below) |
| "add to backlog: …" | «добавь в бэклог: …» | New backlog item, next free number |
| "what's on the backlog?" / "current tasks" | «какие задачи сейчас?» | Backlog summary |
| "commit the session" / "wrap up" | «зафиксируй» | End-of-session procedure |
| "review the last session" | «глянь, как прошло» | Master chat reviews the latest handoff (see below) |

## Principles

- **The repo is the memory, chats are workbenches.** Anything that must outlive a
  chat goes into this repo. Never rely on chat history as the record.
- **One chat per task.** A task chat starts from a backlog number ("take #7" /
  «берём #7») and ends with "commit the session" / «зафиксируй». New topic → new chat.
- **One master chat for organisation** ({{PROJECT_NAME}} only): how we work,
  priorities, backlog grooming, tooling. Not for technical work. It can be recreated
  any time from this file — when it gets long, start a new one with
  "read `docs/process.md`".
- **This repo is canonical** (ADR 0001). Code is changed here first, then deployed.

## Session start (task chat)

1. Project instructions make Claude attach `{{OWNER/REPO}}`, clone it
   (`--depth 1`) and read `CLAUDE.md` + `STATUS.md`. If the attach is denied by a
   permission check, stop and ask the user to approve it explicitly ("Attach
   {{OWNER/REPO}} — I approve"); don't work on a task without the repo. A plain
   `git clone` of the private repo won't work without the attach.
2. If the user names a task number, read its entry in `BACKLOG.md`.
3. Read further docs only when needed (`learnings.md` before touching internals,
   the relevant runbook / ADR / reference).
4. Ask for missing facts instead of guessing; mark what was verified on real
   systems vs mocks/docs.

## During the session

- Code changes go into the repo working copy; the user applies them to live
  systems unless asked otherwise.
- New task, bug or idea discovered → add it to `BACKLOG.md` (next free number),
  don't just mention it in chat. Triggered explicitly by "add to backlog: …" /
  «добавь в бэклог: …».
- Large inputs (logs, JSON exports, dumps): extract with `jq`/`grep`, don't paste
  whole files into the conversation.

## Session end — "commit the session" / "wrap up" / «зафиксируй»

Follow the end-of-session steps in `CLAUDE.md` (backlog → STATUS → ADR → learnings →
runbook → secret scan → commit + push). The `session-handoff` skill implements them.
Only what the user decided becomes a decision; Claude's unconfirmed suggestions are
`Status: proposed`.

## Backlog rules

- `BACKLOG.md`: index table + one details section per item.
- Numbers are permanent; never renumber or reuse. Finished items move to "Done"
  with the date.
- Status: `active` / `todo` / `waiting` / `blocked` / `idea`. Priority P1–P3.
  A paused item is `waiting` with "paused: <reason>" in its title.
- `STATUS.md` "Now" and "Top risks" refer to backlog numbers only.
- **Don't repurpose an item.** An item keeps the meaning it was opened with. When
  its goal is met (its "Done when", or the user says it's done) — close it, even if
  related work remains. Remaining work becomes a new item. Rescoping is fine only
  when the user asks for it; record "Rescoped YYYY-MM-DD by <user> (was: …)".
- **Closing an item:** move it to "Done" with the date, one short paragraph of what
  was achieved and verified, and the numbers of follow-up items. The follow-ups say
  "split from #N" / "found in #N".
- **Findings become items immediately.** Anything discovered during a task that
  isn't part of its goal gets its own number at handoff — never an "also look at"
  line inside the current item.

## Briefing — "remind me about #N"

Read-only: don't start the work, don't edit or commit anything. Sources: the item
in `BACKLOG.md` (open or Done), items it links to, `git log --grep '#N'`, and the
docs it points to (runbook, ADR, reference) — read only what's needed. Answer in
the conversation's language, short:

1. **What and why** — the goal in one or two sentences.
2. **Status** — status, priority, what's done so far (with dates), what's left / "Done when".
3. **Blockers and links** — blocked by / blocks / split from / found in.
4. **Where to look** — runbook, ADR, code paths.
5. **Next step** — the first concrete action if the user takes it.

If the item doesn't exist or is ambiguous, say so; never invent details. If the
backlog entry looks stale (e.g. git history shows later work), point that out.

## Master chat review

The master chat (Claude) owns the process. After a task chat has committed its
handoff, the user asks the master chat to review it ("review the last session" /
«глянь, как прошло»). The review checks the handoff commit(s):
- backlog: numbering intact, items closed/split correctly, findings filed as items;
- decisions attributed correctly (user's vs Claude's `proposed`), ADRs updated not rewritten;
- verified vs unverified marked; learnings/runbooks updated where due;
- no secrets in the diff (also commits made in the GitHub web editor, which bypass pre-commit);
- open questions that the task touched (e.g. related items) still accurate.
Fixes are made in the master chat and committed separately.

## Delegation to Haiku subagents

Skills: `delegated-research` (web), `chat-history-recall` (past chats).
A subagent start costs ~70k tokens before any work, so:
- web research: delegate at ~8+ searches, or when the chat will continue long;
- chat history: one targeted search/read is done directly; delegate only broad
  recall across many chats;
- precision-critical facts (versions, prices, compatibility) are verified directly.

## Secrets

Never in the repo: passwords, tokens, API keys, raw dumps with session data,
host lists with credentials. Pre-commit runs gitleaks with `.gitleaks.toml`;
Claude scans diffs before every commit.

## Cost hygiene

- New chat per task; don't continue technical work in the master chat.
- If a task chat gets long: "commit the session", then continue in a fresh chat
  with "take #N".
- Prefer sending the relevant fragment of a log over the whole file.
