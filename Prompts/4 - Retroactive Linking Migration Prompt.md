# Prompt — Retroactive Linking Migration

> Run this ONCE across your vault (or per-project if you'd rather go gradually) to bring 
> existing Bug/Snippet notes up to the new granular-linking structure. Adds the `## Related` 
> section to old files that predate it, and finds real cross-links that were never made 
> because the structure didn't support them yet. Safe to re-run — skips files already updated.

---

## VARIABLE (fill in before running)

```
VAULT_PATH = <path to your vault>
```

---

## PROMPT

You are migrating existing notes in my Obsidian vault at `{VAULT_PATH}` to a new structure. 
Vault-only operation — do not touch any project's actual codebase.

### Step 1 — Find all existing Bug notes
Scan `{VAULT_PATH}\01-Projects\*\` for files with `type: bug` in frontmatter (typically named 
`Bug - <name>.md`).

For each one that does NOT already have a `## Related` section:
1. Add this section at the end of the file:
   ```markdown
   ## Related
   - Caused by / linked to: 
   - Similar bugs: 
   ```
2. Read the bug's `## Root cause` and `## Fix` sections. Check if the root cause references 
   anything that matches an existing ADR in the same project's folder (`{VAULT_PATH}\01-Projects\<ProjectName>\ADR - *.md`) 
   — e.g. the bug's root cause mentions a migration, a library switch, or a config decision 
   that has a corresponding ADR. If so, fill in:
   ```
   - Caused by / linked to: [[ADR - <decision-name>]]
   ```
3. Check other Bug notes in the SAME project folder — if this bug's symptom or root cause is 
   genuinely similar to another one (same root cause category, recurring issue), link them 
   both ways:
   ```
   - Similar bugs: [[Bug - <other-bug-name>]]
   ```
4. If neither applies, leave the lines blank — don't force a link.

### Step 2 — Find all existing Snippet notes
Scan `{VAULT_PATH}\03-Resources\` for files with `type: snippet` in frontmatter.

For each one that does NOT already have a `## Related snippets` section:
1. Add this section (right before `## Used in`):
   ```markdown
   ## Related snippets
   - 
   ```
2. Compare this snippet's `## What it does` against every OTHER snippet in the same folder. 
   If two or more genuinely solve a similar problem (not just same language — actual similar 
   purpose, e.g. two different realtime-connection hooks, two different retry wrappers), link 
   them both ways:
   ```
   - Related snippets: [[<other-snippet-name>]]
   ```
3. If nothing is genuinely similar, leave it blank.

### Step 3 — Densify Daily Notes (optional, do this only if asked)
Skip this step unless I explicitly ask for it in this session — it's more invasive than 
Steps 1-2. If asked: scan `{VAULT_PATH}\00-Inbox\` for Daily Notes that mention a project by 
name in the `## Log` section but only link the project (e.g. "worked on [[CHORUS]]") without 
linking the specific ADR/snippet/bug involved. Cross-reference that day's work against the 
project's own Log entry for the same date to figure out what was actually touched, and add 
more specific links where confidently possible. Do NOT guess — only add a link if the daily 
note's own text or the project's log entry for that date makes it clear what was worked on.

### Step 4 — Report back
Summarize, without pasting full file contents:
- How many Bug notes were updated with the new section, and how many real links were added 
  (with names)
- How many Snippet notes were updated, and how many real cross-links were added (with names)
- Any files skipped because they already had the section (so no duplicate work happened)
- Anything you weren't confident enough to link, so I can review manually

---

## RULES
- Never fabricate a link — only connect things with real, evidenced overlap (shared root 
  cause, shared problem space). If unsure, leave the line blank.
- Never use `[[bracket links]]` to something that doesn't exist.
- Don't touch any other section of these files — only add/fill the new Related section.
- Skip files that already have a populated `## Related` / `## Related snippets` section — 
  don't overwrite existing manual links.
- This is read-heavy — take the time to actually compare notes to each other rather than 
  linking superficially just to fill the section.
