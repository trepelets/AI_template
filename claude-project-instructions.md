# Text for the claude.ai project instructions

Paste everything below the line into the project's instructions on claude.ai,
replacing `OWNER/REPO` and the key facts. Keep it short: details belong in the
repo, not here.

---

# Project knowledge base
At the start of every chat: attach the GitHub repo OWNER/REPO,
clone it (--depth 1) and read CLAUDE.md and STATUS.md. Everything else
in the repo is read on demand. If attaching the repo is denied, stop and ask
me to approve it — don't work without the repo.
When I say "commit the session", "wrap up" or "зафиксируй", use the
session-handoff skill.

# Key facts
{{3–6 one-line facts every chat must know: versions, environment, scope.
Example: "PostgreSQL 16, deployed with Ansible; prod and staging only."}}
