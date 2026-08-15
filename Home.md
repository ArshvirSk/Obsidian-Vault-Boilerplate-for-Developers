---
title: Home
type: dashboard
tags: [dashboard]
---

# 🏠 Home

## 🟢 Active Projects
```dataview
TABLE
  status AS "Status",
  join(stack, ", ") AS "Stack",
  file.mtime AS "Last touched"
FROM "01-Projects"
WHERE type = "project" AND status = "active"
SORT file.mtime DESC
```

## 🕸️ Going Stale (active, but untouched 30+ days)
```dataview
TABLE
  file.mtime AS "Last touched"
FROM "01-Projects"
WHERE type = "project" AND status = "active" AND file.mtime < date(today) - dur(30 days)
SORT file.mtime ASC
```

## 🟡 Dormant / Paused Projects
```dataview
TABLE
  status AS "Status",
  join(stack, ", ") AS "Stack"
FROM "01-Projects"
WHERE type = "project" AND status = "dormant"
SORT file.name ASC
```

## 📦 Archived Projects
```dataview
TABLE
  join(stack, ", ") AS "Stack"
FROM "01-Projects" OR "04-Archive"
WHERE type = "project" AND status = "archived"
SORT file.name ASC
```

## 📐 Recent ADRs (last 10)
```dataview
TABLE
  project AS "Project",
  date AS "Date"
FROM "01-Projects"
WHERE type = "adr"
SORT date DESC
LIMIT 10
```

## 🐛 Open Bugs
```dataview
TABLE
  project AS "Project",
  date AS "Date"
FROM "01-Projects"
WHERE type = "bug" AND status = "open"
SORT date DESC
```

## 📦 Dependency Check Needed (never checked, or 90+ days stale)
```dataview
TABLE
  join(stack, ", ") AS "Stack",
  last-dependency-check AS "Last checked"
FROM "01-Projects"
WHERE type = "project" AND status = "active" 
  AND (!last-dependency-check OR last-dependency-check < date(today) - dur(90 days))
SORT last-dependency-check ASC
```

## 🔁 Recent Retros
```dataview
TABLE
  project AS "Project",
  date AS "Date"
FROM "01-Projects"
WHERE type = "retro"
SORT date DESC
LIMIT 10
```

## 📚 Lessons Rollup
> Dataview can't reliably query text inside `## Known issues / debt` or retro sections — that 
> content isn't in frontmatter. To browse lessons across all projects at once: `Ctrl/Cmd+Shift+F` 
> → search "Known issues" or open each Retro note directly (linked below).

## 🤝 Client Work
```dataview
TABLE
  status AS "Status",
  join(stack, ", ") AS "Stack"
FROM "01-Projects"
WHERE type = "project" AND client-work = true
SORT file.name ASC
```

## 🧩 Snippets Library
```dataview
TABLE
  language AS "Language",
  source AS "From project"
FROM "03-Resources"
WHERE type != "area"
SORT language ASC
```

## 🗂️ Areas
```dataview
TABLE type AS "Type"
FROM "02-Areas"
WHERE type = "area"
```

## 📓 Recent Daily Notes (last 7)
```dataview
TABLE date AS "Date"
FROM "00-Inbox"
WHERE tags[0] = "daily" OR contains(tags, "daily")
SORT date DESC
LIMIT 7
```

## 🔎 Everything by Stack
> Quick filter — edit the stack name below to see every project using it.
```dataview
TABLE join(stack, ", ") AS "Stack"
FROM "01-Projects"
WHERE type = "project" AND contains(stack, "Supabase")
```
