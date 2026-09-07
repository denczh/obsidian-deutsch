---
type: reference
updated: 2026-09-07
target_language: German
known_language: Spanish
learner: Pedro
command_prefix: /de
---

# Configuration

**This vault is one instance of a reusable system.** All the instruction notes are
written in English and use placeholders, so the same specifications work for any
pair of languages. This note is the only place where the pair is resolved.

Everything the learner produces — vocabulary notes, grammar explanations,
translations, lesson records — stays in the two real languages. Only the
*instructions* are in English, because instructions are read by whoever is
operating the system, and English is the safest common ground.

## Placeholders

| Placeholder | This vault | What it means |
|---|---|---|
| `{TARGET}` | **German** | the language being learned |
| `{KNOWN}` | **Spanish** | the language the learner already speaks |
| `{LEARNER}` | **Pedro** | the learner's name, used in generated prompts |

Read any instruction note with those substitutions. *"Write the story in
`{TARGET}`"* means German here, Spanish in a fork where someone is learning
Spanish.

## Calibration

The one thing a model cannot infer and cannot be told as a lesson number: **how
hard to speak.** It lives in [[Lernprofil]], not here, because it changes as the
learner changes:

- **production level** — what the learner is asked to produce
- **comprehension level** — what the learner is spoken to in

Both are CEFR codes and they are deliberately different. That gap is the
mechanism: understanding more than you can say is how production follows.

## What a fork has to change

Everything below is **this instance's convention**, not part of the system. A fork
renames it and nothing breaks, as long as it renames it consistently.

### Command prefix and phase names

The prefix is the target language's ISO code; the phase names are words in
`{TARGET}`, so the commands feel like the language being learned.

| Phase | This vault | Spanish-learning fork would use |
|---|---|---|
| 1 | `/de lektüre` | `/es lectura` |
| 2 | `/de studium` | `/es estudio` |
| 3 | `/de vorlesen` | `/es lectura en voz` |
| 4 | `/de gramatik` | `/es gramatica` |
| 5 | `/de commit` | `/es commit` |

The skill that routes them is named after the prefix.

### Folder names

Also in `{TARGET}`:

```
00 - Start        10 - Lektionen     10 - Sitzungen
20 - Wortschatz   30 - Grammatik     40 - Ansichten
50 - Ressourcen   90 - Vorlagen
```

**The numeric prefixes are the real structure**; the words are decoration. A fork
translates the words and keeps the numbers, and every path in every specification
still resolves by position.

### Spoken commands

The words the learner says to a GPT mid-session are in `{TARGET}`, because saying
them is itself practice. These are baked into the generated prompts, so a fork
replaces them there:

| Meaning | This vault |
|---|---|
| repeat | `noch einmal`, `wiederhole` |
| back one | `vorherige` |
| forward one | `nächste` |
| give the `{KNOWN}` meaning | `Spanisch` |
| ask me about the story | `frag` |
| in fragments | `Stück für Stück` |
| slower | `langsamer` |
| explain in `{KNOWN}` | `auf Spanisch` |
| what does … mean | `was bedeutet …` |
| be quiet | `warte` |
| finish and emit the block | `fertig` |

### Field keys stay in English

`type`, `term`, `article`, `inflection`, `pos`, `translation`, `cefr`, `lektion`,
`theme`, `example`, `source`, `status`, `error_count`, `last_error`, `anki`.

**These are never translated, in any fork.** Every view filters on them, and one
spelling everywhere beats a localised schema. The *values* of `translation` are in
`{KNOWN}`; the keys are not.

## What is NOT written in English

- **Vocabulary and grammar notes.** The `translation` field, the explanations, the
  example sentences: all in the two real languages. They are study material, not
  instructions.
- **Lesson notes and session notes.** Records of what happened, including stories
  in `{TARGET}` and error logs.
- **The learner's own preferences** in [[Lernprofil]], where they are statements
  about a person rather than instructions to a machine.

## Where to start reading

1. [[Lektionen]] — the five-phase cycle. The system.
2. [[Kommando - Lektüre]] and the four other `Kommando` notes — one per phase.
3. [[E-Mail-Format]] — the closing block, which is the interface between every
   voice session and this vault.
4. [[Niveaus]] — what `cefr` means and who assigns it.
