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

**4. Catch bugs worth remembering — and link them to their real cause**
If we debug something non-trivial and non-obvious, once fixed create 
`{VAULT_PATH}\01-Projects\{PROJECT_NAME}\Bug - <short-bug-name>.md` using the Bug Log 
structure with the real root cause and fix. Skip trivial fixes.

In the bug's `## Related` section:
- If the bug traces back to a specific ADR (a past decision caused or contributed to it), link 
  it: `Caused by / linked to: [[ADR - <decision-name>]]`
- If the root cause involved a specific snippet, link that instead/also.
- Check `{VAULT_PATH}\01-Projects\{PROJECT_NAME}\` for other Bug files in this project — if 
  this bug is similar to or a recurrence of a past one, link it: 
  `Similar bugs: [[Bug - <other-bug-name>]]`
- If none of these apply, leave the relevant line blank rather than forcing a link.

**5. Link snippets to related snippets, not just their source project**
When saving a new snippet (from rule 3), also check `{VAULT_PATH}\03-Resources\` for any 
other snippet solving a similar problem (even in a different language/project) — e.g. two 
different realtime-connection hooks, two different retry wrappers. If one exists, add it under 
the new snippet's `## Related snippets` section: `[[<other-snippet-name>]]`, and add the 
reverse link in the other snippet's file too. Only link genuinely similar patterns — not 
everything in the same language.

**6. Update stack/status if it changed**
Update the `stack` or `status` frontmatter field if a major dependency was added or the 
project's status genuinely changed.

**7. Log milestones as they happen**
If this session reaches a real checkpoint (MVP working, first deploy, feature launched, 
project being paused/archived), add a line to the `## Milestones` section with today's date, 
not just the `## Log`.

**8. Suggest a retro at real milestones**
If this session reaches a significant milestone (shipped, launched, or you tell me you're 
pausing/wrapping up this project), ask me if I want to do a quick retro before we finish the 
session. If I say yes, create `{VAULT_PATH}\01-Projects\{PROJECT_NAME}\Retro - <milestone-name>.md` 
using the vault's Retro structure, and fill it based on what we actually discussed — real 
wins, real regrets, not generic reflection. Link it under the Project Note's `## Related` 
section, replacing "none yet" if present. Don't create a retro without asking first — this is 
the one thing in this prompt that DOES need my go-ahead, since it involves real reflection, 
not just documentation.

**9. Update the dependency-check date if you actually check dependencies this session**
If I ask you to check/update packages this session, update `last-dependency-check` in the 
frontmatter to today's date once done.

**7. Link generously in the Daily Note too**
If today's Daily Note (`{VAULT_PATH}\00-Inbox\`) exists and I'm using it this session, when 
appending to it don't just link the project — link the specific thing worked on: the exact 
ADR, snippet, or bug touched, e.g. "Fixed the audio sync issue in [[CHORUS]] — traced back to 
[[ADR - CHORUS - Server-Side Audio Extraction]], had to revisit [[NTP Client Handshake]] too." 
More specific links compound the vault's usefulness far more than a single project-level link.

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
- Prefer specific links over general ones — linking to the exact ADR/snippet/bug touched is 
  more valuable than only linking the parent project every time.
