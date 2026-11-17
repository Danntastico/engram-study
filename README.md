# engram-study

*An engram is the physical trace a memory leaves in the brain. This plugin helps you make your own,
by writing, linking and recalling what you learn instead of having an AI remember it for you.*

## What it is

Commands that turn an Obsidian vault into a learning system: build a study path, study one concept
at a time, connect ideas, check yourself from memory, and review with Anki. The agent does the
structure, inventory and checking; **you write the understanding**. Each rule comes from learning
research (generation effect, self-explanation, concept mapping, prequestions, retrieval practice,
spacing). The reasons are in each vault's `CLAUDE.md` and in the project doc *Second Brain —
Research Findings & MVP Design*.

This repo holds **only the plugin**. Your notes live in vaults, which are separate folders with
their own git repos. One plugin, many vaults: e.g. a personal vault on your laptop and a work vault
on your work laptop.

## Install

From GitHub (any computer with access to the repo):

    claude plugin marketplace add Danntastico/engram-study
    claude plugin install engram-study@engram-study

From a local clone:

    claude plugin marketplace add "C:\path\to\engram-study"
    claude plugin install engram-study@engram-study

Update after pulling changes: `claude plugin marketplace update engram-study`, then
reinstall or restart Claude Code.

Moving from `second-brain` (v0.2 inside the vault, or v0.3.0):

    claude plugin uninstall second-brain@danilo-second-brain
    claude plugin marketplace remove danilo-second-brain
    # then install as above

## Create a vault

Open Claude Code in an empty folder and run `/new-vault`. It asks:
1. Name and owner.
2. **Personal or work.** Work = anything from your employer: code, specs, docs. When unsure, work.
3. Language, git remote, Anki on or off, and (work) the local paths of the repos and specs to study.

Then open the folder in Obsidian (Open folder as vault). A vault made with v0.2: run
`/new-vault --upgrade` inside it.

## Commands

| Command | What it does | What you do |
|---|---|---|
| `/new-vault [folder]` | Creates or upgrades a vault | Answer 5–6 questions |
| `/map <paths or topic>` | Reads repos/specs/docs (or a topic), writes reference notes, drafts a study path in prerequisite order | Write your goal; reorder, rename, drop; mark what you already know |
| `/capture [source or concept]` | No argument: next concept on the map. Asks for your guesses, points to what to read or trace, then checks your explanation | Guess, study, explain in 2–5 sentences + why it works |
| `/connect [note]` | Suggests related concepts and the kind of relation | Write the *because* for each link |
| `/cards [note]` | Adds 2–5 Anki cards from your own words (if the vault has Anki on) | Write the questions; approve the cards |
| `/recall [note]` | Asks you to explain a due concept from memory; only a pass makes it `solid` | Explain it with the note closed |

All commands work in the current folder's vault, start with `git pull`, log to `log/<today>.md`
and commit (and push only if the vault allows it).

## A study session (45–60 min, 3–4 times a week)
1. `/recall`: clear what's due (5–10 min).
2. `/capture`: it picks the next concept and asks for your guesses (2–3 min).
3. Study: read the source, or trace the files it points to (20–30 min).
4. Back in `/capture`: explain it in your words; fix what the check finds (10 min).
5. `/connect`, then `/cards` if Anki is on.

## Studying a codebase (work vault)
1. `/new-vault` → **work**, git remote = your employer's git host or none, Anki off (or a separate
   profile without sync), and the paths of the repos and specs.
2. `/map <repo paths>` → a reference note per repo/spec and a draft map: purpose and vocabulary →
   system shape → core flows → data → contracts and integrations → running it.
3. Reshape the map, then study it session by session with `/capture`.
4. `/map refresh <domain>` when the repos change.

**Work-vault rules** (enforced by the vault's `CLAUDE.md`):
- Nothing leaves the machine except through the vault's own git remote; the agent checks the
  remote before pushing. No connectors, no web queries with internal names or code.
- Source repos are read-only: the agent reads and traces, never edits, builds or runs them.
- No secrets, credentials or customer data in notes; names only.
- Anki only if you enable it, and no AnkiWeb sync unless your employer allows it.

## Repo layout
```
.claude-plugin/marketplace.json
plugin/.claude-plugin/plugin.json
plugin/skills/
  new-vault/   (+ template/: CLAUDE.md, GUIDE.md, vault.json, templates/ …)
  map/  capture/  connect/  recall/  cards/
```
The vault template in `plugin/skills/new-vault/template/` is the source of truth for vault rules:
change it there, then run `/new-vault --upgrade` in each vault.
