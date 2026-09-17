---
type: dashboard
updated: 2026-09-17
---

# Dashboard

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

**Lesson in progress:** L002 → [[Lektionen]] · [[Lernprofil]]

> **[[Lektionen]]** — the five-phase cycle and its `/de` commands. That is the
> system. [[Workflow]] describes free conversation, which is still alive but
> outside the cycle.

## Views

| View | For what |
|---|---|
| [[Aktuelle Lektion.base\|Aktuelle Lektion]] | The vocabulary of the lesson in progress. |
| [[Nach Niveau.base\|Nach Niveau]] | Everything grouped by CEFR difficulty of the word. |
| [[Nach Thema.base\|Nach Thema]] | Replaces a per-theme folder completely. |
| [[Ohne Beispiel.base\|Ohne Beispiel]] | Notes made in a hurry and never finished. |

`Schwachstellen` and `Nicht gesprochen` were deleted on 2026-09-17 with the fields
they filtered on → [[Lektionen]]. There is no revision queue any more.

## The cycle

```
/de lektüre    Claude writes the story and creates the notes    (at the desk)
/de studium    vocabulary GPT, three modes                      (phone)
/de vorlesen   GPT reads part 2 and asks about it               (phone)
/de gramatik   spoken translation GPT                           (phone)
/de commit     Claude checks, commits and archives              (at the desk)

/de fertig     marks the phone phase just finished as done      (anywhere)
```

Each command checks that the previous one has been passed, and "passed" means
`/de fertig` was said. Detail in [[Lektionen]] and [[Kommando - Fertig]].

## Reference

- [[Configuration]] — **the language pair and what a fork changes**
- [[Lektionen]] — the lesson cycle and the five commands
- [[GPTs]] — the four GPTs, their configuration and the overwrite rule
- [[Kommando - Fertig]] — how a phase gets marked done, and what that replaced
- [[Niveaus]] — the twelve CEFR tokens and who assigns them
- [[Bedeutungen]] — **one note, one sense**, and the grammatical category
- [[Themenliste]] — the themes, as tokens
- [[Workflow]] — free conversation, outside the cycle

## Outside the cycle

| | Prompt | Where |
|---|---|---|
| [[Modus - Sprechen\|Sprechen]] | free conversation while walking | permanent GPT |

It is the only survivor of the earlier "modes" architecture: it is none of the five
phases and it is the only thing that is genuinely conversation.

## Structure

- `10 - Lektionen/` one note per lesson: story, phases, prompts
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
- Every 4-6 free-conversation sessions: refresh the *Learner values* of
  [[Modus - Sprechen]] by hand. It is the only copy left that can go stale.
- Weekly: re-read the `## Notas` of the open lesson. Since 2026-09-17 that is the
  whole error log, and it is next week's curriculum.

The failures that raise no error are listed at the end of [[Workflow]], and
`/de commit` checks for them.
