---
name: new-vault
description: Create a learning vault in a folder (personal or work) - asks who owns it, whether it holds employer information, where git may push and whether Anki is on, then writes vault.json, the agent rules, guide, templates and folders. With --upgrade, brings a vault made before plugin v0.3 up to date without touching its notes. Pass the folder; default the current one. Runs only when you type its slash command.
disable-model-invocation: true
---

# New vault

Sets up a vault the other commands can work on. The files to copy are in this skill's
`template/` folder (next to this file). Short; one question at a time; the user's language.

## 1. The folder
What they passed, else the current folder. If it already has `vault.json`, it's a vault: stop and
say so (offer `--upgrade` only if they asked for it). If it has other files, say which and ask
before writing there; never overwrite or delete anything that isn't from the template.

## 2. Ask (one at a time, defaults in brackets)
1. Vault name [the folder's name] and owner's first name.
2. **Personal or work?** Work = it will hold anything from an employer: code, specs, docs,
   meeting notes. When unsure, it's work.
3. Language [the one they're writing in].
4. Git:
   - personal: the remote URL if they have one, else none for now; `push: true` once a remote exists.
   - work: the employer's git host only (their company GitHub/GitLab org), or none (local only).
     If they give a personal account or a personal host, say why that's not allowed in a work
     vault and ask again.
5. Anki on? [personal: yes; work: no]. For work, if they say yes: `sync: false` unless they confirm
   their employer allows AnkiWeb; suggest a separate Anki profile named after the vault.
6. Work only: the local paths of the repos, specs and docs to study (they can add more later with
   `/map`). Check each path exists; store them in `sources`.

## 3. Write
- `vault.json` from `template/vault.json` with the answers.
- `CLAUDE.md`, `GUIDE.md`, `README.md` from the template (replace `<VaultName>`).
- `template/gitignore` → `.gitignore`; `template/obsidian-app.json` → `.obsidian/app.json`.
- `templates/` (all files).
- Empty folders with a `.gitkeep`: `concepts/`, `maps/`, `sources/`, `projects/`, `log/`.
- `log/<today>.md`: `- setup: vault created (<kind>)`.

## 4. Git
If the folder isn't a repo: `git init -b main`, `git add -A`, `git commit -m "Vault scaffold"`.
If a remote was given: `git remote add origin <url>` and `git push -u origin main`. (A new GitHub
repo: `gh repo create <name> --private --source . --push`, only for a personal vault or within the
employer's org.) Failure: say what to run, don't retry.

## 5. Finish
Tell them in 3–4 lines: open the folder in Obsidian (Open folder as vault), what's in
`GUIDE.md`, and the next step: `/map <paths or topic>` to build the first study path.

## --upgrade (vault made before v0.3)
For a folder with `CLAUDE.md` and `maps/` but no `vault.json`:
1. Ask questions 1–5 above; write `vault.json`.
2. Replace `CLAUDE.md` and `GUIDE.md` with the template versions (show the old files' extra
   sections, if any, and ask whether to keep them in `GUIDE.md`). Add any missing file from
   `templates/`. Leave `concepts/`, `maps/`, `sources/`, `log/` and `README.md` as they are.
3. If the vault still holds `plugin/` or `.claude-plugin/`, tell them the plugin now lives in its
   own repo and they can delete those folders.
4. Commit `upgrade: vault v0.3`.
