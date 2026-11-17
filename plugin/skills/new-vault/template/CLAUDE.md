# Learning vault: rules for the agent

A Markdown vault (opened in Obsidian, versioned with git) where the owner learns and keeps
reference material. The `engram-study` Claude Code plugin works on it. **Read `vault.json` first**:
it names the owner, says whether this is a `personal` or `work` vault, and what may leave this
machine. Every rule below comes from learning research (generation effect, self-explanation,
concept mapping, prequestions, retrieval practice).

**The one rule:** you (the agent) capture, structure, link, check and commit. **The owner writes
the understanding.** If you write it for them, they get an archive and no memory: unrestricted AI
help measurably harms learning.

## Two layers

| Layer | Folders | Who writes it |
|---|---|---|
| Knowledge | `concepts/`, `maps/` | The owner writes "In my words", "Why it works", each link's *because* and the map's goal. You create the structure, questions and frontmatter. |
| Reference | `sources/`, `projects/`, `log/` | You, freely: summaries, key points, file paths, current numbers, links. |

## Hard rules for concept notes
1. **Never write "In my words", "Why it works / mechanism" or a link's *because* yourself.** Not as
   a draft, not as an example, not "to get started". Write exactly what the owner says (fix typos
   only if asked).
2. **Guess, then study.** Before the owner reads about a concept, ask for a quick guess at its
   2–3 guiding questions. Wrong guesses are useful: they're what the reading corrects.
3. **Attempt before explanation.** Ask the owner to explain first. Stuck: a hint (a question, or
   where to look), then a second, narrower hint. Only after two hints, a short explanation; then
   the owner restates it, and the restatement is what gets recorded.
4. **Feedback after the attempt, about the error.** Say what's missing or wrong and why, briefly.
   Record real mistakes (and wrong guesses that taught something) under "Mistakes I made / open
   questions".
5. **Links carry reasons.** Suggest candidates; the owner accepts and writes the *because*.
6. **Status is earned, never self-rated.**
   - `seed`: the note exists.
   - `developing`: "In my words" written by the owner + at least one link with their *because*.
   - `solid`: passed `/recall` (explained from memory, checked against the note) at least 7 days
     after the note last changed. Failing `/recall` on a solid note sets it back to `developing`.
7. **One concept per note, one concept at a time.**

## Schema (the contract other tools rely on)
Concept notes: `concepts/<slug>.md` (lowercase, hyphens; the filename is the link name).

```yaml
id: YYYYMMDD-<slug>    # creation date + slug; never changes
type: concept          # concept | source | project | log | map
domain: [<domain>]     # first item = the map it belongs to (maps/<domain>.md)
tier: 1                # position in the map (prerequisites first)
status: seed           # seed | developing | solid
source: "[[<source-slug>]]"   # main source, or ""
created: YYYY-MM-DD
reviewed: []           # dates of /recall checks, oldest first
cards: []              # Anki note IDs, filled by /cards (Anki owns scheduling)
```

Sections, in order: `## In my words`, `## Why it works / mechanism`, (code concepts only)
`## Where in the code`, `## Connects to`, `## Mistakes I made / open questions`. Guiding
questions go in HTML comments (`<!-- ... -->`), hidden in Obsidian's reading view. "Where in the
code" is reference: you fill it with paths and one-line pointers. Templates are in `templates/`.

Source notes (`sources/<slug>.md`, `type: source`): title, url or path, kind (video, paper,
article, course, docs, talk, conversation, repo, spec), key points in your words, `concepts:`
links. Fast-changing facts (versions, scores, prices) live here with their date.

## Maps
`maps/<domain>.md` (`type: map`) lists a domain's concepts in prerequisite order, with the
owner's goal at the top. `kind: topic` for a subject (e.g. AI); `kind: system` for a codebase or
product, built by `/map` from repos and specs. Study in tier order; mix tiers only once lower tiers
are `developing`. The owner may reorder or rename anything in a map: their order wins.

## Daily log
`log/YYYY-MM-DD.md` (`type: log`): one line per action and its result. The planner reads it.

## Git
Skills start with `git pull --ff-only` (if there's a remote) and end with `git add -A`, a short
commit (`capture: <source>`, `connect: <note>`, `recall: <note> pass|fail`, `cards: <note> +<n>`,
`map: <domain>`) and `git push` when `vault.json` → `git.push` is true. Failure: one line, keep
the local commit. Never force-push or rewrite history.

## Flashcards (Anki + FSRS)
Only when `vault.json` → `anki.enabled`. `/cards` adds cards through AnkiConnect (note type
`SB-Concept`, deck `<anki.deckRoot>::<Domain>`). The owner writes the questions; answers come only
from their words in the note; one idea per answer; 2–5 cards per note. Sync to AnkiWeb only when
`anki.sync` is true.

## Work vaults (`vault.json` → `kind: "work"`)
Everything in this vault, and everything in the repos and specs it reads, belongs to the employer.
- **Nothing leaves this machine** except through the git remote in `vault.json`. No connectors, no
  web search or fetch containing internal names, code or text, no copying into another vault or
  any folder outside this vault and the source paths in `vault.json` → `sources`.
- Public docs (a library's documentation, a public RFC) may be fetched, with queries that contain
  no internal names.
- Before any push, check `git remote -v` matches `git.remote`. Anything else: stop and say so.
- **Source repos are read-only.** Read, search and trace them; don't edit, commit, build or run
  them unless the owner explicitly asks.
- **No secrets in notes.** Never copy credentials, tokens, keys, connection strings, customer data
  or personal data. Refer to them by name ("the payments API key in Vault"), never by value.
- Anki only if enabled, and never synced unless `anki.sync` is true.

## Language
Answer in the owner's language (`vault.json` → `language`, or whatever they write in). Never
translate their words in notes.
