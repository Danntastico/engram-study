# Changelog

## 0.3.1 (2026-11-17)
- Renamed `second-brain` → **engram-study** (plugin, marketplace and repo). Commands are now
  `/engram-study:<command>` (or just `/<command>` when no other plugin uses the name).
- The Anki note type stays `SB-Concept` so existing cards keep working.

## 0.3.0 (2026-11-17)
- Plugin moved out of the SecondBrain vault into its own repo, so it can be installed on any
  computer and used with any number of vaults.
- Vaults are configured by `vault.json` (owner, `personal` or `work`, git remote, Anki). No more
  hard-coded paths or names in the skills.
- New `/new-vault`: creates a vault (or upgrades a v0.2 vault with `--upgrade`).
- New `/map`: reads repos, specs and docs (or a topic) and drafts a study path you reshape.
- `/capture`: guess before you study (prequestions), points to files to trace for code concepts,
  records wrong guesses, picks the next concept on the map when run with no argument.
- `/recall`: checks code concepts against the current code; first check for `(known)` items.
- `/cards`: off unless the vault enables Anki; AnkiWeb sync only when the vault allows it.
- Work-vault rules: nothing leaves the machine except through the vault's own git remote; source
  repos read-only; no secrets in notes.
- New template `concept-code.md` with a "Where in the code" section.

## 0.2.0 (2026-11-17)
- `/cards` with Anki (FSRS) through AnkiConnect.

## 0.1.0 (2026-11-16)
- `/capture`, `/connect`, `/recall` inside the SecondBrain vault.
