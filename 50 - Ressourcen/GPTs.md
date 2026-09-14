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

| GPT                  | Phase             | Instructions                        | Emits           | Spec                    |
| -------------------- | ----------------- | ----------------------------------- | --------------- | ----------------------- |
| `Deutsch - Studium`  | 2                 | generated every lesson              | `OK` + `ERRORS` | [[Kommando - Studium]]  |
| `Deutsch - Vorlesen` | 3                 | generated every lesson              | `ERRORS`        | [[Kommando - Vorlesen]] |
| `Deutsch - Gramatik` | 4                 | generated every lesson              | `OK` + `ERRORS` | [[Kommando - Gramatik]] |
| `Deutsch - Sprechen` | outside the cycle | **written once, refreshed by hand** | all four blocks | [[Modus - Sprechen]]    |

**Phases 1 and 5 have no GPT.** `/de lektüre` and `/de commit` are Claude, at the
desk, with the vault open. That is why they can read and write notes and the other
three cannot.

## Configuration, identical for all four

| Field | Setting |
|---|---|
| Name | as above. Naming them after the phase keeps the home screen legible. |
| Description | short. It changes nothing about behaviour — it is a label for telling four similar names apart on a phone. Texts below. |
| Instructions | the whole prompt. This is the entire GPT. |
| **Knowledge** | **empty.** Voice mode cannot read it, and a file it cannot read is worse than no file at all. |
| Capabilities | image generation **off**, data analysis **off**. Web search off keeps it grounded. |
| Actions | the mail webhook. Text-only: it never fires during a voice conversation. |
| Conversation starters | see below. **In voice you never see them**, so they only matter on the desktop. |
| Visibility | *Only me* |

### The Description texts

Each one says what the GPT does and **which command fills it**, because that is
what gets forgotten.

| GPT | Description |
|---|---|
| `Deutsch - Sprechen` | Free conversation in German while walking. Outside the lesson cycle: nothing gets pasted into it, its prompt is permanent. |
| `Deutsch - Studium` | Phase 2: drilling the lesson's vocabulary, three modes. Paste the /de studium prompt first. |
| `Deutsch - Vorlesen` | Phase 3: reads me part 2 of the story and asks me about it. Paste the /de vorlesen prompt first. |
| `Deutsch - Gramatik` | Phase 4: I translate sentences into German out loud, corrected strictly. Paste the /de gramatik prompt first. |

The Sprechen one says outright that nothing gets pasted into it, because it is the
exception and it is where the confusion will land three weeks from now.

These are UI labels, not material, so they follow the same rule as the rest of the
machinery: English, like every instruction note → [[Configuration]]. A fork
translates them or not; nothing depends on them.

### Conversation starters

Only worth filling where they map onto a command the prompt already knows.
Otherwise they are decoration.

| GPT | Starters | Why |
|---|---|---|
| `Deutsch - Studium` | `Sequenz` · `Frage auf Deutsch` · `Frage auf Spanisch` | they replace the turn-1 menu outright. The only case where the buttons genuinely do something. |
| `Deutsch - Vorlesen` | `Lies vor` · `frag` | the first begins; the second jumps straight to the questions on a second run, without re-reading. |
| `Deutsch - Gramatik` | `Anfangen` | it starts at sentence 1 anyway; one button is enough. |
| `Deutsch - Sprechen` | `Neues Thema` · `Wiederhole meine Fehler` | the second skips turn 1 and goes straight to weak items. |

`noch einmal` is a bad starter anywhere: it only means anything *after* something
has been said.

Because Studium's starters are the three mode names, its prompt carries an extra
line — *if his first message already names a mode, skip the menu and start it* —
so the buttons are not swallowed by the menu they are meant to replace.

Then **open each link on the phone once and add it to the home screen.** Two taps
to start a session instead of navigating menus one-handed — that is the difference
between doing a phase and not bothering.

## Creating them before there is any content

The containers can and should exist before the first lesson. The three phase GPTs
get a **placeholder prompt that refuses to run**: an empty GPT that improvises
would invent vocabulary, and invented vocabulary ends up in the vault as if it had
been taught.

Paste one of these as the Instructions, changing only the command name:

```
You are a placeholder. This GPT has no material yet.

Whatever Pedro says, in any language, reply with exactly this and nothing else:

"Este GPT todavía no tiene contenido. Pídele a Claude /de studium y pega el prompt que te dé encima de estas Instructions."

Then stop. Do not teach, do not drill, do not ask questions, do not start a conversation in German, and above all do not invent a word list. You have no material, and anything you made up would end up in his vault as if it had been a lesson.
```

Same text for `Vorlesen` with `/de vorlesen`, and for `Gramatik` with
`/de gramatik`.

`Deutsch - Sprechen` needs no placeholder: its prompt is finished and permanent →
[[Modus - Sprechen]]. Copy it from the fenced block in that note, which is the
single source of truth for it.

While in the editor, attach the **mail Action to all four**. Doing it now saves
opening each one again later.

## The Action

**One schema, identical in all four GPTs.** Same webhook, same operation, same
fields. The only thing that differs between phases is the email subject, and that
comes from the prompt, not from the schema.

Authentication: **None**. Replace the server and path with the Make hook URL.

```yaml
openapi: 3.1.0
info:
  title: Send closing block
  description: Emails the closing block of a session to the learner.
  version: 1.1.0
servers:
  - url: https://hook.eu1.make.com
paths:
  /YOUR-HOOK-ID:
    post:
      operationId: sendClosingBlock
      summary: Emails the closing block of the session.
      description: >
        Call only when closing a session, and only after the closing block has
        already been written in the chat. Never call it during a voice
        conversation: Actions do not run in voice mode. Send the block exactly as
        written, with no commentary before or after.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [subject, body]
              properties:
                subject:
                  type: string
                  description: >
                    "Deutsch YYYY-MM-DD" followed by the phase name when there is
                    one: Studium, Vorlesen or Gramatik. Free conversation has no
                    suffix.
                body:
                  type: string
                  description: >
                    The closing block verbatim, in plain text, starting with
                    "=== SESSION ===" and ending with "=== END ===". Every header
                    the mode uses is present even when its section is empty. No
                    markdown, no bold, no commentary.
      responses:
        "200":
          description: Sent
          content:
            text/plain:
              schema:
                type: string
```

**The `description` fields are not documentation.** The model reads them and they
change its behaviour: when to call the Action, and what the body is. The first
version of this schema still described the pre-August format — sections that had
not existed for a month — which is exactly the kind of thing that makes a model
paraphrase instead of copying.

> **The webhook URL is a credential.** There is no authentication on it: anyone
> holding it can make Make send mail on your behalf. Keep it out of chats, tickets
> and screenshots, and regenerate the hook if it leaks.

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
