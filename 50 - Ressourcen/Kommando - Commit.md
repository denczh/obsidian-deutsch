---
type: kommando
phase: 5
command: /de commit
updated: 2026-09-17
---

# `/de commit` — phase 5 of 5

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

Specification of the phase. **Claude reads this when the command runs**; the `de`
skill is only the router. See [[Lektionen]].

Closes the lesson. It is the only phase that **writes to git on purpose** and the
only one that looks at the whole vault instead of just the lesson.

## Gate

All four: `phase_1_lektuere`, `phase_2_studium`, `phase_3_vorlesen`,
`phase_4_gramatik`, every one `true`.

If any is missing, say which and which command does it — phases 2, 3 and 4 are
marked with `/de fertig` → [[Kommando - Fertig]]. **Do not offer to close half-way**:
an incomplete lesson that gets closed is a lesson nobody reopens, and its material
stays unpractised for good.

## 1. Nothing to process

There is nothing outstanding to collect. The voice phases leave no block, no mail
and no pasted text: they are confirmed with `/de fertig` and that is all there was.

What can still be missing is a **prompt**. Check that all three `## Prompt - *`
sections are filled. A prompt that was never saved is not recoverable — the shuffle,
the story and the sentences are generated fresh — so say so rather than closing
quietly over the gap.

## 2. Vault health check

**This is the part that justifies the phase existing.** It is the only moment in
the cycle when everything is inspected after everything has happened, so it is
where the silent failures get caught. None of these raises an error on its own.

| Check | What breaks if it fails |
|---|---|
| valid YAML in every note, **frontmatter closed by `---` on its own line** | the note disappears from every view |
| `cefr` is one of the twelve tokens | [[Nach Niveau.base\|Nach Niveau]] splits in two |
| the folder matches the `cefr` | the note lives somewhere it does not claim to |
| `lektion` matches `L\d{3}` | [[Aktuelle Lektion.base\|Aktuelle Lektion]] cannot see it |
| no residual `level`, `status`, `error_count` or `last_error` field, **including in view column lists** | an unknown field does not error: the column comes up blank and a sort on it silently does nothing |
| every note has a `pos` from the nine tokens | the category is the one thing never inferred |
| no two notes share a `term` unless **both** carry a `sense` | a disambiguator only one of a pair has is not a disambiguator → [[Bedeutungen]] |
| no `translation` lists two unrelated meanings | two senses merged into one note, and one of them will never be drilled |
| the four `.base` files parse | the view looks empty and it seems there is no data |
| zero broken wiki links | orphan notes you thought were connected |
| `example` not empty in new vocabulary | the word has no context |
| `Aktuelle Lektion` filters the lesson being closed | you were looking at the previous one |

And one number worth watching even though it is not an error:

- **`Ohne Beispiel` growing.** It is the last work list left, and the only one that
  still says something about how the vault is being kept. A lesson that adds ten
  notes with no example sentence is a lesson written in a hurry.

The two counts that used to live here — `Schwachstellen` never reaching `known`, and
`Nicht gesprochen` rising every lesson — went with the fields they read. Losing them
means **the vault no longer measures whether anything is being retained**; it records
what was taught. That is the known hole left by the change of 2026-09-17, and it is
written here rather than discovered in six months.

**On the first row.** It is there because it has already happened. On 2026-09-14 the
`anki` field was removed from 56 notes by deleting the string `"\nanki: false"` in
each one. In every file `anki` was the *last* field, so the deletion pulled the
closing delimiter up and left `status: new---`: fifty-four vocabulary notes and both
templates with frontmatter that never terminates. Nothing errored. Obsidian shows the
YAML as body text and every view goes empty, which reads like the views being broken
rather than the notes.

The lesson generalises past that one field: **a bulk edit that removes a line must
match the newline that follows it, not the one before it**, or it silently eats the
next line whenever the target is last. The check that catches it is one search:
`^[a-z_]+:.*---$` must return nothing.

Report whatever comes out. **Do not fix things silently**: if something is wrong,
say so and propose the fix.

## 3. Close the lesson

In the note in `10 - Lektionen/`:

- `closed` with today's date.
- `phase_5_commit: true`.
- The final counters: `vocab_count` and `grammar_count`. The error and OK counts were
  removed on 2026-09-17; nothing produces them.
- Check that all three raw blocks and all **three prompts** are stored. If a prompt
  is missing, say so: it is not recoverable, because neither the shuffle nor the
  story nor the sentences can be regenerated identically.

**Nothing is deleted.** The cleanup the original design asked for has no object:
the prompts and the blocks *are* the provenance, and there is no transient
scaffolding in the vault because the prompts were never written to separate files.
The lesson note stays where it is, marked closed.

Six months from now, the thing that will tell you which story taught you
`Bahnsteig` is exactly that note.

## 4. Update the Lernprofil

- **Recurring mistakes**: whatever the learner said, across the lesson, that keeps
  coming back. This is now dictated rather than derived — there is no error log to
  read — so **ask**, and write only what is answered. An empty section is honest; an
  invented pattern is worse than none, because it will be practised.
- **Recent themes**: add this lesson's theme, keep the last five.
- **The CEFR calibration**: review it, do not change it out of habit. If almost
  everything comes out right first time for three lessons running, the production
  level is behind reality and the material is coming out easy. If almost nothing
  does, it is ahead. **Propose the change, do not apply it**: it is the one number
  in the system that decides how hard everything else is.

## 5. Commit

A message with shape, not `update`:

```
Lektion L002 abgeschlossen: <theme>, <N> words, <M> rules
```

`obsidian-git` has already been committing intermediate states every ten minutes,
so this commit saves nothing that was not saved. **Its value is as a marker**: the
history shows where each lesson ends and what it produced.

Push, and check nothing is left unpushed.

## 6. Close the turn

A short summary:

- what the lesson produced: words and rules
- what came out of the health check
- what is outstanding, if anything
- **that `/de lektüre` is ready for the next lesson**

No long congratulations. A closed lesson is a closed lesson.

## What this phase does not do

- **It does not open the next one.** That is `/de lektüre`, and it is a decision,
  not an automatic consequence of closing.
- **It deletes nothing.**
- **It does not touch the CEFR calibration by itself.** It only proposes.
