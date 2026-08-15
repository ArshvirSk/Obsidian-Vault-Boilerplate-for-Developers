# Prompt 3 — Vibe Coding Session

> Paste at the START of a coding session with your AI agent, inside a project repo. Keeps the 
> vault updated automatically as a side effect of normal work — no separate documentation step.

---

## VARIABLES (fill in before running)

```
VAULT_PATH = <path to your vault>
PROJECT_NAME = <name of this project>
```

---

## PROMPT

For the rest of this session, alongside whatever coding work I ask you to do, also keep my 
Obsidian vault updated in real time. Do this silently — don't ask permission each time, just 
do it and mention it briefly when you do.

### Context to load first
Read `{VAULT_PATH}\01-Projects\{PROJECT_NAME}\{PROJECT_NAME}.md` now if it exists. If it 
doesn't exist yet, tell me to run the Ingestion Prompt instead — this session prompt assumes 
the base note already exists.

### Throughout the session:

**1. Log meaningfully, not everything**
At natural breakpoints (feature finished, bug fixed, decision made), append to the `## Log` 
section under today's date heading (create the heading if it doesn't exist yet, don't duplicate 
it if it does):
```
### {today's date, YYYY-MM-DD}
- {plain-language summary, 1-2 lines}
```

**2. Catch real architectural decisions immediately**
If we make a genuine trade-off decision this session, create the ADR right away at 
`{VAULT_PATH}\01-Projects\{PROJECT_NAME}\ADR - <decision-name>.md` using the vault's ADR 
structure, based on the actual reasoning discussed in this conversation. Link it in the 
Project Note's Related section, replacing "none yet" if still present.

**3. Catch reusable code as it's written**
If you write something genuinely reusable this session, save it to 
`{VAULT_PATH}\03-Resources\<snippet-name>.md` using the Snippet structure. Check the folder 
first to avoid duplicating an existing similar one — add this project to its "Used in" list 
instead if found. Link it in Related.

**4. Catch bugs worth remembering**
If we debug something non-trivial and non-obvious, once fixed create 
`{VAULT_PATH}\01-Projects\{PROJECT_NAME}\Bug - <short-bug-name>.md` using the Bug Log 
structure with the real root cause and fix. Skip trivial fixes.

**5. Update stack/status if it changed**
Update the `stack` or `status` frontmatter field if a major dependency was added or the 
project's status genuinely changed.

### End of session
Give me a short summary (not full file contents) of everything written to the vault: log 
entries, ADRs, snippets, bugs — and anything you weren't sure was worth documenting.

---

## RULES
- Never fabricate ADR reasoning — only document what was actually discussed.
- Don't log trivial changes — only things that matter 3 months from now.
- Never use `[[bracket links]]` to something that doesn't exist.
- Don't interrupt coding flow to ask permission — just write and mention briefly.
- When unsure if something's worth documenting, prefer a short log line over silence.
