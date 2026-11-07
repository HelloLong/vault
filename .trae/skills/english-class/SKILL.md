---
name: "english-class"
description: "Records English class notes from images into 03permanent/English-class-YYYY-MM-DD.md and logs class duration via the daily skill. Invoke when user attends an English class, provides notes images, or states the class duration."
---

# English Class Notes

Record the content of an English class from the user's notes images into a permanent note, and optionally log the class duration in the daily note's time accounting.

## When to invoke

- The user says they attended / took an English class (e.g. "我上了英语课", "今天有英语课", "I had an English class").
- The user provides English class notes images and asks to record them.
- The user tells how long the English class took and wants it logged.

## File location and naming

- Save to `03permanent/English-class-YYYY-MM-DD.md`.
- Use the **class date**. If the user names a date, use it; otherwise use today's date.
- If a file for that date already exists, read it first and **append / merge** content rather than overwriting.

## Note format

Follow the existing convention used by other `English-class-*.md` notes:

```
#English 

<unit number> <topic>

- word n./v./adj. 中文释义
- phrase 中文释义

## Useful phrases

- sentence pattern 中文释义
```

Rules:
- First line is `#English ` (inline tag, with a trailing space).
- Then a blank line, then the unit/lesson line (e.g. `11 telling a story`, `Unit 10 — Making, Accepting and Declining`).
- Vocabulary as a bullet list: `word <part of speech>. 中文释义`. Include the part of speech when visible in the notes.
- If the notes contain dialogues or sentence patterns, put them under a `## Useful phrases` heading as bullets with Chinese translations.
- Keep the original English from the notes; add Chinese translations where shown.

## Processing images

- Read each image the user provides.
- Extract all vocabulary, phrases, and example sentences visible in the notes.
- If an image is unclear, note the uncertainty or ask the user to clarify.
- Existing English-class notes do **not** embed images; just transcribe the content into the note. Do not save the images to `07assets/` unless the user explicitly asks.

## Time tracking

- If the user provides how long the class took (e.g. "这节课花了90分钟", "the class took an hour"), log it in today's daily note **by invoking the `daily` skill**.
  - Activity: `Attended English class` (or describe the unit/topic, e.g. `English class — telling a story`).
  - Duration: the number of minutes given.
  - Category: `learning`.
- The `daily` skill handles writing to `02daily/YYYY-MM-DD.md` → `#### Time accounting` and updating the total. **Do not edit the daily note directly; invoke the `daily` skill.**
- If the user says they will provide the duration later, record the notes now and add the time entry when they do.

## Steps

1. Determine the class date (user-provided or today).
2. Read each provided image and extract vocabulary / phrases / sentences.
3. Check whether `03permanent/English-class-YYYY-MM-DD.md` exists; read it if so.
4. Compose the note in the format above and write it (append if the file exists).
5. If a duration was given, invoke the `daily` skill to add a `learning` time-accounting entry for the English class.
6. Report the created/updated note path to the user.
