---
type: kommando
phase: 1
command: /de lektüre
updated: 2026-09-17
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

### Abandoning a lesson

The normal exit is `/de commit`, and it is the one to prefer. But a lesson can reach
a state where `/de commit` will never accept it: its gate wants all four phases
`true`, and if a phase can no longer be done — the story was never written, the GPT
no longer exists, the lesson is months old — then `lektüre` sends you to `commit`
and `commit` sends you back. That deadlock is what this exit is for.

**Abandoning writes two things and invents no field:**

- `closed` — today's date, exactly as a normal close would.
- `phase_5_commit` — **stays `false`.** Phase 5 did not run, and that field is read
  by a gate. Writing `true` would buy one convenience with a lie in the one place
  the system trusts.

So the two states are told apart by a combination that already exists:

| State | `closed` | `phase_5_commit` |
|---|---|---|
| open | empty | `false` |
| **abandoned** | **a date** | **`false`** |
| closed properly | a date | `true` |

And **a sentence in the body saying why**, under the phases table. A date with no
reason is the thing that will be indistinguishable from a bug six months from now.

**The vocabulary survives.** Abandoning is not discarding: the notes the lesson
created keep their `lektion` and stay in the vault exactly as they are. The lesson
stops, the vocabulary it produced does not.

Never abandon a lesson without being asked to. Offer it, name what will be lost, and
wait. **A lesson abandoned by mistake cannot be reopened by this command** — its
`closed` field will send it straight past the gate.

## 1. Read the vault

Before asking anything:

- `10 - Lektionen/` → the latest lesson, its number and its state. The new one is
  `L{XXX}` where `XXX` = last + 1, always three digits.
- [[Lernprofil]] → the **production** and **comprehension** CEFR pair. It is the
  only thing that says how hard to write.
- `20 - Wortschatz/*/` and `30 - Grammatik/*/` → everything already there. The
  list of `term` values, so nothing gets introduced twice, and the spread by `cefr`.
- Themes already used: the `thema` field of previous lessons and the `theme` field
  of the vocabulary.
- `## Notas` of the previous lessons. Since 2026-09-17 that is the only record of
  what gave trouble, and it is prose, not a field → [[Lektionen]].

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
- **Recycles old material on purpose**, without pointing at it: whatever
  `## Notas` flagged in earlier lessons, and failing that, a good handful of words
  from the lesson before. There is no list of weak items to draw on any more, so if
  something specific should come back, **ask** — one question, at theme time.
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

- `pos` — **the grammatical category**, one of the nine tokens. Never inferred,
  never omitted, and it belongs to the sense rather than the word.
- `sense` — `-` if this is the word's only meaning in the vault; otherwise the
  short `{KNOWN}` label that distinguishes it. See [[Bedeutungen]].
- `cefr` — **the difficulty of this sense**, one of the twelve tokens. Decides the
  folder. See [[Niveaus]].
- `lektion` — `L{XXX}`, the lesson being created.
- `source: lektuere` — this provenance means the word came from a story, not from
  a conversation or a list.
- `example` — **the sentence from the story where the word appears**, verbatim, and
  it has to use *this* sense.
- `theme` — the theme token.
- `translation` — in `{KNOWN}`, for this sense only.
- `article` and `inflection` per [[Bedeutungen]]. Verbs with their auxiliary;
  separables split.

And the opening line of every note body, so the category is visible without
opening the Properties panel:

```
**der Bahnsteig** · *noun* · Plural: *-e* · "andén"
```

### One note, one sense

**Before creating a note, check whether `term` already exists in the vault.** Only
the agent can do this — a voice GPT has never read the vault — so it happens here.

- **No existing note** → create it, `sense: "-"`, title = the bare term.
- **Exists with the same meaning** → do not create a second note. Add the lesson's
  example to the existing one if it is better than what is there, and leave
  `lektion` alone: the word entered in the lesson that first introduced it.
- **Exists with a different meaning** → two notes, and the rename procedure in
  [[Bedeutungen]]: rename the old note to `Term (1)`, **update every link to it**,
  then create `Term (2)` in whatever folder its own `cefr` dictates. The number is
  the order of arrival and is never reassigned; the meaning goes in `sense`, never
  in the title.

The boundary between *another sense* and *another translation of the same sense* is
the judgement call, and [[Bedeutungen]] holds the test: `cuadro` and `imagen` are
one note, `techo` and `manta` are two.

A story is allowed to use a word in a sense that is already in the vault. That is
not a problem to solve — it is recycling, which is what the story is supposed to do.

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
