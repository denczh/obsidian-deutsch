---
type: reference
updated: 2026-09-07
---

# Levels: the difficulty of words

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

The **`cefr`** field classifies **the difficulty of the word** on the international
scale, and decides which folder it lives in:

```
20 - Wortschatz/A11/   30 - Grammatik/A11/
```

It does not measure where the learner is. It measures how advanced the vocabulary
is. `sein` is `A11` after three years of study; `aufbewahren` is `B11` even if it
was learned on day one.

## Why this way, and not the previous way

There used to be a personal scale, `L1, L2…`, precisely so as not to depend on an
external table. The problem was not the scale: it was that **it said two things at
once** — when something was learned and how hard it was — and then it collided with
the lesson sequence, which says the first of those. Two fields with the same name is
what breaks views silently.

Split apart, each one does one job well:

| Field | What it means | Where it lives |
|---|---|---|
| **`cefr`** | difficulty of the word | **decides the folder** |
| **`lektion`** | which lesson it entered in | a field, **never a folder** |

And CEFR turns out to be good for this even though it was bad for the other thing:
it is a **closed set of twelve**, it is stable, and it describes the word rather
than the student. "Is this A21 or A22?" now has an answer, because it is a question
about `{TARGET}` and not about a person.

## The twelve tokens

`A11 A12 A21 A22 B11 B12 B21 B22 C11 C12 C21 C22`

One spelling, always. Never `A2.1`, never `a11`, never `A2`. The moment a folder
says `A21` and a note says `A2.1` is the moment
[[Nach Niveau.base|Nach Niveau]] splits in two without warning.

**Folders get created when a word lands in them**, not before. Five are populated
today: `A11`, `A12`, `A21`, `A22`, `B11`.

## Who assigns the `cefr`

Claude does, when the note is created in `/de lektüre`. And this has to be said:
**it is an approximate judgement.** There is no official word-by-word list, so two
reasonable people would disagree at the edges.

That is fine for what it is for — grouping, browsing, deciding what is early and
what is late — and not fine for anything that needs precision. **No critical view
filters on `cefr`**: [[Schwachstellen.base|Schwachstellen]] does not look at it,
and neither does the `/de studium` drill. If a word ever looks misclassified, drag
it to another folder and change the field; nothing breaks.

## What was lost by dropping the personal scale

**The promotion criterion.** There used to be a checkable definition of *I have
improved*: five consolidated structures and nothing with `error_count` above 2.
There is no equivalent now, because lessons measure activity, not ability.

What still measures mastery, item by item:

- `status: new → learning → known`, moved by the `ERRORS` and `OK` blocks.
- [[Schwachstellen.base|Schwachstellen]], which empties only when something reaches
  `known`.

That is enough to know what to study. It is not enough to know whether progress is
happening. If that is missed three months from now, the natural repair is a
criterion over `known` — *80% of A11 and A12 in `known`* — not resurrecting the
field.

## And the tutor's calibration

A model does not know what `L001` is, and the `cefr` of an individual word does not
tell it how hard to speak. That is still the CEFR pair in [[Lernprofil]]:
**production and comprehension.**

It lives only there, it is the input when a prompt is generated, and it enters no
vocabulary note. See [[Lektionen]] and [[Configuration]].

## History

| Date | What |
|---|---|
| 2026-07-31 | twelve CEFR folders, `level` = the learner's level |
| 2026-08-01 | personal scale `L1`, one folder, `level` = the learner's position |
| 2026-09-07 | `cefr` = difficulty of the word, `lektion` = when it entered |
