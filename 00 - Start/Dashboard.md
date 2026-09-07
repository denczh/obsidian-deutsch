---
type: dashboard
updated: 2026-09-07
---

# Dashboard

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

**Lesson in progress:** L001 → [[Lektionen]] · [[Lernprofil]]

> **[[Lektionen]]** — the five-phase cycle and its `/de` commands. That is the
> system. [[Workflow]] describes free conversation, which is still alive but
> outside the cycle.

## Views

| View | For what |
|---|---|
| [[Schwachstellen.base\|Schwachstellen]] | The revision queue. The most useful view in the system. |
| [[Aktuelle Lektion.base\|Aktuelle Lektion]] | The vocabulary of the lesson in progress. |
| [[Nach Niveau.base\|Nach Niveau]] | Everything grouped by CEFR difficulty of the word. |
| [[Nach Thema.base\|Nach Thema]] | Replaces a per-theme folder completely. |
| [[Nicht in Anki.base\|Nicht in Anki]] | Export queue for spaced repetition. |
| [[Ohne Beispiel.base\|Ohne Beispiel]] | Notes made in a hurry and never finished. |
| [[Nicht gesprochen.base\|Nicht gesprochen]] | Words never produced: given by a tutor, or added by hand. |

## The cycle

```
/de lektüre    Claude writes the story and creates the notes    (at the desk)
/de studium    vocabulary GPT, three modes                      (phone)
/de vorlesen   GPT reads part 2 and asks about it               (phone)
/de gramatik   spoken translation GPT                           (phone)
/de commit     Claude processes, commits and archives           (at the desk)
```

Each command checks that the previous one has been passed, and "passed" means its
closing block exists. Detail in [[Lektionen]].

## Reference

- [[Configuration]] — **the language pair and what a fork changes**
- [[Lektionen]] — the lesson cycle and the five commands
- [[E-Mail-Format]] — the closing block format and the error types
- [[Niveaus]] — the twelve CEFR tokens and who assigns them
- [[Themenliste]] — the themes, as tokens
- [[Verarbeitung]] — processing a free-conversation block
- [[Workflow]] — free conversation, outside the cycle

## Outside the cycle

| | Prompt | Where |
|---|---|---|
| [[Modus - Sprechen\|Sprechen]] | free conversation while walking | permanent GPT |

It is the only survivor of the earlier "modes" architecture: it is none of the five
phases and it is the only thing that is genuinely conversation.

## Structure

- `10 - Lektionen/` one note per lesson: story, phases, raw blocks, prompts
- `10 - Sitzungen/` one note per free-conversation session
- `20 - Wortschatz/<cefr>/` one note per word, folder = CEFR difficulty
- `30 - Grammatik/<cefr>/` one note per rule, short label as title
- `40 - Ansichten/` the views (Bases)
- `50 - Ressourcen/` everything that is not atomic
- `90 - Vorlagen/` five templates: lesson, session, grammar, vocabulary, verbs

**The one rule:** every note has exactly one home. Type decides the folder, `cefr`
is a subfolder of type, and theme and lesson are fields — never folders.

## Maintenance

- Nothing to keep in sync by hand any more: the prompts are generated from the
  vault every lesson. That was the recurring cost of the system and it is gone.
- Weekly: re-read the error log. That is next week's curriculum.

The five failures that raise no error are listed at the end of [[Workflow]], and
`/de commit` checks for them.
