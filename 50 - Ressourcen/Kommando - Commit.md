---
type: kommando
phase: 5
command: /de commit
updated: 2026-09-07
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

If any is missing, say which and which command does it. **Do not offer to close
half-way**: an incomplete lesson that gets closed is a lesson nobody reopens, and
its material stays unpractised for good.

## 1. Process whatever is outstanding

There may be unprocessed blocks: from a repeated Studium session, or one pasted
late. Before closing:

- Ask for any missing blocks. If a `## Roh - *` section in the lesson note is empty
  but its phase is `true`, something was marked without evidence: say so.
- Process each block as its specification requires: `ERRORS` raises `error_count`
  and sets `status: learning`; `OK` moves `learning → known` and `new → learning`.

## 2. Vault health check

**This is the part that justifies the phase existing.** It is the only moment in
the cycle when everything is inspected after everything has happened, so it is
where the silent failures get caught. None of these raises an error on its own.

| Check | What breaks if it fails |
|---|---|
| valid YAML in every note | the note disappears from every view |
| `cefr` is one of the twelve tokens | [[Nach Niveau.base\|Nach Niveau]] splits in two |
| the folder matches the `cefr` | the note lives somewhere it does not claim to |
| `lektion` matches `L\d{3}` | [[Aktuelle Lektion.base\|Aktuelle Lektion]] cannot see it |
| no residual `level` field | leftover from the September 2026 migration |
| the six `.base` files parse | the view looks empty and it seems there is no data |
| zero broken wiki links | orphan notes you thought were connected |
| `example` not empty in new vocabulary | the word has no context |
| `Aktuelle Lektion` filters the lesson being closed | you were looking at the previous one |

And two numbers worth watching even though they are not errors:

- **`Schwachstellen` growing lesson after lesson** with nothing reaching `known`:
  it means phases 2 and 4 are not producing `OK`, and the revision queue only ever
  grows. That is the leak plugged in August; it can come back through another door.
- **`Nicht gesprochen` growing**: words that arrive and never get used. If it rises
  every lesson, the 70/30 in Studium is not doing its job.

Report whatever comes out. **Do not fix things silently**: if something is wrong,
say so and propose the fix.

## 3. Close the lesson

In the note in `10 - Lektionen/`:

- `closed` with today's date.
- `phase_5_commit: true`.
- The final counters: `vocab_count`, `grammar_count`, `error_count`, `ok_count`.
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

- **Recurring mistakes**: the patterns that showed up in two or more phases. Not the
  full error list — that is already in the notes — but what repeats. Especially the
  `comprehension` ones with no note to point at.
- **Recent themes**: add this lesson's theme, keep the last five.
- **The CEFR calibration**: review it, do not change it out of habit. If almost
  everything comes out right first time for three lessons running, the production
  level is behind reality and the material is coming out easy. If almost nothing
  does, it is ahead. **Propose the change, do not apply it**: it is the one number
  in the system that decides how hard everything else is.

## 5. Commit

A message with shape, not `update`:

```
Lektion L002 abgeschlossen: <theme>, <N> words, <M> rules, <E> errors
```

`obsidian-git` has already been committing intermediate states every ten minutes,
so this commit saves nothing that was not saved. **Its value is as a marker**: the
history shows where each lesson ends and what it produced.

Push, and check nothing is left unpushed.

## 6. Close the turn

A short summary:

- what the lesson produced: words, rules, errors, correct answers
- what came out of the health check
- what is outstanding, if anything
- **that `/de lektüre` is ready for the next lesson**

No long congratulations. A closed lesson is a closed lesson.

## What this phase does not do

- **It does not open the next one.** That is `/de lektüre`, and it is a decision,
  not an automatic consequence of closing.
- **It deletes nothing.**
- **It does not touch the CEFR calibration by itself.** It only proposes.
