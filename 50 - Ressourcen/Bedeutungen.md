---
type: reference
updated: 2026-09-17
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

It used to appear in a third place, the fourth column of every closing block. Blocks
were retired on 2026-09-17 → [[Lektionen]].

**`pos` belongs to the sense, not to the word.** The same word can be a noun in one
meaning and a verb in another, and each meaning is its own note with its own `pos`.

## 2. One note = one sense

A note describes **one meaning**. If a word has two unrelated meanings, it gets two
notes.

This is not tidiness. Each sense has its own difficulty, its own lesson, its own
error count and its own mastery:

```yaml
# 20 - Wortschatz/A12/Decke (1).md
term: Decke
sense: techo
pos: noun
article: die
translation: "techo"
cefr: A12
lektion: L001
```

```yaml
# 20 - Wortschatz/A21/Decke (2).md
term: Decke
sense: manta
pos: noun
article: die
translation: "manta"
cefr: A21
lektion: L004
```

Same word, two folders, two lessons, two independent entries. Merged into one note,
that collapses: one difficulty would be applied to both meanings, and only one of
them would ever reach a drill.

> Each sense also used to carry its own `status` and `error_count`, and that was the
> strongest form of the argument: you could record that *ceiling* was mastered and
> *blanket* never once produced. Those fields went on 2026-09-17 → [[Lektionen]]. The
> rule stands on the reasons that are left, which are enough.

### The fields that make it work

| Field | Role |
|---|---|
| **`term`** | the bare word, **identical across all its senses**. This is what groups them: a search for `term: Decke` finds every sense. |
| **`sense`** | a short label in `{KNOWN}` naming **this** meaning. `-` while the word has only one sense in the vault. |
| the **title** | the bare term when there is one sense; `Term (n)` once there are two, where `n` is a plain ordinal. |

**An absent `sense` means the same as `sense: "-"`**: the only sense in the vault.
That is the same convention as `article: "-"` for a non-noun, and it is why the
notes written before this rule existed did not all have to be edited. The field
becomes **mandatory the moment a second sense appears** — at that point both notes
carry one, because a disambiguator that only one of a pair has is not a
disambiguator.

`term` stays bare on purpose. It is what makes `[[Bahnsteig]]` work inside an
example sentence, and that link is how the vault accumulates a record of every
sentence where a word has been used.

### The number in the title

The disambiguator is **a number, not a translation**: `heißen (1)`, `heißen (2)`.
Three rules, and they are all about the same thing — a filename is an identity.

**Assigned in order of arrival.** Sense 1 is the one that was in the vault first.
When two senses are split in the same operation, as the inherited cases were, the
lower `cefr` takes `(1)`: the everyday meaning is the one that was going to be met
first anyway.

**Never reassigned.** A third sense is `(3)` even if it is the most basic of the
three. Renumbering would rename a file that links, git history and your own memory
already point at, to buy a tidiness nobody reads.

**Never reused.** If `(2)` is deleted or merged away, the next sense is still `(3)`.
Gaps are correct; a reused number silently makes two different things look like the
same thing in the history.

There is **no `sense_number` field.** The number lives in the title and nothing
filters on it. A field written once and read never is the mistake `anki` was.

#### What this costs, and what pays for it

A number carries no meaning. A link reading `heißen (2)` in a list tells you
nothing, where one reading `heißen (significar)` told you everything — that is a
real loss and it is the price of the rule.

Two things pay it back. A filename is the most expensive string in the vault to
change: it lives in every link, in git history and in the Bases cache, and a
translation is exactly the kind of thing that gets reworded a year later. A number
never needs rewording. And the translation was in the wrong language anyway — a
`{KNOWN}` word inside the title of a `{TARGET}` note, where nothing else in the
filename is in `{KNOWN}`.

What replaces it: **`sense` stops being decoration and becomes the only
machine-readable carrier of the meaning.** It was redundant with the title before;
now it is not. That is why [[Nach Niveau.base|Nach Niveau]] shows it as a column, and
why the `/de commit` check that no two notes share a `term` without both carrying a
`sense` matters more than it did: an empty `sense` on `heißen (2)` now leaves nothing anywhere that says
what it means.

And where a bare number would read badly — a long word list, a cross-reference —
**write the link with an alias**: `[[heißen (2)|heißen, significar]]`. The filename
stays stable, the prose stays readable. The alias is presentation and can be changed
freely; the filename is identity and cannot.

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

1. **Rename the existing note** to `Term (1)` and fill its `sense` field. It was
   there first, so it is `(1)` — the new arrival never takes the number.
2. **Update every link to it.** Renaming inside Obsidian does this automatically and
   preserves any alias; an agent editing files has to do it by hand. A link left
   pointing at the old bare title becomes a broken link, and broken links are the
   one thing `/de commit` checks for.
3. **Create the new note** as `Term (2)`, in the folder its own `cefr` dictates —
   which is often not the same folder as sense 1.
4. **Cross-link them** under `## Verwandt`, both ways, and say in one clause how to
   tell them apart. That clause is the useful part of the note, and it is where the
   meaning that used to be in the filename now lives.

Renaming is deliberately the cost of the *second* sense, not the first. Numbering
every note `Bahnsteig (1)` from the start would put a number on nine hundred notes
that will never need one, to avoid a rename that happens rarely.

## Categories and inflection

`pos` is one of nine: `noun`, `verb`, `adj`, `adv`, `prep`, `conj`, `pron`, `num`,
`phrase`. **Never omitted and never guessed afterwards** — and it belongs to the sense
rather than to the word.

**No noun-only lists.** They are the easiest thing to list and the least useful thing
to have. `inflection` is filled according to the category:

| Category | What goes in `inflection` |
|---|---|
| `noun` | the plural: `-e`, `-en`, `Häuser`, or `-` if there is none |
| `verb` | irregular third person, Präteritum, and Perfekt **with its auxiliary**: `fährt, fuhr, ist gefahren`. Separables split: `räumt auf, räumte auf, hat aufgeräumt` |
| `adj` | comparative and superlative only if irregular: `besser, am besten` |
| everything else | `-` |

`article` is `-` for anything that is not a noun. The `term` field is bare:
`Bahnsteig`, never `der Bahnsteig` — the article has its own field, and a term with
the article glued on stops matching a search for the word.

> This section lived in `E-Mail-Format` until 2026-09-17, because it described the
> columns of the closing block. The block is gone and the conventions are not: they
> are how every note is written. They moved here rather than being deleted with their
> old home → [[Lektionen]].

## Who applies this

**The agent, at note-creation time**, because only the agent can see what already
exists in the vault. A voice GPT cannot: it has never read the vault, and it never
will.

That was the argument for the closing block carrying **no sense column**: the GPT
reported `Decke | die | -n | noun | manta` and the agent looked up `term: Decke`, found
a note saying `techo`, recognised the collision and ran the procedure above. Asking the
GPT to disambiguate would have meant asking it about a vault it cannot see.

Since 2026-09-17 the GPTs report nothing at all and every note is written here, so the
question no longer arises → [[Lektionen]].

See [[Kommando - Lektüre]] for the create step.

## Done: the three inherited cases

Three words mixed senses from before this rule existed. All three were split on
2026-09-12, and they are the worked example of the procedure:

| Word | Was | Now |
|---|---|---|
| `Decke` | one note, `techo (interior)`, *manta* only mentioned in the body | [[Decke (1)\|Decke (1), techo]] `A12` + [[Decke (2)\|Decke (2), manta]] `A21` |
| `heißen` | one note, `llamarse; significar` | [[heißen (1)\|heißen (1), llamarse]] `A11` + [[heißen (2)\|heißen (2), significar]] `A21` |
| `es gibt` | explained inside the `geben` note | [[es gibt]], its own `phrase` note |

Each split put the two senses in **different folders**, which is the visible proof
that the rule earns its keep: the everyday meaning stays where it was and the rarer
one moves up, instead of one difficulty being applied to both.

The `Decke` case shows what was actually lost before: *manta* existed only as a
sentence in the body of another note. It could never have appeared in a drill,
could never have accumulated its own errors, and could never have been marked
mastered.

The two tombstones the split left behind, `Decke.md` and `heißen.md`, were deleted
on 2026-09-14.

**The four notes were first titled `Decke (techo)`, `heißen (significar)` and so
on.** They were renumbered on 2026-09-14, two days after the split — which is the
argument for numbers in one line: the naming scheme changed before the notes were a
week old, and every title that encoded a translation had to be touched. A number
would have survived it.
