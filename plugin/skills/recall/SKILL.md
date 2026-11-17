---
name: recall
description: Recall check - the owner explains a concept from memory without opening the note, you check it against the note and the source, record what they missed, and only a passed check promotes the note to solid. Pass a note ("context-window"); default the most overdue developing note. Runs only when you type its slash command.
disable-model-invocation: true
---

# Recall: earn "solid"

People overrate how well they know something, especially right after studying it, and stop too
early. So "solid" comes from an actual retrieval attempt checked against the note, never from
"I think I know it". Retrieval also strengthens the memory, so a failed check is still useful.
Their language. Short.

## 0. Where to work
The vault is the current folder if it holds `vault.json`; else ask which folder. `cd` there, read
`vault.json` and `CLAUDE.md`, and follow them, the work-vault rules above all. `git pull --ff-only`
if there's a remote (failure: one line, go on).

## 1. Which note
What they passed, else the most overdue note that is:
- not marked `(known)` on its map, or marked and never recalled (a known item's first check);
- `developing`, last changed (`git log -1 --format=%cs -- <file>`, else `created`) 7+ days ago; or
- `solid`, last `reviewed` date 30+ days ago.
Lowest tier first among equally overdue notes. Nothing due: say so, name the next due note and
its date, and stop (don't run a check early; spacing is the point). A note they name that was
changed under 7 days ago: run the check, but it can't promote to `solid`; say so first.

## 2. The attempt
Say the concept's name only. Ask them to explain it from memory, without opening the vault or the
source: what it is, why it works that way, and one thing it connects to. Don't show any part of
the note until they have answered.

## 3. Check it
Compare their answer with the note's "In my words", mechanism and links, and with the source for
correctness. Reply with three short lists: **got it**, **missed**, **wrong** (with the correction
and why). Then show the note's own wording for what they missed.

For a `system` concept, also check against the code at the paths in "Where in the code"
(work vaults: read-only), since the code may have changed since the note was written.

Pass = the core idea and the mechanism are there and nothing important is wrong. A missing link
alone doesn't fail it. You decide pass or fail from the lists, not from how confident they sound;
they can't pass themselves.

## 4. Record
- Pass: add today to `reviewed`; `developing` → `solid` (only if 7+ days since the last change).
- Fail: add each miss or error to "Mistakes I made / open questions" (`- <date>: recall: <what>`);
  a `solid` note goes back to `developing`. Offer to let them rewrite "In my words" now if their
  understanding changed (their words, as always).
- Append `- recall: [[<note>]] pass|fail` to `log/<today>.md`.
- `git add -A`, `git commit -m "recall: <note> pass|fail"`, `git push` if `vault.json` allows (failure: one line).

## 5. Next
If another note is due, offer it. Otherwise stop.
