---
name: "delegated-research"
description: "Delegate heavy web research (about 8+ searches, candidate hunting, broad overviews) to a Haiku subagent and bring back only a compact summary."
---

# Delegated research via Haiku

Goal: raw search results stay in the subagent's context, and only a short summary reaches the main chat. The chat history stays small, so later messages don't keep re-reading thousands of tokens of search noise.

## First decide: direct or delegated

Launching a subagent has a fixed cost of roughly 70k tokens in its own context before it does any work (measured: a 2-call test cost ~72k). A search with its results is typically a few thousand tokens. So delegation only pays off when the raw material is large, or when the chat will go on long enough that keeping results out of the history matters.

Rough guide:
- **1-7 searches: search yourself.** Delegating costs more than it saves.
- **About 8+ searches or fetches: delegate.** Measured rounds of 21-28 calls cost ~90-100k tokens, mostly the fixed start-up, so the saving comes from keeping all that out of the main chat.
- **Borderline (5-8 searches):** delegate only if the chat is already long or will clearly continue, since every later message re-reads whatever lands in the history.

## When to use

- The answer needs about 8 or more searches or page fetches.
- Hunting for candidates: "find a game/film/book from this description", "list tools for X", "what alternatives to Y exist".
- Broad overviews and first-pass exploration of a topic, where breadth matters more than the precision of every number.
- Grunt work across similar sources: go through archives, lists or forums and note the matches.

## When NOT to use

- A handful of searches is enough. Searching directly is faster and cheaper.
- Precision matters: prices, software versions, hardware compatibility, investing and finance, legal or medical questions. Search yourself here, or at least verify the key facts from the summary with 1-2 searches of your own.
- The task needs iterative judgment, where each finding changes the search strategy and the full conversation context is required.

## Procedure

1. **Tell the user in one line** before launching: what is being searched and the rough size ("running a Haiku search, up to ~25 queries"). Delegation is token-heavy, so the user should know before it starts.
2. **Write the task using the template below.** Include everything already known from the conversation: details, options already ruled out, sources already checked.
3. **Launch the subagent:** Agent tool, `model: haiku`, `subagent_type: general-purpose`. Use `run_in_background: false` when the next step depends on the result.
4. **Read the summary critically.** Haiku can miss things or overrate a match. If a decision depends on a high-confidence candidate, check it yourself with 1-2 searches.
5. **Report to the user briefly**, in the conversation's language: what was found, how confident, what was checked without results, and the next step. Don't relay the whole report.
6. **For a follow-up round, launch a new subagent** with a refined task instead of continuing the old one via SendMessage. The old context is already bloated with results, and every continuation re-reads it. Continue the old one only if it genuinely needs memory of earlier findings.

## Subagent task template

```
Task: <one sentence on what to find>.
Use WebSearch and WebFetch. Search language(s): <languages that fit the topic>.

WHAT IS KNOWN
- <all details from the conversation, as bullet points>

ALREADY RULED OUT (do not report these)
- <options already checked that didn't fit>

WHERE TO LOOK (hints)
- <source types, specific sites, forums, archives>
- Useful keywords: <alternative phrasings>

RULES
- At most ~<N, usually 20-25> searches and fetches.
- Do not invent anything: report only what you actually saw in a source.
- If nothing fits, say so plainly.
- Do not download anything except page text.

OUTPUT FORMAT (in <the conversation's language>, at most 350 words)
1. Up to 6 candidates, best first: name, key facts, what matches and what doesn't, confidence (high/medium/low), one source URL.
2. One line: which sources were checked and gave nothing.
3. One line: what to try next.
No raw search dumps, no long quotes.
```

## Notes

- Because the start-up cost is fixed, one well-scoped round of 20-25 calls is far better value than several small rounds. Give the subagent enough budget to finish in one go.
- The subagent also uses up limits. The main saving is not the model choice but keeping its tokens out of the main chat history.
- The most useful parts of the task: the ruled-out list, the query limit, and a strict output format with a word cap. Without them the summary comes back longer and worse.
- The line "checked and gave nothing" shows where to look next and avoids repeating work.
- If two rounds in a row find nothing, the information is probably not in the open web index. Suggest another route to the user (ask on a specialist forum, recall more details) instead of a third identical search.