---
name: "update-daily-file"
description: "Find or create today's Obsidian daily note and add insights to the Thinking list under Summary and Reflection, grouped by source. Invoke when the user asks to add content to the daily file/note or its summary."
---

# Update Daily File

Add captured insights and notes into the Obsidian daily note, organized under a `Thinking` section.

## Locate the daily note

- Daily notes live in `02daily/` with filename `YYYY-MM-DD.md` (see `.obsidian/daily-notes.json`).
- Default to the current date for "today's" note.
- If today's note does not exist, create it from `06templates/daily.md`, substituting `{{date}}` with the date.

## Where content goes

A daily note follows this structure:

```
# {{date}} Daily Note

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

Insights go into `#### Summary and Reflection`, appended under the `- Thinking` list item, which is the **last** entry after "What went well?" and "What could be improved?". Create the `- Thinking` item if it does not exist.

## Group by source

- Each distinct source (e.g. a podcast episode or a dialogue) becomes a **parent bullet** under `- Thinking`.
- The individual points from that source become **nested sub-bullets** under the parent.

```
- Thinking
  - "知行小酒馆" podcast on Xiaoyuzhou, a dialogue with 罗振宇
    - <point 1>
    - <point 2>
```

## Language rules

- Default to **English** for note content.
- Keep **proper nouns** (people, show names) in their original language when the user asks (e.g. `知行小酒馆`, `罗振宇`).
- Keep original **Chinese quotes** (especially classical sayings) in Chinese when the user asks.

## Steps

1. Resolve today's date and build the path (`02daily/YYYY-MM-DD.md`).
2. Read the file; if missing, create it from `06templates/daily.md` with `{{date}}` filled in.
3. Locate `#### Summary and Reflection` and the `- Thinking` item (append it if missing).
4. Find the parent bullet matching the source.
   - If it exists, append the new point(s) as sub-bullets.
   - If not, add a new parent bullet under `- Thinking` with the point(s) as sub-bullets.
5. Write the file, preserving all other content unchanged.
