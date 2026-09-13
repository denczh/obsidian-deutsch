---
type: reference
updated: 2026-09-13
---

# The four GPTs

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

**A GPT is a container I create by hand; only its contents are generated.**

| | What it is | Who makes it | How often |
|---|---|---|---|
| **The GPT** | name, link, phone shortcut, capabilities, the mail Action, chat history | **me, on the web** | **once, ever** |
| **Its Instructions** | the 6-8k prompt: word list, story, sentences | Claude, from the vault | **every lesson** |

Nothing about the container is dynamic, and it cannot be: **there is no API for
creating GPTs.** The web editor is the only way. So all four have to exist before
the first lesson — otherwise phase 2 arrives with a generated prompt and nowhere to
paste it.

| GPT | Phase | Instructions | Emits | Spec |
|---|---|---|---|---|
| `Deutsch - Studium` | 2 | generated every lesson | `OK` + `ERRORS` | [[Kommando - Studium]] |
| `Deutsch - Vorlesen` | 3 | generated every lesson | `ERRORS` | [[Kommando - Vorlesen]] |
| `Deutsch - Gramatik` | 4 | generated every lesson | `OK` + `ERRORS` | [[Kommando - Gramatik]] |
| `Deutsch - Sprechen` | outside the cycle | **written once, refreshed by hand** | all four blocks | [[Modus - Sprechen]] |

**Phases 1 and 5 have no GPT.** `/de lektüre` and `/de commit` are Claude, at the
desk, with the vault open. That is why they can read and write notes and the other
three cannot.

## Configuration, identical for all four

| Field | Setting |
|---|---|
| Name | as above. Naming them after the phase keeps the home screen legible. |
| Description | short. Only used to find it. |
| Instructions | the whole prompt. This is the entire GPT. |
| **Knowledge** | **empty.** Voice mode cannot read it, and a file it cannot read is worse than no file at all. |
| Capabilities | image generation **off**, data analysis **off**. Web search off keeps it grounded. |
| Actions | the mail webhook. Text-only: it never fires during a voice conversation. |
| Conversation starters | two at most. In voice you never see them. |
| Visibility | *Only me* |

Then **open each link on the phone once and add it to the home screen.** Two taps
to start a session instead of navigating menus one-handed — that is the difference
between doing a phase and not bothering.

## Overwrite, never recreate

Every lesson, Claude hands over a prompt that **replaces** the Instructions of an
existing GPT.

Deleting and recreating costs three things: the home-screen shortcut, the chat
history, and about fifteen minutes of clicking per lesson. The chat history is the
one that bites — **the closing block of a session lives in that chat until it is
processed**, so deleting a GPT before processing its last session destroys data
that exists nowhere else.

## The fourth one is different in kind

`Deutsch - Sprechen` is not a lesson phase. No gate, no fixed material, no
sequence — free conversation while walking, which is the only part of the system
that is genuinely a conversation.

Its prompt is **permanent rather than generated**, so it is the one prompt that
goes stale: its learner values have to be refreshed by hand from [[Lernprofil]]
every four to six sessions. That is the last surviving piece of the maintenance
cost the lesson cycle removed everywhere else, and it is the price of having a
partner that is always there with no lesson to open first.

## When something goes wrong

**The GPT ignores a rule.** Note it in the lesson note under *Notes*, then tighten
that one line in the phase specification — not in the prompt by hand, because the
prompt gets regenerated next lesson and the edit would vanish. The specifications
are the `Kommando - *` notes.

**The prompt does not fit.** The cap is 8000 characters. Shorten the material — the
word list, the story, the sentences — **never the rules**.

**A GPT behaves strangely and it is not obvious why.** The exact prompt that was
pasted is stored in the lesson note under `## Prompt - *`. It is not reproducible:
the drill order is shuffled randomly and the stories and sentences are generated
fresh, so that copy is the only record.
