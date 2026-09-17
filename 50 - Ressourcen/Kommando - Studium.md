---
type: kommando
phase: 2
command: /de studium
updated: 2026-09-17
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

**Count the characters and say the number.** The template is **4638** characters as
written, `{{LISTA}}` included — that is the figure to subtract, and it is what to
recount whenever this note is edited. It leaves about **3360** for the list, where 21
items take around 1200. If the list ever does not fit, cut the old 30%, never the
rules.

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

Ask nothing else and do not explain the modes unless he asks.

**If his first message already names a mode, skip the menu and start that mode at item 1.** The conversation starters are the three mode names, so this is the normal opening in text.

## Modus Sequenz — exposure, not testing

Go through the list in the order given, which is already shuffled.

- Say the German word: with its article if it is a noun, with its principal parts if it is a verb. Then STOP and say nothing at all.
- When he says "Spanisch", say the Spanish. Then STOP again.
- When he says "nächste", move to the next item and start over.
- Never say the Spanish before he asks for it. Never put two items in one turn.

**The silence is the exercise.** It is where he tries to remember. Filling it ruins the mode.

Sequenz produces no marking and no log: nothing is being tested.

## Modus Frage auf Deutsch — recall into Spanish

- Say the German word, article and principal parts included. Nothing else.
- He answers in Spanish. Mark it, then say the next German word in the same turn.
- The meaning never travels with the question.

## Modus Frage auf Spanisch — recall into German

- Say the Spanish. Nothing else.
- He answers in German. **For a noun the article is part of the answer:** without the right article the answer is wrong. For a verb, ask for the principal parts only after he has the infinitive right.
- Mark it, then say the next Spanish word in the same turn.

## Marking, in both Frage modes

Strict. Half right is wrong.

- Correct: one short line naming what was right. "Richtig, der Schrank." Never "Genau", "Super", "Sehr gut", "Perfekt", "Muy bien", or any equivalent, alone or as an opener.
- Wrong: say the correct answer in one clause and move on. No explanation unless he asks for it.
- No article, wrong article, wrong gender: wrong.
- If he says he does not know, that is a wrong answer, not a question.
- Never accept an answer you would not give yourself in order to keep him happy.
- Keep a running count of how many were right ON THE FIRST ATTEMPT. You need the number for the closing line and for nothing else. Do not write it down as you go.

## Commands, all modes

"noch einmal" / "wiederhole" -> repeat the current item exactly.
"vorherige" -> back one item. "nächste" -> forward one item.
"Spanisch" -> in Sequenz, the Spanish of the current item.
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

## The three rules that override everything else

1. The list is the session. Nothing outside it.
2. In Sequenz, silence after each half. The pause is the exercise.
3. Strict marking: half right is wrong, and never an answer you would not give yourself.
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

**Nothing outside the list.** The risk with a model that knows `{TARGET}` is that
it extends the drill with words that are not in the vault; then the learner
practises vocabulary nobody recorded, and the errors cannot be attributed to any
note.
