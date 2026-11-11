---
name: "session-handoff"
description: "Record a work session into the project's git repo (backlog, STATUS.md, decisions, learnings, runbooks) and commit, when the user says 'commit the session', 'wrap up', 'зафиксируй' or similar."
---

# Session handoff

Purpose: whatever was learned or decided in this chat must survive the chat. The project repo is the long-term memory; the chat is temporary. After a handoff, the next session should need only `CLAUDE.md` + `STATUS.md` (and the backlog entry it works on) to continue, with no chat history search.

## Trigger

Russian and English phrases are equivalent:

- English: "commit the session", "wrap up", "wrap up the session", "record this", "save progress to the repo"
- Russian: "зафиксируй", "зафиксируй сессию"
- or the user ends a working session and asks to write things down.

## Preconditions

1. The project repo is attached and cloned in this session. If not, attach it (the repo named in the project instructions or `CLAUDE.md`), clone it with `--depth 1`, and continue.
2. Pull first: `git fetch origin <branch> && git rebase origin/<branch>` (the user or another session may have pushed).
3. If the repo's `CLAUDE.md` has its own end-of-session procedure, it takes precedence over the defaults below.

## Procedure

1. **Review the session.** List, for yourself: what was finished, what changed direction, what was decided (and by whom), what broke and why, what is still open, what files the user received.
2. **Separate decisions from suggestions.** Only what the user decided or confirmed is a decision. Your own proposals that the user didn't confirm are recorded as `Status: proposed` or as open items, never as decisions.
3. **Update the backlog** if the repo has one (`BACKLOG.md`): change statuses of the items worked on, add new tasks/bugs/ideas with the next free number (index row + details section), move finished items to "Done" with the date. Never renumber or reuse numbers.
4. **Update `STATUS.md`:**
   - refresh the current-state sections (e.g. "Now", "Top risks"), referring to backlog numbers where a backlog exists;
   - add one line per notable change under "Recent changes" (newest first, `YYYY-MM-DD — what`); keep about 20 lines, drop the oldest (git keeps them);
   - update the "Last updated" line.
5. **New decision?** Add `docs/decisions/NNNN-short-name.md` from `0000-template.md` (next free number). If it replaces an old one, set the old one to `superseded by NNNN`.
6. **New gotcha?** Add one bullet to the right section of `docs/learnings.md`: the rule first, then why. Mark it "(unverified)" if it wasn't confirmed on real hardware/systems.
7. **New repeatable procedure?** Add or update a file in `docs/runbooks/`.
8. **Files produced in the session** (scripts, workflow JSON, patches): if the user wants them in the repo, put them in the right folder. Never commit secrets, credentials, passwords, tokens, or raw dumps with session data; scan diffs for them before committing.
9. **Show the user a short summary** of what will be written (3-8 bullets) when anything is ambiguous, especially decisions. If the session was routine, proceed without asking.
10. **Commit and push:**
    - one commit per session, message: short summary line + bullets of what changed;
    - append the attribution trailers the environment requires;
    - `git push`. If push is rejected, fetch, rebase, push again once.
11. **Report in one or two lines** what was recorded, in the conversation's language.

## Rules

- Keep `CLAUDE.md` short and stable: change it only when the map of the repo or the working rules change.
- Write docs in the repo's language (usually English); answer the user in theirs.
- Prefer editing existing text over appending duplicates. If a fact in `STATUS.md`, `BACKLOG.md` or `learnings.md` was wrong, correct it in place.
- Don't record transient chat details (exact commands tried in passing, intermediate guesses) unless they are part of a gotcha or runbook.
- Facts about the live infrastructure that are already queryable (inventories, APIs, dashboards) stay there; record only decisions, rules and pointers.