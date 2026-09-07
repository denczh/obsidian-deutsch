---
type: kommando
phase: 1
command: /de lektüre
updated: 2026-09-07
---

# `/de lektüre` — phase 1 of 5

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

Specification of the phase. **Claude reads this when the command runs**; the `de`
skill is only the router. To change how the phase behaves, edit this note, not the
skill. See [[Lektionen]].

## Gate

None — it is the first phase. But **a new lesson cannot open while the previous
one is unclosed**: if the latest note in `10 - Lektionen/` has an empty `closed`
field, say so and offer two ways out — continue that lesson, or abandon it
explicitly — before creating another.

## 1. Read the vault

Before asking anything:

- `10 - Lektionen/` → the latest lesson, its number and its state. The new one is
  `L{XXX}` where `XXX` = last + 1, always three digits.
- [[Lernprofil]] → the **production** and **comprehension** CEFR pair. It is the
  only thing that says how hard to write.
- `20 - Wortschatz/*/` and `30 - Grammatik/*/` → everything already there. The
  list of `term` values, so nothing gets introduced twice, and the spread by `cefr`.
- The weak items: `error_count > 0` and `status != "known"`. The story must
  **recycle** them on purpose.
- Themes already used: the `thema` field of previous lessons and the `theme` field
  of the vocabulary.

## 2. Ask for the theme

Offer **three themes** from [[Themenliste]], **skipping those of the last two
lessons**. If a repeated theme is requested deliberately, accept it: the story
then has to go to a different corner of the theme, and the new words have to be
genuinely new rather than synonyms of what is already there.

A free theme outside the list is also fine. If it looks recurrent, add it to
[[Themenliste]] first — a token invented on the fly is an orphan tag.

## 3. Write the story

**A narrative with characters who interact**, in two parts. Part 1 is this phase;
part 2 is written by `/de vorlesen` and has to be able to continue it.

Rules for the story:

- **150 to 250 words.** Long enough for the vocabulary to appear in context, short
  enough to read twice without effort.
- **Calibrated to the comprehension level** in [[Lernprofil]], not the production
  level. Understanding more than you can say is the point.
- **Named characters**, two or three, who talk to each other. Dialogue is what
  makes expressions sound like a language instead of a list.
- **Ends open.** Part 2 continues it: leave something unresolved.
- **Recycles at least three weak items**, without pointing at them.
- No glossary, no bold inside the text. It is a story, not a lesson.

After the `{TARGET}` story, **the `{KNOWN}` translation below it**, separated. The
order matters: read the `{TARGET}` twice before looking. A translation placed
alongside gets read instead.

## 4. The new vocabulary

**Between 10 and 15 items**, plus **1 or 2 grammar points**. They get drilled in
`/de studium`, so more fits than in a spoken session.

**No noun-only lists.** The spread to aim for:

- **verbs**, including separable ones and those that govern a case or preposition
- **adverbs**, above all of frequency, time and degree
- **adjectives**, in opposite pairs where that comes naturally
- **expressions and fixed phrases**, as a single item (`pos: phrase`)
- **connectors and prepositions**
- nouns, yes, but not half the list

**Never invent** a word, a gender or a form to fill a category. Unsure of a
gender: use a different word.

## 5. Create the notes

One note per item, from the matching template:

| `pos` | Template | Folder |
|---|---|---|
| `verb` | `V - Verb` | `20 - Wortschatz/<cefr>/` |
| anything else | `V - Wortschatz` | `20 - Wortschatz/<cefr>/` |
| grammar | `V - Grammatik` | `30 - Grammatik/<cefr>/` |

Fields to fill without exception:

- `cefr` — **the difficulty of the word**, one of the twelve tokens. Decides the
  folder. See [[Niveaus]].
- `lektion` — `L{XXX}`, the lesson being created.
- `source: lektuere` — this provenance means the word came from a story, not from
  a conversation or a list.
- `example` — **the sentence from the story where the word appears**, verbatim.
- `theme` — the theme token.
- `translation` — in `{KNOWN}`.
- `article` and `inflection` per [[E-Mail-Format]]. Verbs with their auxiliary;
  separables split.

**On `example`:** it used to be left empty so the learner would write the
sentence. Now the story writes it, and that is better: a real sentence, in
context, with the characters. Production has moved to `/de vorlesen` and
`/de gramatik`, which are spoken, and speaking beats typing a sentence into a
note. [[Ohne Beispiel.base|Ohne Beispiel]] stops being a backlog of homework and
becomes what its name says: a detector of notes made in a hurry.

In the body of each grammar note: the explanation in `{KNOWN}`, two or three
sentences, and two examples, **one of them from the story**.

## 6. Create the lesson note

`10 - Lektionen/L{XXX}.md` from `V - Lektion`, with:

- `thema`, `started` with today's date, `phase_1_lektuere: true`
- **story part 1** complete, in `{TARGET}`, under its heading
- the list of vocabulary and grammar introduced, with links
- `vocab_count` and `grammar_count`

Part 2 stays empty: phase 3 writes it.

## 7. Update the view

In `40 - Ansichten/Aktuelle Lektion.base`, change the filter to the new lesson. It
is the only thing that has to be edited by hand when a lesson changes.

## 8. Close the turn

Show in the chat: the story in `{TARGET}`, the translation below it, and a table
of the new vocabulary with `cefr` and translation. Say how many notes were created
and in which folders.

**Do not commit.** That is `/de commit`, and only once all four phases are done.
`obsidian-git` commits on its own anyway; the phase 5 commit is the marker that
the lesson closed.

## What this phase does not do

- It does not ask the learner to speak or produce anything. It is pure input, in
  silence.
- It generates no GPT. The first one is generated by `/de studium`.
- It emits no closing block: the notes are written directly, so there is nothing
  to parse.
