# Prompt 5 — Betterment Fields Migration (vault-wide)

> Run this ONCE across your whole vault to backfill Milestones, Deployment, Client Context, 
> and dependency-check fields onto every existing project that predates them. No per-project 
> variable needed — it scans all of `01-Projects\` in a single pass. Safe to re-run — skips 
> anything already filled in.

---

## VARIABLE (fill in before running)

```
VAULT_PATH = <path to your vault>
```

---

## PROMPT

You are upgrading every existing project note in my Obsidian vault at `{VAULT_PATH}` to the 
current structure. Vault-only operation — you do NOT need access to the actual project repos; 
work from what's already documented in each note (Overview, Architecture, Log, Known issues).

### Step 1 — Find every project note
Scan `{VAULT_PATH}\01-Projects\*\` for every file with `type: project` in frontmatter. Process 
each one in turn using Steps 2-4 below.

### Step 2 — Add missing frontmatter fields
If the frontmatter doesn't already have these fields, add them:
```yaml
last-dependency-check: {today's date, YYYY-MM-DD}
client-work: {true if the note's Overview/Architecture/Log mentions a client, contract, or 
  freelance context — otherwise false}
```
Don't overwrite these if they already exist.

### Step 3 — Add missing sections
If the note doesn't already have these sections, add them in this order (Milestones and 
Deployment go after Architecture; Client Context goes after Deployment, before Tasks):

```markdown
## 🏁 Milestones
- (pull any real dates already mentioned in the note's Log or Overview — e.g. "started", 
  "shipped", "launched". If nothing concrete exists in the note, use: 
  "{started date from frontmatter}: Project started")

## 🚀 Deployment
- Live URL: (only if already mentioned somewhere in the note — else leave blank, do not guess)
- Hosting: (only if already mentioned or clearly implied by the documented stack — else blank)
- Env setup notes: (only if genuinely documented in the note — else blank)

## 🤝 Client context
> Leave blank unless client-work is true above.
- 
```

If a section already exists (even partially filled), skip it entirely for that project — 
don't duplicate or overwrite.

### Step 4 — Add "Retros: none yet" to Related section if missing
Check the `## Related` section of each note. If it doesn't have a `Retros:` line, add one:
```
- Retros: none yet
```

### Step 5 — Report back
Give me one summary table across all projects processed: project name, fields added, sections 
added, sections skipped (already existed), and anything left blank because it wasn't 
confidently evidenced. Flag any project where Deployment/Client Context couldn't be filled at 
all from the note alone — those are the ones worth opening the actual repo for later.

---

## RULES
- Never fabricate deployment URLs, hosting providers, or client details — leave blank if not 
  already evidenced in the note itself. This prompt does not have repo access, so don't guess 
  based on the stack alone (e.g. don't assume "Next.js project = hosted on Vercel").
- Don't overwrite or duplicate any section/field that already exists and has real content.
- Don't touch Overview, Goals, Stack, Architecture, Tasks, Known issues, Log, or the ADR/Snippet 
  links in Related — this migration only adds the new sections/fields, nothing else.
- Process every project note found in Step 1 — don't stop partway through without reporting 
  which ones were completed vs. remaining.
