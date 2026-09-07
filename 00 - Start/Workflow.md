---
type: reference
updated: 2026-09-07
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
| Whenever | [[Nicht in Anki.base\|Nicht in Anki]] | export to spaced repetition |

For drilling: export `term` + `translation` to CSV and flip the `anki` flag, or use
the spaced-repetition plugin and stay in the vault. A chat is a poor drilling tool
and a good conversation partner; let each do its own job.

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

## The five silent failures

None of these produces an error. All of them degrade the system without saying so.
Each has an observable symptom, which is the only reason they get caught —
`/de commit` checks for all five.

1. **Not refreshing this GPT's prompt.** It freezes at last month's level and never
   says so. *Symptom: it keeps introducing new words and never recycles.*
2. **Skipping the error cross-referencing.** The vault becomes an archive of things
   nobody reviews. *Symptom: `Schwachstellen` empty after five sessions.*
3. **Leaving examples blank.** *Symptom: `Ohne Beispiel` growing.*
4. **Piling up unused words.** *Symptom: `Nicht gesprochen` growing.*
5. **Writing a different token or key** — `L 1`, `a11`, renaming a frontmatter
   field. The view stops seeing the note **and does not warn**. This is the one that
   is genuinely hard to find later.
