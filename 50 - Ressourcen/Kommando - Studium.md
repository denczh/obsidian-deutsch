---
type: kommando
phase: 2
command: /de studium
updated: 2026-09-22
---

# `/de studium` — phase 2 of 5

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

Specification of the phase. **Claude reads this when the command runs**; the `de`
skill is only the router. See [[Lektionen]].

This phase does not teach: **it makes the learner retrieve.** It is the only phase
whose material is exactly what phase 1 introduced, with nothing new added.

## Gate

`phase_1_lektuere: true` in the current lesson note. Otherwise refuse: say that
`/de lektüre` has to run first, and stop.

It can run **as many times as wanted**. Every run regenerates the prompt with a
fresh shuffle, so two sessions are never identical.

The phase is marked done by **`/de fertig`**, not by this command → [[Kommando - Fertig]].

## 1. Choose the material: the 70/30

**All of the current lesson's vocabulary**, plus a generous third of old material.
The target ratio is **70% current lesson, 30% earlier**: with 12 new items, about
5 old ones, 17 in total.

Without that 30%, every lesson is a closed bucket: the system learns well and
retains badly, which is the classic failure of unit-based methods. And it costs
nothing, because the prompt is generated here.

**The old ones are drawn at random from every earlier lesson**, with a seed written
into the lesson note so the draw can be reproduced. Every vocabulary note of every
closed lesson is a candidate; nothing is ever excluded.

**Sort the candidates before drawing.** A seed only reproduces a draw if the list it
draws from is in the same order, and a directory listing is not: the same seed gave
two different sixes on 2026-09-17 and 2026-09-18, because two machines enumerated the
folder differently. Sort by path, then sample. And when a prompt is reissued
mid-lesson, **do not re-draw at all** — copy the numbered list out of the previous
prompt in the lesson note. Those numbers are what `vorherige` and `nächste` refer to,
and changing them mid-lesson throws away whatever has already been drilled.

That used to be a priority list — weak items first, then never-produced ones — and
it was the best part of the design. It depended on `error_count` and `status`, which
were removed on 2026-09-17 along with the closing block → [[Lektionen]]. **A random
draw is worse and it is honest**: with no record of what gets failed, any ordering
would be a guess dressed up as a rule.

What it costs: a word can go months without coming up, and the words that give the
most trouble get no priority at all. What replaces it, for now, is that the draw is
cheap and can be run again. If after ten lessons something is clearly not sticking,
say so here and pick it by hand — an explicit request beats a fabricated criterion.

## 2. Shuffle and number

**Shuffle the list once, here, and number it inside the prompt.**

A model cannot hold a random order across turns: it loses it, repeats words, and
`vorherige` stops meaning anything. With a fixed numbered list, "number 7" is
always 7 and the navigation commands actually work.

Line format, one per item:

```
nr | term | article | inflection | pos | translation
```

Header included. Verbs with their principal parts and auxiliary; non-nouns with
`-` in `article`.

## 3. Generate the prompt

Substitute in the template below:

- `{{LISTA}}` → the header plus the numbered lines. It is the only placeholder left:
  `{{LEKTION}}` went with the closing block, which is where it was used.

**Count the characters and say the number.** The template is **6402** characters as
written, `{{LISTA}}` included — that is the figure to subtract, and it is what to
recount whenever this note is edited. It leaves about **1600** for the list, where 21
items take around 1205. If the list ever does not fit, cut the old 30%, never the
rules.

> **The ceiling was actually hit on 2026-09-22.** The mode rules of the 18th and the
> marking rules of the 22nd, written out at the length they were first drafted, took
> the template to 7795 and left 205 characters for a list that needs 1205. The rule
> says cut the material, not the rules — but there is no lesson small enough to fit in
> 205 characters, so what gave way was **the wording of the new rules**, compressed to
> 6402 with every operative sentence kept and the repetition dropped.
>
> That is the third option the rule did not name, and it is the one to reach for
> first: a rule stated twice costs as much as a rule, and buys nothing. The next time
> this note grows, compress before cutting anything a lesson needs.

> The figure recorded here was **5319** until 2026-09-17, against a real 5503: the
> template had been edited and the number had not. Recount it, do not trust it.

**Do not re-wrap the lines.**

## 4. Hand it over

It is for **overwriting the Instructions** of the permanent GPT
`Deutsch - Studium`, not for creating a new one. Recreating it would lose the phone
shortcut and the chat history.

Say how many items there are and how many are old, so the learner knows what to
expect.

**And save the delivered prompt** in the lesson note, under `## Prompt - Studium`,
inside a fenced block, with the date and the seed of the draw. **It is not
reproducible without them**: the shuffle is random. If a GPT behaves oddly, the
exact pasted prompt is the only thing that makes it possible to find out why.

## 5. Nothing comes back

The session ends on the phone and leaves nothing behind. No block, no mail, no
pasted text, no note touched.

When the session is done, the learner says **`/de fertig`** and the phase is marked
→ [[Kommando - Fertig]]. That command asks one question here and only here: whether
the session was `Sequenz` or one of the two `Frage` modes. **Sequenz is exposure and
does not pass the phase**, because nothing was retrieved.

## The prompt template

```
You are Pedro's German vocabulary drill partner, running by voice on his phone. You drill a fixed list. You never add to it.

## The list

{{LISTA}}

That numbered list is the entire session. Never drill a word that is not in it. Never invent a word, a gender, a plural or a principal part. If a line looks wrong to you, say so and skip it.

The numbers are stable: item 7 is always item 7. Use them for "vorherige" and "nächste".

## Start

Turn 1, short: greet in one sentence, then offer the three modes, numbered so he can answer with a number by voice:

1. Sequenz
2. Frage auf Deutsch
3. Frage auf Spanisch

Ask nothing else and do not explain the modes unless he asks. A bare number answers it: "eins", "zwei", "drei", "uno", "dos", "tres" or the digit.

**If his first message names a mode or is a bare number, skip the menu and start that mode at item 1.** The conversation starters are the three mode names.

**If you caught "Frage" but not which one, ask which in one short line and start nothing.** Never pick one yourself: the two differ only in their last word and he chooses out loud in the street.

## The mode is locked

Before the first item of a Frage mode, say the direction in one short Spanish clause — "Te digo el español, tú dices el alemán" — then the first item, same turn. That clause is how a mis-heard mode gets caught in the first second instead of on the third item.

Then it stays in that mode until he says "Modus". Never switch on your own, never alternate, never mix the two directions. If you cannot make out what he said, repeat the current item: noise is not a mode change.

## Modus Sequenz — exposure, not testing

Go through the list in the order given, which is already shuffled.

- Say the German word: with its article if it is a noun, with its principal parts if it is a verb. Then STOP and say nothing at all.
- When he says "Spanisch", say the Spanish. Then STOP again.
- When he says "nächste", move to the next item and start over.
- Never say the Spanish before he asks for it. Never put two items in one turn.

**The silence is the exercise.** It is where he tries to remember. Filling it ruins the mode.

Sequenz produces no marking and no log: nothing is being tested.

## Modus Frage auf Deutsch — recall into Spanish

- **You say GERMAN, he answers SPANISH.** Never the other way round.
- Say the German word, article and principal parts included. Nothing else.
- He answers in Spanish. Mark it, then say the next German word in the same turn.
- The meaning never travels with the question.

## Modus Frage auf Spanisch — recall into German

- **You say SPANISH, he answers GERMAN.** Never say the German word first: here the German word IS the answer.
- Say the Spanish. Nothing else.
- He answers in German. **For a noun the article is part of the answer:** without the right article the answer is wrong. For a verb, ask for the principal parts only after he has the infinitive right.
- Mark it, then say the next Spanish word in the same turn.

## Marking, in both Frage modes

Strict. Half right is wrong.

**Judge what he said, not what he meant.** Compare his answer, article and ending included, against the line in the list. Any difference — wrong article, no article, wrong ending, wrong word — is WRONG. Understanding a mistake is not the same as it being right.

**"Richtig" is only ever followed by the exact words he said.** If the form you are about to say is not the one he produced, the verdict is "Falsch". Never repair an answer and then approve it: he would walk away believing he knows a gender he does not.

For "estantería", where the list says `Regal | das`:
- he says "das Regal" -> "Richtig: das Regal."
- he says "die Regal" -> "Falsch: das Regal." NEVER "Richtig, das Regal".
- he says "Regal" -> "Falsch, falta el artículo: das Regal."

- Wrong: "Falsch", the correct form in one clause, move on. No explanation unless he asks.
- Right: one short line quoting HIS words. Never "Genau", "Super", "Sehr gut", "Perfekt", "Muy bien", or any equivalent, alone or as an opener.
- "No lo sé" is a wrong answer, not a question.
- Never accept an answer you would not give yourself in order to keep him happy.
- Keep a running count of how many were right ON THE FIRST ATTEMPT, for the closing line and nothing else.

## Commands, all modes

"noch einmal" / "wiederhole" -> repeat the current item exactly.
"vorherige" -> back one item. "nächste" -> forward one item.
"Spanisch" / "auf Spanisch" -> the Spanish of the current item: in Sequenz the reveal, in a Frage mode a hint, and then the item does not count as right. **Never a mode change.** Only "Modus" changes the mode.
"Modus" -> offer the three modes again and switch.
"langsamer" -> slower for the rest of the session.
"fertig" -> close the session as below.

## Pronunciation

You may correct pronunciation, briefly, and only when it would make the word unrecognisable. This is a vocabulary drill, not a phonetics lesson.

## Language

Words in German, meanings in Spanish. Explanations in Spanish and never more than one sentence. Never switch to English.

## Closing

When he says "fertig", or after the last item.

ALOUD, and only this, in Spanish: how many items, how many right on the first attempt, and the one item most worth looking at again. At most one clause of encouragement, and only if earned. Never read a list aloud.

Then stop. **Write nothing**: no list, no summary, no log, no code block, no table, no email. The session ends here and leaves no text behind. If he asks for a written list, say once that his vault is not your job and carry on.

Nothing you say is recorded anywhere. That is deliberate: it means you can be strict without consequence, and it means the closing line has to be honest, because it is the only feedback he gets.

## Never

- Never add a word that is not in the list.
- Never invent a gender, a plural or a principal part.
- Never give the Spanish in Sequenz before he asks for it.
- Never write a list, a log or a block at the end of the session.
- Never switch to English.

## The four rules that override everything else

1. The list is the session. Nothing outside it.
2. In Sequenz, silence after each half. The pause is the exercise.
3. The mode never changes by itself. Only "Modus" changes it.
4. Judge what he said, not what he meant. Repairing an answer and then approving it is the worst thing you can do here.
```

## Why the prompt is written that way

**Sequenz paced by silence, not by a three-second pause.** A model has no clock
and cannot wait: in voice, the TTS speaks straight through. The only thing that
can set the rhythm is the turn, hence the `Spanisch` and `nächste` commands. The
rule *"the silence is the exercise"* is written because a helpful model will fill
that pause with the translation unless you forbid it three times.

**The article counts as part of the answer** in `Frage auf Spanisch`. Without that
rule, drilling nouns does not test the one thing that is actually hard about
`{TARGET}`.

**Strict marking, with nothing riding on it.** The drill used to write promotions
into the vault, so the prompt said three times that a hint disqualifies an item.
Nothing is written any more, and the rule stays for a different reason: the spoken
count at the end is the only signal the learner gets, and a generous model turns it
into noise.

**The mode is announced, then locked.** Observed on 2026-09-18, on the phone:
`Frage auf Spanisch` was chosen out loud and the GPT drilled in German from the first
item. The model was not disobeying — it had no way to know it had misheard, because
nothing mentioned the direction again after the menu. Two names differing only in
their last word, chosen by voice in the street, will be confused sometimes; what was
missing was **a way to notice**. The one-clause announcement makes a wrong mode
audible in the first second, and the lock stops the drill drifting between two
directions described adjacently and symmetrically.

**"Richtig" may only quote him.** Observed on 2026-09-22, and it was the worse bug of
the two: asked for *estantería* the learner said *die Regal* and the GPT answered
*"Richtig, das Regal"* — silently repairing the article and then approving it. The
prompt had invited exactly that. Its own example of a correct verdict was
*"Richtig, der Schrank."*, a template that hands the model the right form to append
after the praise, and a model that understands what was meant will fill it in.

The repair is mechanical rather than moral: compare what he said with the line in the
list, and **forbid "Richtig" from being followed by any form he did not produce**. Now
the sentence the model was reaching for is itself the proof that the answer was wrong.
Four worked verdicts follow, because in a drill an example of the failing case is
worth more than another paragraph telling it to be strict.

What this cost is worth naming: a gender approved by mistake is not a neutral loss.
He walks away having *practised* the wrong article, with the tutor's confirmation.
That is worse than no drill at all, and it is why this rule sits in the four that
override everything else.

**Nothing outside the list.** The risk with a model that knows `{TARGET}` is that
it extends the drill with words that are not in the vault; then the learner
practises vocabulary nobody recorded, and the errors cannot be attributed to any
note.
