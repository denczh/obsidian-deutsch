---
type: reference
updated: 2026-09-07
---

# The closing block format

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

The interface between every voice session and this vault, and the
highest-leverage text in the system. Flat, delimited, no prose, no markdown:
designed so a script can parse it and so nothing breaks if it passes through a
mail client.

## Sprechen — free conversation, outside the cycle

```
=== SESSION ===
date: YYYY-MM-DD
lektion: "-"
mode: sprechen
themes: reisen, gesundheit

=== VOCAB ===
term | article | inflection | pos | translation
Bahnsteig | der | -e | noun | andén
umsteigen | - | steigt um, stieg um, ist umgestiegen | verb | hacer transbordo

=== EXTRA ===
term | article | inflection | pos | translation
verpassen | - | verpasst, verpasste, hat verpasst | verb | perder (un tren)
pünktlich | - | - | adj | puntual
meistens | - | - | adv | la mayoría de las veces

=== GRAMMAR ===
Perfekt mit sein | verbs
Wechselpräpositionen mit Akkusativ | prepositions

=== ERRORS ===
what he said | correction | type
ich bin gegangen zum Bahnhof | ich bin zum Bahnhof gegangen | word-order

=== END ===
```

## Studium and Gramatik — the block with `OK`

Every phase emits the same format, but only the blocks that belong to it. Drilling
introduces no vocabulary, so there is no `VOCAB` or `EXTRA`; instead there is `OK`,
which only `studium` and `gramatik` produce. See [[Lektionen]].

```
=== SESSION ===
date: YYYY-MM-DD
lektion: L001
mode: studium
themes: -

=== OK ===
item | type
Possessivartikel meine | grammar
warten | vocab

=== ERRORS ===
what he said | correction | type
ich helfe dich | ich helfe dir | case

=== END ===
```

**`OK` is the only exit from the revision queue.** An item in `learning` that
appears there becomes `known` and leaves
[[Schwachstellen.base|Schwachstellen]]; one in `new` becomes `learning` and leaves
[[Nicht gesprochen.base|Nicht gesprochen]]. `error_count` is never touched: it is
the historical record of how much trouble something gave.

That is why the drill prompts say three separate times that **a hint disqualifies
it**. That block writes promotions into the vault; a generous model would mark as
known what the learner cannot do.

**Vorlesen** emits `SESSION` and `ERRORS` only — no `VOCAB`, no `EXTRA`, no `OK`.
Nothing new enters and nothing gets promoted, because an answer about a story
cannot be attributed to an individual note.

## The two header lines

**`mode:`** is what makes it possible to know later where each error came from.
Values: `sprechen`, `studium`, `vorlesen`, `gramatik`. If `Schwachstellen` ever
fills with things that only get failed in writing, that is information, not noise.

**`lektion:`** says which lesson the session belongs to, or `-` for free
conversation outside the cycle. It replaced the old `level:` line. See
[[Lektionen]].

## The error types

The `type` field in `ERRORS` was free text, which means that by session ten there
would be `word-order`, `wordorder` and `Wortstellung` living side by side in a
field meant to be filtered. Closed vocabulary:

| `type` | What it is |
|---|---|
| `article` | wrong or missing article |
| `gender` | noun gender |
| `case` | wrong case after a verb or preposition |
| `agreement` | possessive, adjective or number agreement |
| `word-order` | verb position, order in a subordinate clause |
| `verb-form` | conjugation, auxiliary, participle |
| `preposition` | wrong preposition |
| `vocabulary` | wrong or invented word |
| `pronunciation` | pronunciation |
| `comprehension` | **did not understand.** Only `vorlesen` produces this |

`comprehension` is a different kind of thing from the rest, and that is exactly why
it earns its own label: *I did not understand it* and *I said it wrong* are
different problems with different fixes. If `Schwachstellen` fills with
`comprehension`, the diagnosis is that the input is too fast, not that vocabulary
is missing.

A fork learning a language with written accents should add `accents`: in Spanish,
`esta` and `está` are different words and a missing accent is a wrong answer, not a
typo. Extend the list when something does not fit — but add it here first.

## The two vocabulary blocks

**`VOCAB` is what happened in the conversation.** Words the tutor introduced aloud,
words the learner produced, words the learner asked about. Nothing else.

**`EXTRA` is teaching judgement.** Words that were not said but belong to the
lesson: the verb that goes with those nouns, the adverb that would have made the
sentence natural, the opposite of an adjective that came up. They get studied
offline from the notes, so they do not need to have been said — they need to be
worth learning next.

The separation is not bureaucracy. Three weeks later, *what the learner managed to
say* and *what the learner was given* are different facts: the first measures
production, the second measures homework. Together they measure nothing.

An item appears in one block or the other, never both. `EXTRA` has the same five
columns and no size limit; the 6-10 ceiling applies only to what gets said aloud.

In the vault they are told apart by `source`: `voice-session` for VOCAB,
`tutor-extra` for EXTRA, `lektuere` for words that came from a lesson story,
`manual` for hand-added ones. [[Nicht gesprochen.base|Nicht gesprochen]] lists
everything that has never been produced, whatever its source.

## Categories and inflection

`pos` is one of: `noun`, `verb`, `adj`, `adv`, `prep`, `conj`, `pron`, `num`,
`phrase`.

**No noun-only lists.** They are the easiest thing to list and the least useful
thing to have. `inflection` is filled according to the category:

| Category | What goes in `inflection` |
|---|---|
| `noun` | the plural: `-e`, `-en`, `Häuser`, or `-` if there is none |
| `verb` | irregular third person, Präteritum, and Perfekt **with its auxiliary**: `fährt, fuhr, ist gefahren`. Separables split: `räumt auf, räumte auf, hat aufgeräumt` |
| `adj` | comparative and superlative only if irregular: `besser, am besten` |
| everything else | `-` |

`article` is `-` for anything that is not a noun. The term is bare: `Bahnsteig`,
never `der Bahnsteig`.

## Rules, and they have to be stated as rules

- Plain text. No bold, no markdown tables, no bullets, no numbering.
- The `|` character never appears inside a field.
- A field that does not apply is a single hyphen.
- One line per item. No blank lines inside a block.
- Every header the mode uses, always present, even when the block is empty. Five in
  `sprechen`, four in `vorlesen`, three in `studium` and `gramatik`.
- The `mode:` line always, with the exact token.
- Field names without accents or special characters even when the content has them.
  That keeps the parse keys intact whatever the mail client does with encoding.

## Why the ERRORS block is not optional

Vocabulary and grammar make the system grow. Mistakes make it improve. Without an
error log you accumulate notes you never revisit and the recurring-mistakes section
of the profile stays empty forever. This block is what feeds
[[Schwachstellen.base|Schwachstellen]], and Schwachstellen is what actually gets
studied.

## One field worth reconsidering

The list is deliberately bare: enrichment is a separate, deliberate act. The
exception to keep in mind — translation and inflection can be looked up later; the
sentence being spoken when a word was got wrong cannot be reconstructed. If it ever
becomes unclear why a word is on the list, add an optional context field and accept
a slightly longer block.
