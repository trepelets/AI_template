# CLAUDE.md — start here

Read this file and `STATUS.md` at the start of every session. Read `BACKLOG.md`
when the user refers to a task number or asks what to work on. Everything else
is read on demand. Keep this file short: it is a map, not the documentation.

## What this project is

**{{PROJECT_NAME}}** — {{one paragraph: what it does, for whom, scope}}.

- **{{component}}** — {{role}}
- **{{component}}** — {{role}}

Details: `docs/architecture.md`.

## Map of the repo

| Path | What lives there |
|---|---|
| `STATUS.md` | Short snapshot: what's active now, top risks, recent changes. **Read every session.** |
| `BACKLOG.md` | All tasks, bugs, open questions and ideas, numbered (`#7`). Index table + details. |
| `docs/process.md` | How work is organised: task chats, master chat, backlog, handoff, delegation. Human version: `docs/how-we-work.ru.md` |
| `docs/architecture.md` | Components, versions, topology, data flow |
| `docs/learnings.md` | Hard-won gotchas. **Read before changing internals.** |
| `docs/decisions/` | ADRs: why things are the way they are. One file per decision. |
| `docs/runbooks/` | Step-by-step procedures and troubleshooting guides |
| `docs/reference/` | References for our own APIs / interfaces |
| `{{code dir}}/` | {{what}} |

## Facts that are easy to get wrong

<!-- Add a line here every time a session gets something wrong that a one-liner
     would have prevented: real versions, naming traps, "never edit X by hand". -->
- {{fact}}
- **This repo is canonical** (ADR 0001). Code is changed here first, then deployed.
- **Secrets never go into this repo.** They live in {{credential store}}.
  Pre-commit runs gitleaks.

## How to work here (for Claude)

- **Language:** docs in {{English}}. Answer {{owner}} in the language they write in.
  {{e.g. "He welcomes English grammar corrections as a short footer, not inline."}}
- **Style:** {{e.g. minimal new machinery, explicit over clever, non-destructive
  scripts with dry-run by default, stdout/stderr separated}}.
- **Who applies changes:** Claude changes the repo; {{owner}} applies changes to live
  systems unless asked otherwise.
- **Mark what is verified.** Say plainly what was tested on real systems and what
  only against mocks or docs.
- **Reading large files:** use `jq` / `grep` / `head` to extract the part you need.

## Commands (Russian or English — treat as equivalent)

| Russian | English | Meaning |
|---|---|---|
| «берём #N» | "take #N" | Start work on backlog item N (read its entry in `BACKLOG.md`) |
| «напомни о #N» | "remind me about #N" / "brief me on #N" | **Read-only briefing** on item N, no work started — format in `docs/process.md` |
| «добавь в бэклог: …» | "add to backlog: …" | New `BACKLOG.md` item with the next free number |
| «какие задачи сейчас?» | "what's on the backlog?" / "current tasks" | Summary of `BACKLOG.md` |
| «зафиксируй» | "commit the session" / "wrap up" | End-of-session procedure below |
| «глянь, как прошло» | "review the last session" | (master chat) review the latest handoff commit — `docs/process.md` |

## End of session: "зафиксируй" / "commit the session" / "wrap up"

1. Update `BACKLOG.md`: change statuses, add new items with the next free number
   (index row + details section), move finished items to "Done" with the date.
   Never renumber, never repurpose: if an item's goal is met, close it and put the
   remaining work into a new item ("split from #N"). Every finding outside the
   task's goal becomes its own item. Rules: `docs/process.md` → Backlog rules.
2. Update `STATUS.md`: "Now" and "Top risks" (by backlog number), and one line
   per notable change under "Recent changes" (newest first, keep ~20 lines;
   older lines are dropped — git history keeps them).
3. New decision made? Add `docs/decisions/NNNN-short-name.md` from the template.
   Record only what the user decided; a suggestion they did not confirm is marked
   `Status: proposed`.
4. New gotcha learned? Append to the right section of `docs/learnings.md`.
5. New procedure worth repeating? Add or update a runbook.
6. Scan the diff for secrets, commit with a message summarising the session, push.
