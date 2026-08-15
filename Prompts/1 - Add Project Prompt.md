# Prompt 1 — Add Project (all-in-one)

> Run this ONCE per project, from inside that project's repo, when adding it to the vault for 
> the first time. Combines what used to be 3 separate prompts (Ingestion, Enrichment, 
> Cross-linking) into a single pass. Safe to re-run later to refresh a project.

---

## VARIABLES (fill in before running)

```
VAULT_PATH = <path to your vault>
PROJECT_NAME = <name of this project>
```

---

## PROMPT

You are adding this project to my Obsidian vault, fully — base note, decisions, reusable code, 
category, and links to related projects, all in one pass. Do NOT touch the project's actual 
codebase — vault-only writes, informed by reading the code.

### Step 1 — Check if a note already exists
Look for `{VAULT_PATH}\01-Projects\{PROJECT_NAME}\{PROJECT_NAME}.md`.
- If it exists, READ it first. Preserve manually-written content — append new findings, 
  don't overwrite.
- If not, create the folder `{VAULT_PATH}\01-Projects\{PROJECT_NAME}\` and the note fresh.

### Step 2 — Analyze the repo
- **Stack**: inspect `package.json`, `requirements.txt`, `pyproject.toml`, `Cargo.toml`, 
  `go.mod`, or equivalent — real languages/frameworks/major libraries only.
- **Structure**: architecture from top-level folders.
- **Git history**: `git log --oneline -20` and `git log --stat -5`.
- **Repo URL**: `git remote -v`.
- **Known issues**: `TODO`/`FIXME`/`HACK` comments, README caveats.
- **Start date**: date of first commit if available.

### Step 3 — Write the Project Note
Create/update `{VAULT_PATH}\01-Projects\{PROJECT_NAME}\{PROJECT_NAME}.md`:

```markdown
---
title: {PROJECT_NAME}
status: active
type: project
stack: [detected stack, as a YAML list]
repo-url: {detected repo URL}
started: {detected or estimated date, YYYY-MM-DD}
tags: [project]
---

# {PROJECT_NAME}

## 📋 Overview
> 2-3 sentence summary based on real evidence in the repo.

## 🎯 Goals
- (infer from README/docs, otherwise leave for the user)

## 🛠️ Stack
- (full detected stack, one item per line)

## 📐 Architecture
> Real structure: key folders/modules, data flow. Cite actual paths.

## ✅ Tasks
- [ ] (leave empty unless an obvious in-progress TODO is worth surfacing)

## 🐛 Known issues / debt
- (real TODO/FIXME/HACK findings, with file path if possible)

## 📓 Log
### {today's date, YYYY-MM-DD}
- Added to vault
- (summarize last 5-10 commits in plain language)

## 🔗 Related
- ADRs: none yet
- Snippets used: none yet
- Area: none yet
- Similar stack: none yet
- Repo: {repo URL}
```

### Step 4 — Find real architectural decisions (ADRs)
Look for: config/infra trade-offs (hosting, database, auth, state management, deploy pipeline), 
migration evidence in git history (commits with "migrate"/"switch"/"replace"), README "why we 
use X" sections, non-default config with comments.

For each distinct decision found, create 
`{VAULT_PATH}\01-Projects\{PROJECT_NAME}\ADR - <short-decision-name>.md`:

```markdown
---
title: <short-decision-name>
date: {date from git history, else today}
status: accepted
type: adr
project: {PROJECT_NAME}
tags: [adr]
---

# ADR: <short-decision-name>

## Status
accepted

## Context
> Real evidence only.

## Decision
> What was decided, stated plainly.

## Alternatives considered
> Only if documented in repo. Otherwise: "Not documented in repo — inferred decision only."

## Consequences
> What this means for the codebase now.

## Related
- Project: [[{PROJECT_NAME}]]
```

Skip entirely if nothing evidenced.

### Step 5 — Find real reusable snippets
Non-trivial, genuinely reusable, not boilerplate. Max 5. Check `{VAULT_PATH}\03-Resources\` 
first — if a similar one exists, add this project to its "Used in" instead of duplicating.

New snippets: `{VAULT_PATH}\03-Resources\<snippet-name>.md`:

```markdown
---
title: <snippet-name>
language: <language>
tags: [snippet]
source: {PROJECT_NAME}
created: {today's date, YYYY-MM-DD}
---

# <snippet-name>

## What it does
> One-liner, specific.

## Code
```<language>
<code, cleaned of secrets/env values>
```

## Usage / gotchas
- 

## Used in
- [[{PROJECT_NAME}]]
```

Skip if nothing genuinely snippet-worthy.

### Step 6 — Identify if this project belongs to an Area
Check `{VAULT_PATH}\02-Areas\`. If it fits an existing area, add it under that area's 
`## Active` section. If no relevant area exists and this is clearly a recurring category 
(not one-off), create ONE new area note. Skip if genuinely standalone.

### Step 7 — Cross-link with existing projects (this replaces the separate Cleanup step)
Read the `stack` frontmatter of every OTHER project note already in `{VAULT_PATH}\01-Projects\*\`. 
If this project shares 2+ non-trivial stack elements with another (not generic things like 
"JavaScript" or "Git"), add a line under THIS project's `## Related`:
```
- Similar stack: [[OtherProjectName]] (shares: X, Y)
```
AND add the reverse link under the OTHER project's `## Related` section too, so it's 
bidirectional. Only for genuinely meaningful overlap — don't force it. If none found, leave 
`Similar stack: none yet`.

### Step 8 — Update the Project Note's Related section
Fill in ADRs/Snippets/Area/Similar stack from Steps 4-7, replacing the "none yet" placeholders. 
Leave Overview/Architecture/Log untouched.

### Step 9 — Report back
Summarize: what was created/updated, ADRs created (or none), snippets created (or none), area 
linked (or none), similar-stack projects linked (or none), anything uncertain.

---

## RULES
- Never fabricate information — leave blank or say "not documented" if unsure.
- Do not modify anything outside `{VAULT_PATH}` except the cross-link update in Step 7 
  (which touches OTHER project notes' Related sections only — nothing else in them).
- Never use `[[bracket links]]` to something that doesn't exist — plain text instead.
- Preserve existing manual content on re-runs; check for existing files before duplicating.
