# Obsidian Vault Boilerplate for Developers

A ready-to-use Obsidian vault structure for developers, built around vibe-coding with AI 
agents (Antigravity, Claude Code, Cursor, etc.). Documentation is generated as a side effect 
of your normal coding sessions, not a separate chore.

---

## 1. Setup

1. Install [Obsidian](https://obsidian.md).
2. In Obsidian: **Open folder as vault** → select this `Dev-Brain` folder (or copy its 
   contents into wherever you want your vault to live).
3. Settings → **Files & Links**:
   - Default location for new notes → `00-Inbox`
   - New link format → Shortest path
   - Automatically update internal links → ON
4. Settings → **Community plugins** → turn off Restricted mode → install:
   - **Templater** (required — templates use its syntax)
   - **Dataview** (required — powers `Home.md`)
   - **Git** (recommended — auto-commit/push your vault)
   - **Homepage** (optional — auto-opens `Home.md` on launch)
   - **Advanced Tables**, **Excalidraw** (optional but useful)
5. Settings → **Templater** → set "Template folder location" to `Templates`.
6. Settings → **Git**:
   - Set the path to your `git.exe` if it's not auto-detected (`where git` in a terminal to find it)
   - Set a vault backup interval (e.g. every 10-30 min)
   - Turn on "Auto push after commit" and "Pull changes before push"
7. In a terminal, inside this folder:
   ```bash
   git init
   git remote add origin <your-private-repo-url>
   git branch -M main
   git add .
   git commit -m "Initial vault structure"
   git push -u origin main
   ```

---

## 2. Folder structure

```
00-Inbox/       Quick captures, daily notes — unsorted by default
01-Projects/    One subfolder per project — contains the project note + its ADRs
02-Areas/       Ongoing recurring responsibilities (Freelance, Client Work, etc.)
03-Resources/   Reusable snippets, reference material
04-Archive/     Dead/completed projects
Attachments/    Images, files pasted into notes
Templates/      All note templates (Templater-powered)
Prompts/        The 4 AI-agent prompts — see below
```

Each project lives as: `01-Projects/<ProjectName>/<ProjectName>.md` plus any 
`ADR - <decision>.md` or `Bug - <name>.md` files for that project, all in the same subfolder.

---

## 3. Templates (in `Templates/`)

| Template | Use for |
|---|---|
| Daily Note | Your day-to-day scratchpad — commands, thoughts, quick log |
| Project Note | One per project — overview, stack, architecture, log, related links |
| ADR | A real architectural decision with trade-offs |
| Snippet | A genuinely reusable piece of code |
| Bug Log | A non-trivial bug — symptom, root cause, fix |
| Meeting Note | Notes/decisions/action items from a meeting |
| Area | A recurring responsibility that spans multiple projects |

Insert any of these via `Ctrl/Cmd + P` → "Templater: Insert Template".

---

## 4. The 3 AI-agent prompts (in `Prompts/`)

These are meant to be pasted into your coding agent (Antigravity, Claude Code, etc.), not 
used inside Obsidian itself. Fill in `VAULT_PATH` and `PROJECT_NAME` at the top of each before 
running.

| # | Prompt | When to use | Frequency |
|---|---|---|---|
| 1 | **Add Project** | First time adding an existing project to the vault — writes the base note, ADRs, snippets, area, AND cross-links it to every other project with shared stack, all in one pass | Once per project |
| 2 | **Vibe Coding Session** | Paste at the start of every regular coding session — keeps the vault updated live as you work (logs, ADRs, snippets, bugs) | Every session |
| 3 | **Vault Hygiene** | Occasional pass across the whole vault to catch drift — renamed stacks, broken/placeholder links, manually-edited frontmatter | Monthly-ish, optional |

**Typical flow for a new project:** run Prompt 1 once, from inside that project's repo.
**Typical flow for ongoing work:** paste Prompt 2 at the start of each session, then just code.
**Occasionally:** run Prompt 3 once across the whole vault if something looks off.

---

## 5. Daily usage

- Start your day: insert a Daily Note, jot what you're working on.
- Coding sessions: use Prompt 2 — the agent logs, documents ADRs/snippets/bugs automatically.
- Don't over-organize — write first, link with `[[brackets]]` as you naturally reference 
  things, tidy later.
- Find things via `Ctrl/Shift + F` (search), the Backlinks panel on any note, or `Home.md` 
  (see below) — not folder browsing, not Graph View.
- Weekly: skim recent Daily Notes for anything recurring that deserves its own note. Archive 
  dead projects into `04-Archive/` (update `status: archived` in frontmatter).

---

## 6. Home dashboard (`Home.md`)

A Dataview-powered dashboard sitting in the vault root. Requires the **Dataview** plugin 
(Community plugins → Browse → search "Dataview" → install → enable). It auto-generates, with 
zero manual upkeep:

- Active projects (sorted by last touched)
- Projects going stale — active but untouched 30+ days
- Dormant / archived projects
- Recent ADRs, open bugs, snippet library, areas, recent daily notes
- A filterable "everything using stack X" query

Open it and pin the tab, or install the community plugin **Homepage** to auto-open it on launch.

---

## 7. Graph View tips

Graph View is a nice-to-glance-at side effect, not a working tool. If you want it readable:
- Settings (gear icon in Graph View) → Filters → "Existing files only" ON
- Groups tab → color by tag: `tag:#project`, `tag:#adr`, `tag:#snippet`, `tag:#area`
- Forces tab → increase Repel Force to separate clusters
- Save the config as a preset (star/save icon) so it persists

---

## 8. Notes

- All templates use [Templater](https://silentvoid13.github.io/Templater/) syntax 
  (`<% tp.date.now(...) %>`, `<% tp.file.title %>`) — install the plugin or strip that syntax 
  and fill fields manually.
- `.gitignore` excludes Obsidian's local workspace/cache state, not your notes.
- This boilerplate assumes Windows-style paths in the prompts — adjust to `/` for Mac/Linux.
