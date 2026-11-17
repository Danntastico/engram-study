---
name: connect
description: Link a concept note to related concepts - suggests candidates with the kind of relation (prerequisite, part of, contrast, example, cause), the owner writes the reason for each link they accept, and a note with its first reasoned link moves to developing. Pass the note ("attention"); default the note captured last. Runs only when you type its slash command.
disable-model-invocation: true
---

# Connect: links with reasons

A link is only worth something when the owner can say why two ideas connect. Building the connections
yourself is what teaches (making a concept map beats studying a given one), so you suggest and
they explain. Their language. Short.

## 0. Where to work
The vault is the current folder if it holds `vault.json`; else ask which folder. `cd` there, read
`vault.json` and `CLAUDE.md`, and follow them, the work-vault rules above all. `git pull --ff-only`
if there's a remote (failure: one line, go on).
The note: what they passed, else the concept in the last `capture:` line of the newest `log/` file.

## 1. Candidates
Read the note and the domain map. Pick 3–5 candidates, preferring concepts that already have
notes, then concepts from lower tiers (prerequisites). For each, show only the name and the
**kind** of relation: prerequisite of, builds on, part of, contrasts with, example of, causes.
Not the reason: that's theirs to write. Skip concepts already in "Connects to".

## 2. His reasons
Ask them to pick the ones they see and write, for each, one sentence: "because …". If their reason
is wrong, say what's off in one line and let them rewrite it; if it's vague ("they're related"),
ask "related how: what does one need from the other?" A candidate they can't explain: leave it out
and add it to "Mistakes I made / open questions" as an open question.

## 3. Write
- Each accepted link: `- [[<other>]] — because <their sentence>` under "Connects to" in this note
  only. Obsidian shows backlinks, so don't add the reverse link unless they want to write a reason
  from the other side too.
- A candidate with no note yet: create it from `templates/concept.md` (`concept-code.md` for a `system` map) as a `seed` (frontmatter
  and guiding comments only).
- Status: `seed` → `developing` when "In my words" is theirs and there's at least one reasoned link.

## 4. Log and commit
Append `- connect: [[<note>]] → <n> links (<status>)` to `log/<today>.md`, then `git add -A`,
`git commit -m "connect: <note>"`, `git push` if `vault.json` allows (failure: one line).
