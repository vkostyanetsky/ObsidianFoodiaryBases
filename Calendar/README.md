# Obsidian recurring-task calendar 📆 🔁 ✅

[README in Russian](README.ru.md)

This example shows how to manage recurring tasks in Obsidian without installing
third-party plugins.

In short, it comes down to two steps:

1. Store task lists as ordinary notes with recurrence settings.
2. Use [Obsidian Bases](https://help.obsidian.md/bases) in daily notes to see
   which tasks are due that day.

## 🚀 Try the example

1. Open the [vault](vault) directory as an Obsidian vault.
2. Allow the **Bases**, **Daily notes**, and **Templates** core plugins if
   Obsidian asks.
3. Open [Calendar.base](vault/Calendar/Calendar.base) to see the **Today** and
   **Yesterday** views.
4. Open the Daily notes command. Notes are created in `Daily Notes`, named
   `YYYY-MM-DD`, and include the embedded **Day** view.
5. Open a due calendar entry and copy its checklist from the code block into
   the **Tasks** section of the daily note.

The included `2026-08-15` daily note is a stable demonstration of the **Day**
view: it matches the daily, Saturday, monthly, and annual examples regardless
of the actual current date.

## 👀 How it works

Every note under `vault/Calendar` is a recurring-task definition. The base
selects notes whose `calendar_active` and `calendar_visible` properties are
both enabled, then evaluates either of these schedules:

- `calendar_days_of_week_days`: ISO weekdays from `1` (Monday) through `7`
  (Sunday).
- `calendar_day_of_month_day`: day of the month. If
  `calendar_day_of_month_month` is omitted, the task repeats every month; if a
  month from `1` through `12` is present, it repeats annually.

If a note contains both kinds of schedule, it is due when either schedule
matches. `calendar_icon` is optional and is displayed in the first column.

The base contains three views:

- **Today** — entries due today.
- **Yesterday** — entries that were due yesterday, useful for catching up.
- **Day** — entries matching the date in the current file name. This view is
  designed to be embedded in daily notes named exactly `YYYY-MM-DD`:

```md
![[Calendar.base#Day]]
```

The checklists in recurring notes are fenced code blocks on purpose. They stay
unchanged as templates and do not appear as permanently incomplete tasks in
global task searches.

## 🛠️ Add a recurring task

The `Templates` folder contains ready-made weekly, monthly, and annual task
templates. For example, a task due Monday through Friday looks like this:

```yaml
---
calendar_active: true
calendar_visible: true
calendar_icon: 💼
calendar_days_of_week_days:
  - "1"
  - "2"
  - "3"
  - "4"
  - "5"
---
```

A monthly task due on the first day uses:

```yaml
calendar_day_of_month_day: 1
```

An annual task due on August 15 uses:

```yaml
calendar_day_of_month_month: 8
calendar_day_of_month_day: 15
```

Keep the new note anywhere under `Calendar`; subfolders are for organization
only.

## 📦 Copy into an existing vault

1. Copy `vault/Calendar` and `vault/Templates` into your vault.
2. Copy the calendar section from
   [Daily Note.md](vault/Templates/Daily%20Note.md) into your daily-note
   template.
3. Make sure daily notes use the `YYYY-MM-DD` file-name format.
4. If you place recurring notes outside the top-level `Calendar` folder,
   change `file.inFolder("Calendar")` in `Calendar.base`.
5. Optionally copy the relevant `.obsidian` settings. Review them first so you
   do not overwrite settings from your existing vault.

## ⚠️ Limitations

- The example supports weekdays, monthly dates, and annual dates. It does not
  calculate intervals such as “every two weeks” or rules such as “the last
  business day of the month.”
- Tasks are copied into daily notes manually; the example does not synchronize
  completion back to recurring definitions.
- A date such as the 31st simply does not occur in shorter months.
