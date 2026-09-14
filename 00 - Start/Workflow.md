---
type: reference
updated: 2026-09-14
---

# Free conversation: the walking session

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

> **This note describes free conversation only**, which is still alive but is no
> longer the main system. The system is the five-phase cycle with the `/de`
> commands → **[[Lektionen]]**. What is still current here: the session shape, the
> offline study views, and the five silent failures at the end.

A 15-30 minute voice session while walking, with the [[Modus - Sprechen]] GPT.
Unlike a lesson, it has no gate, no fixed material and no sequence: it is the only
piece of the system that is genuinely a conversation.

It is one of the four GPTs in [[GPTs]], and the only one whose prompt is written
once instead of generated each lesson. Setting it up is described there; what it
does is described here.

## What the tutor does, unprompted

- Turn 1: a short greeting and **three numbered themes**, so the answer can be a
  number while walking. It skips whatever is in *recent themes*.
- Asks for production at the **production level** and speaks at the
  **comprehension level** from [[Lernprofil]]. The gap is deliberate.
- Introduces **6-10 new words aloud**, never more, always in context. And **1-2
  grammar rules**, using them itself several times before asking for them.
- **Recycles on purpose** the vocabulary and structures in its learner values. If a
  mistake from the watch list reappears, it works on it that day.
- Corrects on the spot only what blocks understanding; the rest is batched and
  reviewed every 5-6 exchanges. **It logs every mistake, including the ones it does
  not mention.**
- It does not agree to be nice: no "Genau" or "Super" as an opener, and no
  confirming half-finished sentences.

## What can be said at any moment

`warte` (be quiet) · `protokoll fertig` (only answer after `fertig`) ·
`wiederhole` · `langsamer` · `auf Spanisch` · `was bedeutet …` ·
`anderes Thema` · `einfacher` / `schwieriger`

And for the phone, not the prompt: **Voice Isolation** in Control Centre, and
headphones. Being cut off mid-sentence is the app's turn detection, not the tutor.

## From chat to vault

Say it is finished. The tutor says aloud how many words and rules came up, one
encouraging sentence, and that the list is written in the chat. **It never reads
the list aloud.**

Then it writes **the five-section block** in the chat → [[E-Mail-Format]].

Actions do not run in voice. So: **leave voice mode and type "envía la lista de
hoy"** in that same chat. Then it calls the Action, subject `Deutsch YYYY-MM-DD`.
In a new chat there is no block to reuse and it has to be pasted.

Processing: [[Verarbeitung]]. Free-conversation sessions carry `lektion: "-"`,
because they belong to no lesson.

## Offline study

Each view answers a different question. They do not all need looking at:

| When | View | What to do |
|---|---|---|
| Any time you study | [[Schwachstellen.base\|Schwachstellen]] | what gets failed. **The revision queue.** It empties only through the `OK` block of `/de studium` or `/de gramatik`. |
| While processing | [[Ohne Beispiel.base\|Ohne Beispiel]] | write the sentences left blank |
| Before a session | [[Nicht gesprochen.base\|Nicht gesprochen]] | words never produced. Use them today. |
| Starting a topic | [[Nach Niveau.base\|Nach Niveau]] | what is there, by difficulty |

**Four of the six, on purpose.** [[Nach Thema.base|Nach Thema]] is for preparing a
story and [[Aktuelle Lektion.base|Aktuelle Lektion]] is for following a lesson in
progress: both belong to the cycle, not to studying alone between sessions. The full
list and what each one filters on is in [[Ansichten - Referenz]].

The first two rows are **work lists** — you open them, empty them, and the rows
disappear on their own because the note changed. The last two are **instruments**:
they never empty, and what they tell you is the trend, not the contents.

**On spaced repetition.** There is none, deliberately. The `anki` field and its
export view were inherited from the source document and removed on 2026-09-14: a
field nobody writes and nobody reads is worse than no field, because it looks like
a feature, and the view was a queue silently filling up towards an export step that
did not exist.

What the system has instead is the 30% of older material in every `/de studium`
and [[Schwachstellen.base|Schwachstellen]] as the queue of what gets failed. That
is not spaced repetition — it schedules by error, not by interval — and it may not
be enough. If forgetting becomes visible after ten lessons, the fix is the
`obsidian-spaced-repetition` plugin, which is already installed and works **on the
notes themselves**. Not Anki: exporting creates a second copy of the vocabulary
that starts drifting the same day, and a split sense or a promoted `status` never
reaches it.

## How the tutor "remembers"

**It does not.** A GPT uses no saved memory and no previous conversations: every
session starts amnesiac.

Inside the lesson cycle this stopped being a problem, because Claude generates
every prompt from the live vault. **For this GPT it is still a problem**, because
its prompt is permanent: its learner values have to be refreshed by hand every 4-6
sessions from [[Lernprofil]].

That is the one place in the system where the old maintenance cost survives. It is
the price of having a conversation partner that is always there, with no lesson to
open first.

## The six silent failures

None of these produces an error. All of them degrade the system without saying so.
Each has an observable symptom, which is the only reason they get caught — the
twelve-row health check in [[Kommando - Commit]] exists for exactly these.

1. **Not refreshing this GPT's prompt.** It freezes at last month's level and never
   says so. *Symptom: it keeps introducing new words and never recycles.*
2. **Skipping the error cross-referencing.** The vault becomes an archive of things
   nobody reviews. *Symptom: `Schwachstellen` empty after five sessions.*
3. **Leaving examples blank.** *Symptom: `Ohne Beispiel` growing.*
4. **Piling up unused words.** *Symptom: `Nicht gesprochen` growing.*
5. **A note stops matching a filter it used to match** — a value written differently
   (`L 1`, `a11`, `A2.1`), or a field renamed on one side only. The view stops seeing
   the note **and does not warn**. An unknown field raises nothing: the column comes
   up blank and a sort on it silently does nothing. Two real cases so far —
   `level` → `cefr`, where four views kept filtering on the old name for three days,
   and the `anki` field, which was removed everywhere but had to be removed from the
   view too. *Symptom: one note missing from one view, which is why it is the hardest
   of the six to find.*
6. **Malformed frontmatter.** Not a wrong value — no value at all, because the YAML
   never parses. The usual cause is an edit across many files that removes a line
   and takes the newline before it instead of the one after, so whenever the target
   was the last field the closing `---` gets pulled up: `status: new---`. That is how
   54 notes broke on 2026-09-14. *Symptom: several views going empty at once* — and
   that is the tell. One note missing is number 5; everything missing is this.

The two are worth separating because the instinct they trigger is opposite. When a
view empties completely, the reflex is to suspect the view, and the view is almost
never the problem: a `.base` file that is broken shows an error, while notes that
are broken show nothing at all.
