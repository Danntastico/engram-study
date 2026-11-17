---
name: cards
description: Turn a developing concept note into a few Anki flashcards (vaults with Anki on) - the owner writes the questions first, answers come from their own words in the note, you check card quality and coverage, then add the approved cards to Anki through AnkiConnect and record their IDs in the note's cards field. Pass a note ("embeddings"); default the developing note with no cards, lowest tier first. Runs only when you type its slash command.
disable-model-invocation: true
---

# Cards: from a concept note to flashcards in Anki

Anki (FSRS-6) schedules the reviews; this skill makes the cards. Two findings shape it:
cards people make themselves are remembered better than premade ones (Pan et al., 2023: d ≈ 0.45
for definitions, ≈ 0.29 for application), and people grading their own answers are too lenient
when an answer has several parts (Dunlosky & Rawson, 2012). So **the owner writes the questions**,
answers come from **their own words**, and every answer is **one idea** they can grade yes/no.
Their language. Short. About 5 minutes per note.

## 0. Where to work
The vault is the current folder if it holds `vault.json`; else ask which folder. `cd` there, read
`vault.json` and `CLAUDE.md`, and follow them, the work-vault rules above all. `git pull --ff-only`
if there's a remote (failure: one line, go on).

If `vault.json` → `anki.enabled` is false: say cards are off for this vault, that `/recall` still
works, and stop. Deck root = `anki.deckRoot`; vault name for links = `name`.

## 1. Anki must be open
AnkiConnect listens on `http://127.0.0.1:8765` while Anki desktop runs. To avoid shell-quoting
problems, write each request body to a temp JSON file and send it with
`curl -s -X POST http://127.0.0.1:8765 -d @<file>`. Every request is
`{"action": "...", "version": 6, "params": {...}}`; check `error` in every reply.

Send `version`. No answer: tell them in one line to open Anki (AnkiConnect add-on code
`2055492159`) and stop.

## 2. One-time setup (skip what already exists)
- `modelNames`. If `SB-Concept` is missing, `createModel`:
  - `modelName`: `SB-Concept`
  - `inOrderFields`: `["Front", "Back", "NoteId", "Kind", "SourceLink"]`
  - `cardTemplates`: one template, `Name` `Card 1`,
    `Front` `{{Front}}`,
    `Back` `{{FrontSide}}<hr id=answer>{{Back}}<div class=src>{{Kind}} · <a href="{{SourceLink}}">open note</a></div>`
  - `css`: `.card{font-family:system-ui,sans-serif;font-size:20px;text-align:left;max-width:640px;margin:auto}.src{margin-top:1.5em;font-size:13px;opacity:.6}`
- `deckNames`. If `<deckRoot>::<Domain>` is missing (Domain = the note's first `domain`, capitalised:
  e.g. `SecondBrain::AI`), `createDeck`.
- First time only, tell them once: in Anki, deck options of `<deckRoot>` → FSRS on, desired
  retention 0.90; press Optimize after about a month of reviews.

## 3. Which note
What they passed, else the `developing` note with an empty `cards: []`, lowest tier first.
- `seed` note: no cards. Say why in one line (cards come after they have written it in their own words)
  and offer `/capture` or `/connect`.
- Note that already has cards: `notesInfo` on its `cards` IDs, show them, and ask whether to add
  more or fix one (`updateNoteFields` for a fix; never delete without asking).

## 4. Their questions first
Show the concept name only (not the note). Ask: "What 2–3 questions should future-you be able to
answer about <concept>? At least one *why/how* or *when would you use it*." They write the fronts.
Stuck: offer one hint of the **kind** of question (e.g. "what breaks without it?"), not the
question itself.

## 5. Answers from their words
For each front, write the back **only from the note's "In my words", "Why it works" and
"Mistakes I made"**. You may shorten their wording; never add a fact the note doesn't contain. If the
note can't answer a question they asked, that's a gap: say so and offer to let them add it to the note
in their words first (then continue), or drop the card.

Tag each card with one `Kind`:
`definition` · `mechanism` (why/how) · `contrast` (X vs Y) · `application` (use it on a case) ·
`mistake` (from "Mistakes I made": "Is it true that <old belief>? Why not?").

## 6. Check quality, then they approve
Check every card and fix with them, one line per problem:
- **One idea per back**: one sentence, roughly 20 words at most. Two ideas → two cards.
- **No lists to memorise** ("the 4 types of…") → one card per item, or a contrast card.
- **No yes/no fronts** and no front that gives the answer away.
- **Coverage**: at least one `mechanism` or `application` card. Missing → ask them for one more
  question of that kind. Any line under "Mistakes I made" → propose a `mistake` card front;
  they rewrite or reject it.
- **2–5 cards per note.** More means the note holds more than one concept: suggest splitting it.

Show the final set as a short table (Kind · Front · Back). Only add what they approve, as edited.

## 7. Add to Anki
`addNotes` with one note per card:
```json
{"deckName": "<deckRoot>::<Domain>", "modelName": "SB-Concept",
 "fields": {"Front": "...", "Back": "...", "NoteId": "<note id>", "Kind": "mechanism",
            "SourceLink": "obsidian://open?vault=<name>&file=concepts%2F<slug>"},
 "tags": ["sb::<domain>::tier<tier>", "kind::<kind>"],
 "options": {"allowDuplicate": false, "duplicateScope": "deck"}}
```
The reply is a list of Anki note IDs (`null` = rejected, usually a duplicate: say which one).
Append the new IDs to the note's `cards: []` (integers). Change nothing else in the note.

If `anki.sync` is true, send `sync` so the cards reach the phone through AnkiWeb (failure: one
line; they can press Sync in Anki). If false, never sync: the cards stay in this computer's Anki.

## 8. Log and commit
- Append `- cards: [[<note>]] +<n> (<kinds>)` to `log/<today>.md` (create with `type: log`).
- `git add -A`, `git commit -m "cards: <note> +<n>"`, `git push` if `vault.json` allows (failure: one line).
- If another `developing` note has no cards, offer it. Otherwise stop.

## Don't
- Write fronts for them before they have tried, or write backs from the source instead of their note.
- Make cards from `seed` notes or from `sources/` (reference material isn't for memorising).
- Put scheduling data (due dates, ease, stability) in the vault. Anki owns scheduling; the note
  only stores card IDs.
