# Prompt 4 — Vault Cleanup

> Run occasionally with your AI agent pointed at the VAULT folder itself (not a project repo). 
> Fixes phantom/placeholder links and adds cross-project links where genuinely relevant.

---

## VARIABLE (fill in before running)

```
VAULT_PATH = <path to your vault>
```

---

## PROMPT

You are cleaning up existing notes in my Obsidian vault at `{VAULT_PATH}`. Vault-only 
operation — do not touch any project's actual codebase.

### Step 1 — Scan every project note
For each `.md` file with `type: project` in frontmatter (under `{VAULT_PATH}\01-Projects\*\`), 
read its `## Related` section and anywhere else with `[[wikilinks]]`.

### Step 2 — Identify phantom/placeholder links
A phantom link points to a note title that does NOT exist as a real file, and is clearly a 
template artifact — e.g. `[[ADR]]`, `[[Snippet]]`, `[[Project Note]]`, `[[Daily Note]]` used 
literally instead of a real reference.

### Step 3 — Fix each phantom link
- If genuinely no ADR/snippet exists yet: replace with plain text, e.g. `ADRs: none yet`.
- If a real ADR/snippet file DOES exist for this project (check its subfolder and 
  `03-Resources\` for files with `source: {ProjectName}`): replace with the real link, e.g. 
  `[[ADR - Migrated to Expo]]`.

### Step 4 — Cross-link related projects
Compare each project's `stack` frontmatter against every other project's. If two or more 
projects share 2+ non-trivial stack elements (not generic things like "JavaScript" or "Git"), 
add under `## Related` in both:
```
- Similar stack: [[OtherProjectName]] (shares: X, Y)
```
Only for genuinely meaningful overlap — don't force it.

### Step 5 — Report back
Per project touched: phantom links fixed, real links added, cross-links added. Summary only, 
no full file contents.

---

## RULES
- Never create a `[[link]]` to a note that doesn't exist — plain text if in doubt.
- Don't invent ADRs/snippets — this only fixes links, doesn't generate new content.
- Only modify `## Related` sections — leave everything else untouched.
- Safe to re-run periodically.
