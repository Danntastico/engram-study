---
type: guide
---
# Quick guide: how to use this vault

You learn something → guess → study → write it in your own words → link it → recall it from
memory → review it as flashcards. Claude Code handles structure, files and git. **You write the
understanding** (see `CLAUDE.md` for why).

## The loop

```
/map (once)  →  /capture  →  /connect  →  /cards  →  Anki (daily)  →  /recall (7+ days later)
 study path      seed         developing     flashcards   FSRS reviews     solid
```

| Command | Use it when | You do | You get |
|---|---|---|---|
| `/map <paths or topic>` | Starting a new subject or codebase | Read the draft path, reorder it, write your goal | `maps/<domain>.md` + source notes |
| `/capture [source]` | Studying the next concept (no argument = next one on the map) | Guess first, study, then explain in 2–5 sentences + why it works | Concept note, mistakes logged |
| `/connect [note]` | Right after a capture | Write the *because* for each link you accept | First reasoned link → `developing` |
| `/cards [note]` | A `developing` note has no cards (if Anki is on) | Write 2–3 questions; approve the cards | 2–5 Anki cards |
| `/recall [note]` | A note changed 7+ days ago | Explain it from memory, note closed | Pass → `solid`; fail → mistakes logged |

## A study session (45–60 min)
1. **`/recall`** (5–10 min): clear what's due.
2. **`/capture`** with no argument: it picks the next concept on the map and asks for your guesses
   (2–3 min). Wrong guesses are fine; they make the reading stick.
3. **Study** (20–30 min): read the source or the code it points to, with your guesses in mind. For
   a flow, follow one real request from start to end.
4. **Back in `/capture`** (10 min): explain it in your words and why it works; fix what the check finds.
5. **`/connect`** (5 min), then **`/cards`** if Anki is on.

3–4 sessions a week beats one long one: the gaps between them are part of how memory works.

## Note status
| Status | Means | How it changes |
|---|---|---|
| `seed` | Note exists | Write "In my words" + one link with your *because* |
| `developing` | You've explained and linked it | Pass `/recall` 7+ days after the last edit |
| `solid` | You've retrieved it from memory | Failing a later `/recall` sends it back |

## Rules of thumb
- One concept at a time, in map order.
- The agent never writes "In my words", the mechanism or a *because* for you. Ask for hints.
- Mistakes are useful: they're recorded and become `mistake` cards.
- Reference material (`sources/`, `projects/`, `log/`, "Where in the code") is for finding, not memorising.
