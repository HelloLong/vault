---
name: "update-daily-file"
description: "Update/create today's daily note in 02daily/ — time→Review/Time accounting, plans→Todo/Schedule, summaries→Summary and Reflection, thoughts→Thinking. Invoke when user updates the daily file."
---

# Update Daily File

Find or create today's Obsidian daily note and route new content into the correct section based on what the user says.

## Locate the daily note

- Daily notes live in `02daily/` with filename `YYYY-MM-DD.md` (see `.obsidian/daily-notes.json`).
- Default to the **current date** for "today's" note.
- If today's note does not exist, create it from `06templates/daily.md`, substituting `{{date}}` with the date.
- If the user names another date (e.g. "yesterday", "2026-10-17"), resolve it to that date's file.

## Daily note structure

```
---
tag: [daily]
date: YYYY-MM-DD
---

# YYYY-MM-DD Daily Note

## Todo
### Schedule
| Priority | Activity | Items |
|----------|----------|-------|

### Review
#### Time accounting
- **total：0min (work 0 / learning 0)**

#### Incomplete tasks

#### Summary and Reflection
- What went well?
  - 
- What could be improved?
  - 
- Thinking
```

## Routing: where content goes

Decide the target section from the user's intent:

| User says | Target section |
|-----------|----------------|
| Plans to do something ("I plan to…", "I will…", "I intend to…") | `#### Schedule` (Todo) — add a table row |
| How long something took / time spent ("it took…", "spent… on…", "how long…") | `#### Time accounting` (Review) — add an entry and update the total |
| Summarize today ("what went well", "what could be improved", daily summary) | `#### Summary and Reflection` — the "What went well?" / "What could be improved?" items |
| Something just happened / a thought / an insight | `#### Summary and Reflection` — append under an item named appropriately (e.g. `- Thinking`) |
| Tasks carried over, unfinished today | `#### Incomplete tasks` |

## Section formats

### Schedule (Todo)
Add one row per planned activity. Keep the existing 3-column table:

```
| Priority | Activity | Items |
|----------|----------|-------|
|          | Recite 30 English words | ~15min |
```

- `Activity` = short verb phrase in English.
- `Items` = expected duration or the deliverable, e.g. `~15min`.
- Leave `Priority` blank unless the user gives one.

### Time accounting (Review)
Add one bullet per activity, then keep a single bold `total` line. Categories are free-form (`work`, `learning`, `commute`, …); sum every category in the total.

```
#### Time accounting
- Recited 30 English words — 15min (learning)
- Drove from Shanghai to Wuxi — 150min (commute)
- **total：165min (work 0 / learning 15 / commute 150)**
```

- Use the full-width colon `：` after `total`, matching the template.
- Recompute the total whenever an entry is added.

### Summary and Reflection (Review)
- `What went well?` and `What could be improved?` each hold nested sub-bullets.
- `- Thinking` is the **last** list item. Create it if it does not exist.

When grouping insights by source, each distinct source becomes a **parent bullet**; the points become **nested sub-bullets**:

```
- Thinking
  - "知行小酒馆" podcast on Xiaoyuzhou, a dialogue with 罗振宇
    - Bring order to this world.
    - Be a long-termist: long-term thinking makes it easy to envision the endgame.
  - In this era, the most important things are vitality and the ability to write.
```

## Language rules

- Write **all content in English**, even when the user writes in Chinese.
- **Keep in the original language (Chinese)** only for these proper nouns:
  - People's names (e.g. `罗振宇`)
  - Addresses / place names (e.g. `上海`, `无锡` — but a place used inside an English sentence may be romanized if clearer)
  - Book / work / show titles (e.g. `知行小酒馆`, `资治通鉴`)
- Preserve Chinese **quotes**, especially classical sayings, verbatim (e.g. `古之学者为己，今之学者为人。`).

## Steps

1. Resolve the target date and build the path (`02daily/YYYY-MM-DD.md`).
2. Read the file; if missing, create it from `06templates/daily.md` with `{{date}}` filled in.
3. Classify each piece of user input using the routing table above.
4. Insert the content into the matching section, following that section's format.
   - Schedule: append rows to the table.
   - Time accounting: append an entry and recompute the `total` line.
   - Summary and Reflection: append sub-bullets under the right item; add the `- Thinking` item if needed.
   - Incomplete tasks: append bullets.
5. Write the file, preserving **all other content unchanged**.
