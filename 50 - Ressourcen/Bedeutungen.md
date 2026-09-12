---
type: reference
updated: 2026-09-12
---

# One note, one sense

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

Two rules about words, and the second one has consequences everywhere.

## 1. The grammatical category is always explicit

Every vocabulary note carries **`pos`**, and it is one of nine closed tokens:

`noun` · `verb` · `adj` · `adv` · `prep` · `conj` · `pron` · `num` · `phrase`

It is not optional and it is not inferred. It appears in three places:

- **the `pos` field**, which is what the views filter and sort on
- **the opening line of the note body**, so it is visible without opening the
  Properties panel:
  `**der Bahnsteig** · *noun* · Plural: *-e* · "andén"`
- **the closing block** from every voice session, as its fourth column

**`pos` belongs to the sense, not to the word.** The same word can be a noun in one
meaning and a verb in another, and each meaning is its own note with its own `pos`.

## 2. One note = one sense

A note describes **one meaning**. If a word has two unrelated meanings, it gets two
notes.

This is not tidiness. Each sense has its own difficulty, its own lesson, its own
error count and its own mastery:

```yaml
# 20 - Wortschatz/A12/Decke (techo).md
term: Decke
sense: techo
pos: noun
article: die
translation: "techo"
cefr: A12
lektion: L001
status: learning
error_count: 2
```

```yaml
# 20 - Wortschatz/A21/Decke (manta).md
term: Decke
sense: manta
pos: noun
article: die
translation: "manta"
cefr: A21
lektion: L004
status: new
error_count: 0
```

Same word, two folders, two lessons, two independent histories. Merged into one
note, all of that collapses: you could not record that you have mastered *ceiling*
and never once produced *blanket*.

### The fields that make it work

| Field | Role |
|---|---|
| **`term`** | the bare word, **identical across all its senses**. This is what groups them: a search for `term: Decke` finds every sense. |
| **`sense`** | a short label in `{KNOWN}` naming **this** meaning. `-` while the word has only one sense in the vault. |
| the **title** | the bare term when there is one sense; `Term (sense)` once there are two. |

**An absent `sense` means the same as `sense: "-"`**: the only sense in the vault.
That is the same convention as `article: "-"` for a non-noun, and it is why the
notes written before this rule existed did not all have to be edited. The field
becomes **mandatory the moment a second sense appears** — at that point both notes
carry one, because a disambiguator that only one of a pair has is not a
disambiguator.

`term` stays bare on purpose. It is what makes `[[Bahnsteig]]` work inside an
example sentence, and that link is how the vault accumulates a record of every
sentence where a word has been used.

### Where the line is

The hard part is not the rule, it is the boundary. Two translations do **not** mean
two senses.

**One note** — the translations are synonyms of the same meaning. `{TARGET}` has one
word, `{KNOWN}` happens to have two:

| Word | `translation` | Why one |
|---|---|---|
| `Bild` | `cuadro, imagen` | one thing: a visual representation |
| `Kissen` | `almohada, cojín` | one object, two Spanish words for it |
| `sein` | `ser, estar` | one German verb; Spanish splits it, German does not |
| `hinzufügen` | `añadir, agregar` | plain synonyms |

**Two notes** — the meanings are unrelated, and a native speaker would call them
different words that happen to be spelled alike:

| Word | Sense A | Sense B |
|---|---|---|
| `Decke` | techo | manta |
| `heißen` | llamarse | significar |
| `Schloss` | castillo | cerradura |

**The test:** could you swap one translation for the other in a sentence and have it
still make sense? `cuadro`/`imagen`, usually yes → one note. `techo`/`manta`, never
→ two notes.

**Same meaning with different grammar is still one note.** `hängen` is intransitive
and strong when it describes a state, transitive and weak when it describes the
action — but it means *hang* either way. One note, which documents both. The same
goes for a verb that is optionally reflexive, like `entspannen`.

**A fixed construction is its own item, not a sense of its verb.** `es gibt` is not
a meaning of `geben`; it is an invariable phrase with `pos: phrase` and its own
note. If it behaves as a unit, it is a unit.

### When a second sense arrives

The common case is a word that lived happily with one sense for months and then
turns up in a story meaning something else. The procedure, in order:

1. **Rename the existing note** to `Term (sense A)` and fill its `sense` field.
2. **Update every link to it.** Renaming inside Obsidian does this automatically;
   an agent editing files has to do it by hand. A stale `[[Decke]]` becomes a
   broken link, and broken links are the one thing `/de commit` checks for.
3. **Create the new note** as `Term (sense B)`, in the folder its own `cefr`
   dictates — which is often not the same folder as sense A.
4. **Cross-link them** under `## Verwandt`, both ways, and say in one clause how to
   tell them apart. That clause is the useful part of the note.

Renaming is deliberately the cost of the *second* sense, not the first. Titling
every note `Bahnsteig (andén)` from the start would be uglier every day to avoid a
rename that happens rarely.

## Who applies this

**The agent, at note-creation time**, because only the agent can see what already
exists in the vault. A voice GPT cannot: it has never read the vault, and it never
will.

That is why the closing block has **no sense column**. The GPT reports
`Decke | die | -n | noun | manta`; the agent looks up `term: Decke`, finds a note
that says `techo`, recognises the collision, and runs the procedure above. Asking
the GPT to disambiguate would mean asking it about a vault it cannot see — the
failure this whole system is built to avoid.

See [[Kommando - Lektüre]] for the create step and [[E-Mail-Format]] for the block.

## Done: the three inherited cases

Three words mixed senses from before this rule existed. All three were split on
2026-09-12, and they are the worked example of the procedure:

| Word | Was | Now |
|---|---|---|
| `Decke` | one note, `techo (interior)`, *manta* only mentioned in the body | [[Decke (techo)]] `A12` + [[Decke (manta)]] `A21` |
| `heißen` | one note, `llamarse; significar` | [[heißen (llamarse)]] `A11` + [[heißen (significar)]] `A21` |
| `es gibt` | explained inside the `geben` note | [[es gibt]], its own `phrase` note |

Each split put the two senses in **different folders**, which is the visible proof
that the rule earns its keep: the everyday meaning stays where it was and the rarer
one moves up, instead of one difficulty being applied to both.

The `Decke` case shows what was actually lost before: *manta* existed only as a
sentence in the body of another note. It could never have appeared in a drill,
could never have accumulated its own errors, and could never have been marked
mastered.

**Two tombstones remain**, `Decke.md` and `heißen.md`, carrying `type: tombstone`
so no view sees them. They exist only because the shell could not reach the folder
on the day of the split. Delete them in Obsidian whenever; nothing links to them.
