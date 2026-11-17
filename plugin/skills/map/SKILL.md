---
name: map
description: Build a study path - for a codebase or product, reads the repos, specs and docs you point to, writes a reference note for each, and drafts a map of the concepts to learn in prerequisite order; for a topic, drafts the map from your goal. You reorder it and write your goal; only then are the first seed notes created. Pass paths, a topic, or "refresh <domain>". Runs only when you type its slash command.
disable-model-invocation: true
---

# Map: from a pile of material to a study path

The first slice of the teacher agent. You do the reading-and-structuring work the owner doesn't
need to memorise (an inventory of what exists and where), and draft the order to learn it in. The
owner then reshapes the draft: building and editing a map teaches far more than being handed one
(g ≈ 0.82 vs 0.37), and an overview up front helps only a little on its own. So the draft is short
and easy to change, and it never explains the concepts: understanding is the owner's to write
later, in `/capture`. Their language. Keep messages short.

## 0. Where to work
The vault is the current folder if it holds `vault.json`; else ask which folder. `cd` there, read
`vault.json` and `CLAUDE.md`, and follow them, the work-vault rules above all. `git pull --ff-only`
if there's a remote (failure: one line, go on).

## 1. What to map
- **Paths** (repos, spec folders, docs) → a `system` map. Check each exists; add new ones to
  `vault.json` → `sources`. In a work vault, only local paths, read-only.
- **A topic** ("Kubernetes", "AI evals") → a `topic` map. Personal vaults may use web search for
  it; work vaults may search public sources only with queries that hold no internal names.
- **`refresh <domain>`** → re-read that map's sources, update their source notes, and list what
  changed (new modules, removed files, new specs) and which map items it affects. Change the map
  only with the owner's OK; never touch concept notes.

Ask for the domain slug [the system's or topic's name, lowercase-hyphenated].

## 2. Inventory (system maps)
For each repo, read in this order and stop when you have the picture; skim, don't read everything:
`README*`, `docs/`, architecture docs and ADRs, API specs (OpenAPI, proto, GraphQL, AsyncAPI),
manifests (`go.mod`, `package.json`, `pom.xml`, …), entry points (`main.go`, `cmd/`, `src/main`,
handlers, routers), the top two levels of the tree, config and infrastructure (Docker, Kubernetes,
Terraform, CI), migrations and schemas, and tests that show the main flows.

Write one reference note per repo or spec set, `sources/<slug>.md` from `templates/source.md`
(`kind: repo` or `spec`, `url:` replaced by the local path):
- Purpose (2–3 lines), stack, how it runs.
- Entry points and main modules, as paths with one-line pointers.
- What it talks to (other repos, databases, queues, external APIs), with the files that show it.
- Key specs and docs, as paths.
- Domain terms found (names only, no definitions).
- Gaps: what you couldn't find or understand.

No secrets, customer data or credentials, ever: names only. No long code excerpts.

## 3. Draft the map
`maps/<domain>.md` from `templates/map.md`, 20–35 concepts. One concept = something the owner can
explain in 2–5 sentences. Each line: `- [[slug]]: what it is in one line · start at <path>`.
No explanations beyond that line.

Default tiers for a system (rename, merge or drop to fit):
1. **Purpose and vocabulary**: what the product does, for whom; the core domain terms and entities.
2. **System shape**: the services/repos, each one's job, how they talk (sync, events, shared DB).
3. **Core flows**: the 3–6 most important requests or jobs, each traced end to end.
4. **Data**: main models, where they live, who owns and changes them.
5. **Contracts and integrations**: APIs, events, external services, the specs that define them.
6. **Running it**: config, deployment, observability, known failure modes.

For a topic: tiers from foundations to applications, prerequisites first.

## 4. The owner reshapes it
Show the tiers as a compact list (names only, not the one-liners) and ask, one at a time:
1. "What do you want to be able to do when this is done?" Their answer goes, verbatim, under
   `## My goal`.
2. "Does this order match how you'd explain the system to a new teammate? Move, rename, merge or
   drop anything." Apply exactly what they say.
3. "Anything you already know well?" Those items get `(known)` and go to the end; they'll be
   checked with `/recall`, not studied.

## 5. First seeds
Create seed notes **only for tier 1** (`templates/concept-code.md` for a system,
`templates/concept.md` for a topic): frontmatter, `source` set to the main source note, and 2–3
guiding questions in comments: questions the owner can guess at before reading (which, how, why,
what happens when …). Fill "Where in the code" with the paths from the map line. Never write
answers. Later tiers get their notes when `/capture` reaches them.

## 6. Log, commit, next
- `log/<today>.md`: `- map: [[<domain>]] (<n> concepts, <m> sources)`.
- `git add -A`, `git commit -m "map: <domain>"`, `git push` if `vault.json` allows.
- End with: "Next: `/capture` with no argument picks the first concept and asks for your guesses."

## Don't
- Explain concepts in the map, the source notes or the seeds. Inventory and pointers only.
- Create notes for every concept at once.
- In a work vault: modify, build or run the source repos; send any of their content anywhere.
