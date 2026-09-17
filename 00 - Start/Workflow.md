---
type: reference
updated: 2026-09-17
---

# Free conversation: the walking session

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

> **This note describes free conversation only**, which is still alive but is no
> longer the main system. The system is the five-phase cycle with the `/de`
> commands → **[[Lektionen]]**. What is still current here: the session shape, the
> offline study views, and the silent failures at the end.

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
  reviewed every 5-6 exchanges. **It notices every mistake, including the ones it
  does not mention**, and brings back the ones that repeat.
- It does not agree to be nice: no "Genau" or "Super" as an opener, and no
  confirming half-finished sentences.

## What can be said at any moment

`warte` (be quiet) · `protokoll fertig` (only answer after `fertig`) ·
`wiederhole` · `langsamer` · `auf Spanisch` · `was bedeutet …` ·
`anderes Thema` · `einfacher` / `schwieriger`

And for the phone, not the prompt: **Voice Isolation** in Control Centre, and
headphones. Being cut off mid-sentence is the app's turn detection, not the tutor.

## From chat to vault

Say it is finished. The tutor says aloud how many words and rules came up, **the
three or four most worth keeping, slowly, one by one**, and one encouraging sentence
if it was earned. Then it stops, and writes nothing at all.

**That spoken handover is the whole interface.** Until 2026-09-17 the session ended
with a five-section block in the chat and a mail Action that sent it; both are gone
→ [[Lektionen]]. What reaches the vault now is whatever gets repeated back to Claude
afterwards — a word, a rule, a sentence that would not come — and Claude writes the
notes from that, with `lektion: "-"` because a walk belongs to no lesson.

Everything not said out loud is lost. A conversation was always the part of the
system least suited to being logged, and this is the price of it costing nothing to
have.

## Offline study

Each view answers a different question. They do not all need looking at:

| When | View | What to do |
|---|---|---|
| While writing notes | [[Ohne Beispiel.base\|Ohne Beispiel]] | write the sentences left blank |
| Starting a topic | [[Nach Niveau.base\|Nach Niveau]] | what is there, by difficulty |

**Two of the four, on purpose.** [[Nach Thema.base|Nach Thema]] is for preparing a
story and [[Aktuelle Lektion.base|Aktuelle Lektion]] is for following a lesson in
progress: both belong to the cycle, not to studying alone between sessions. The full
list and what each one filters on is in [[Ansichten - Referenz]].

**Ohne Beispiel** is the only work list left: you open it, write the sentence, and the
row disappears on its own because the note changed. **Nach Niveau** never empties; it
is read, not worked, and what it tells you is the shape of what you have, not a list
of chores.

**Two views were deleted on 2026-09-17**, with the fields they filtered on.
`Schwachstellen` was the revision queue — what gets failed — and `Nicht gesprochen`
listed words never produced. Both read `status`, `error_count` and `last_error`, which
only the closing block ever wrote → [[Lektionen]]. Nothing replaces them: **there is no
revision queue any more.** What gets studied between sessions is whichever lesson note
you open.

**On spaced repetition.** There is none, deliberately. The `anki` field and its
export view were inherited from the source document and removed on 2026-09-14: a
field nobody writes and nobody reads is worse than no field, because it looks like
a feature, and the view was a queue silently filling up towards an export step that
did not exist.

What the system has instead is **the 30% of older material in every `/de studium`**,
drawn at random. That is all. It used to be more — that 30% was ordered by what had
been failed, and `Schwachstellen` was a standing queue — and both went on 2026-09-17
with the fields behind them.

So, stated plainly: retention now rests on a random third of a drill list plus
whatever the learner happens to notice. If forgetting becomes visible after ten
lessons, the fix already installed is the `obsidian-spaced-repetition` plugin, which
works **on the notes themselves** and keeps its own scheduling data — and which,
unlike the fields just removed, something actually writes. Not Anki: exporting creates
a second copy of the vocabulary that starts drifting the same day.

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

## The silent failures

None of these produces an error. All of them degrade the system without saying so.
Each has an observable symptom, which is the only reason they get caught — the health
check in [[Kommando - Commit]] exists for exactly these.

1. **Not refreshing this GPT's prompt.** It freezes at last month's level and never
   says so. *Symptom: it keeps introducing new words and never recycles.* Since
   2026-09-17 this is the **only** hand-copied thing left in the system, and so the
   only one that can go stale this way.
2. **Confirming a phase that did not happen.** `/de fertig` writes `true` on the
   strength of a sentence, and nothing checks it. *Symptom: none — and that is the
   point. It is the successor to the old number 2 and the price of the change of
   2026-09-17* → [[Lektionen]].
3. **Leaving examples blank.** *Symptom: `Ohne Beispiel` growing.*
4. **Learning nothing from a session.** A walk or a drill produced something worth
   keeping and nobody said it out loud afterwards. *Symptom: lesson notes whose
   `## Notas` stay empty lesson after lesson.*
5. **A note stops matching a filter it used to match** — a value written differently
   (`L 1`, `a11`, `A2.1`), or a field renamed on one side only. The view stops seeing
   the note **and does not warn**. An unknown field raises nothing: the column comes
   up blank and a sort on it silently does nothing. Two real cases so far —
   `level` → `cefr`, where four views kept filtering on the old name for three days,
   and the `anki` field, which was removed everywhere but had to be removed from the
   view too. The removal of `status`, `error_count` and `last_error` on 2026-09-17 is
   the third, and the reason two views were deleted outright rather than edited. *Symptom: one note missing from one view, which is why it is the hardest
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
