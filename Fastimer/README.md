# Obsidian Fastimer (Bases) ⏳ 🍽️ 🪵

[README in Russian](README.ru.md)

This example Obsidian vault keeps a log of intermittent fasting: for every fast
it shows when you started, how long you planned to fast, when the fast is
expected to end, and how long it actually lasted. While a fast is running, its
duration is counted from the start up to the current moment.

Everything is built on plain Obsidian Bases, with a bit of automation from the
Templater plugin.

---

## 🚀 Getting started

### 1. Install Templater

Enable community plugins in your vault and install
[Templater](https://github.com/SilentVoid13/Templater).

### 2. Configure Templater

> [!tip]
> The example already includes a Templater
> [settings file](vault/.obsidian/plugins/templater-obsidian/data.json), so
> you'll probably be good to go out of the box.

Set the following options:

- `Template folder location` — choose the `Templates` folder
- `Trigger Templater on new file creation` — enable
- `Folder templates` — set up for one folder:
  - `Records`: `Templates/Record.md`

### 3. Log your fasts

Each fast is a separate note in the [Records](vault/Records) folder. The easiest
way to create one is by pressing `New` right in the
[base view](vault/Fasting.base).

Fast note properties (created automatically by Templater when you make a new
file):

- `length` — planned length of the fast in hours (auto-filled with `12`;
  fractional values such as `13.5` are fine)
- `start` — when the fast started (auto-filled with the current date and time)
- `end` — when the fast ended; leave it empty while the fast is running
- `timestamp` — time / timestamp (used for sorting and as the file name,
  auto-filled)

So the routine is: create a note when you stop eating, and fill in `end` when
you break the fast. Everything else is calculated by the base.

> [!tip]
> `start` and `end` are ordinary date-and-time properties, so you can correct
> them right in the properties panel if you remembered about the log a bit
> later than you should have.

---

## 👀 How to view your data

The [Fasting.base](vault/Fasting.base) file has two configured views:

- **All** — all fasts for all time, newest first
- **Day** — the fasts that overlap one particular day

Both views show the same set of columns:

- **Start** — start of the fast
- **Planned** — planned length as `HH:mm`
- **End (planned)** — `start` plus the planned length, that is when the fast is
  supposed to end
- **Actual** — actual length as `HH:mm`; for a running fast (empty `end`) it is
  counted up to the current moment, so the view doubles as a timer
- **End (actual)** — when the fast actually ended

The **Day** view takes the date from the name of the file the base is embedded
into, so it is designed to be embedded in daily notes named exactly
`YYYY-MM-DD`:

```md
![[Fasting.base#Day]]
```

A fast usually crosses midnight, so this view shows every fast that touches the
day — the one started on the previous evening as well as the one started in the
evening of the day itself. A running fast is always included.

---

## 🛠️ How to customize it

### Change the default planned length

The default of `12` hours lives in the [record template](vault/Templates/Record.md):

```js
output += `length: 12\n`;
```

Put your own usual number there — the value is only a starting point, you can
always change it in a particular note.

### Track whether the plan was met

Let's say you want to see how far the actual length was from the plan.

Add a formula to the base:

```js
((if(end, end, now()) - start).hours - length).round(1)
```

Then add a column with this formula to the views. A positive number means you
fasted longer than planned, a negative one means you broke the fast early.

### Keep a comment on a fast

The note body is free, so the simplest way to comment a fast is to write the
text right in the note. If you would rather see comments in the table, add a
property to the [record template](vault/Templates/Record.md):

```js
output += `note: \n`;
```

Then add `note` to the `order` list of the views in
[Fasting.base](vault/Fasting.base) — and that's it, the column will show up.
