# 0001. This repo is the single source of truth

- **Status:** accepted
- **Date:** {{YYYY-MM-DD}}
- **Decided by:** {{user}}

## Context

Work with Claude happens in many chats; chats of one project can't see other
projects' chats, and long chat histories are expensive and lose detail. Code and
notes were scattered ({{former locations}}).

## Decision

`{{OWNER/REPO}}` holds the code, docs, backlog and decisions. Changes are made here
first; chats read it at the start and write back at the end ("commit the session").

## Consequences

- Former locations stop receiving changes (frozen / read-only / mirrors).
- Live systems are updated from this repo.
- Nothing important lives only in a chat.
