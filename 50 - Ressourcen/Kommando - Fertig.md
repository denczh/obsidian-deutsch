---
type: kommando
phase: confirmation
command: /de fertig
updated: 2026-09-17
---

# `/de fertig` — confirming a phase

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

Specification of the command. **Claude reads this when the command runs**; the `de`
skill is only the router. See [[Lektionen]].

`/de fertig` is not a sixth phase. It is **how a phase gets marked as done**, and it
exists because phases 2, 3 and 4 happen on a phone, in a GPT that cannot write to
this vault. Something has to cross back, and since 2026-09-17 that something is a
sentence from the learner instead of a mailed block.

## What it replaced, and what that costs

Until 2026-09-17 each voice phase ended by writing a closing block in the chat, a
Make Action mailed it, and it was pasted back at the start of the next command. The
block was the evidence: it proved the phase had happened, and it carried every
mistake into the vault, where it moved `status`, `error_count` and `last_error`.

All of that is gone. The Action, the block, the three fields, the error types and
the two views that read them → see [[Lektionen]].

**So the gate is now a statement, not evidence.** `/de fertig` writes `true`
because the learner says the session happened, and nothing checks it. That is the
deliberate trade: the system stopped measuring item-level mastery in exchange for
never parsing an email again. Saying `/de fertig` without having done the session
is possible, cheap, and only harms the person doing it.

## Gate

The lesson's previous phases have to be `true`, exactly as the phase itself would
require. `/de fertig` **cannot be used to skip a phase**: it marks the next one
that is genuinely pending.

It also refuses on a lesson whose `closed` field has a date. A closed or abandoned
lesson is finished.

## 1. Read the state

The most recent note in `10 - Lektionen/`. Its five `phase_N_*` fields say where the
lesson is.

## 2. Work out which phase is being confirmed

- **No argument** → the **first** phase that is `false` and whose predecessors are
  all `true`. Say which one it is before writing anything.
- **An argument** (`/de fertig studium`, `vorlesen`, `gramatik`) → that phase, if
  its predecessors are `true`. If they are not, refuse and say which is missing.

Phase 1 is never confirmed this way: `/de lektüre` writes the notes itself, so it
marks its own phase. Phase 5 is `/de commit`. **`/de fertig` only ever writes
`phase_2_studium`, `phase_3_vorlesen` or `phase_4_gramatik`.**

If the phase named is already `true`, say so and do nothing. Running a phase twice
is fine and needs no second confirmation.

## 3. The one question, for Studium only

Before marking `phase_2_studium`, ask **which mode the session used**.

`Sequenz` is exposure: nothing is asked, nothing is retrieved. A lesson whose only
Studium session was Sequenz has not passed phase 2, and `/de vorlesen` should not
open on it. If the answer is Sequenz alone, say that and do not write. Offer the
obvious remedy: run `Frage auf Deutsch` or `Frage auf Spanisch` and come back.

That check is what survives of the old rule *"the gate is opened by a Frage session,
not a Sequenz one"*. It used to be provable from an empty block; now it is a
question.

Phases 3 and 4 have no equivalent question. They have one mode each.

## 4. Write

In the lesson note, two things and nothing else:

- the `phase_N_*` field to `true`
- the date in the **Fecha** column of that phase's row in the phases table

No counters, no session log, no notes about what went well. If the learner
volunteers something worth keeping — a GPT that ignored an instruction, a rule that
clearly has not landed — it goes in `## Notas`, as prose, because that is where the
lesson's own history lives. **Never invent that line.** An empty `## Notas` is an
honest one.

## 5. Close the turn

One or two lines: which phase is now `true`, and which command comes next. Nothing
else. Confirming a phase is bookkeeping, not an achievement.

## What this command does not do

- **It does not touch any vocabulary note.** No `status`, no `error_count`, no
  promotion: those fields do not exist any more.
- **It does not generate a prompt.** That is the phase command itself.
- **It does not close the lesson.** That is `/de commit`.
- **It does not ask how it went.** If something is worth recording, the learner says
  so; the command does not interview him after every session.
