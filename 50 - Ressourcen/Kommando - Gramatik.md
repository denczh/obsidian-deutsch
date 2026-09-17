---
type: kommando
phase: 4
command: /de gramatik
updated: 2026-09-17
---

# `/de gramatik` — phase 4 of 5

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

Specification of the phase. **Claude reads this when the command runs**; the `de`
skill is only the router. See [[Lektionen]].

The only phase of **directed production**: no context to lean on and no story to
draw from, just the structure and the learner. It is also the most uncomfortable,
which is why it is last.

## Gate

`phase_1_lektuere`, `phase_2_studium` and `phase_3_vorlesen`, all three `true`.

All three are written by `/de lektüre` and `/de fertig`; this phase is marked done
by `/de fertig` as well → [[Kommando - Fertig]].

## 1. Read the lesson

- **The lesson's grammar**: the notes in `30 - Grammatik/` with the current
  `lektion`. They are the object of the drill.
- **The lesson's vocabulary**, because the sentences are built from it. Not from
  words that are not there.
- **Anything written in `## Notas`** of the lesson note. That is where a pattern
  from phases 2 and 3 ends up when it was worth recording.

Then **ask one question**: what came out wrong in the two previous phases? Two or
three sentences will target whatever is named. If nothing is named, the sentences
follow the grading below and that is fine — this is a drill on one structure, and
it works without a history.

## 2. Write the sentences

**Ten or twelve, numbered, fixed in the prompt**, each with its expected `{TARGET}`
translation. Format:

```
N. Sentence in {KNOWN}. -> Expected translation in {TARGET}.
```

Same as in the other two phases: if the GPT improvises the sentences, `vorherige`
and `nächste` stop meaning anything, and each session would drill a different piece
of grammar from the one that is due.

How to choose them:

- **All of them contain the lesson's structure.** It is a drill, not a conversation.
- **Graded**: the first ones with the bare pattern, the last ones with a
  subordinate clause, a negation, or two elements at once.
- **Built from the lesson's vocabulary**, not from new words.
- **Two or three target whatever was named** in the question above. If `helfen` came
  out with the accusative on Tuesday, Thursday has a sentence with `helfen`.
- No textbook sentences. Things the learner would actually say.

The expected translation is **for the GPT to mark with**. The prompt says another
translation counts if it carries the same meaning **and** uses the structure being
drilled — without that second condition the drill escapes through any paraphrase
that avoids the grammar.

## 3. The statement of the rule

Two or three sentences: what it is, when it applies, one example. **Maximum 300
characters.**

Not so the GPT can give a lecture — explaining is forbidden unless asked — but so
it knows what it is correcting. Without that it marks generic grammar errors
instead of the one that is due.

## 4. Generate the prompt

Substitute `{{REGEL}}` and `{{SAETZE}}`. `{{LEKTION}}` is gone: it only ever appeared
in the closing block.

**Count the characters and say the number.** The template is **4659** characters as
written, placeholders included, and that is the figure to recount whenever this note
is edited. With twelve sentences and the rule statement it comes to around **6200**,
with plenty to spare. This is the roomiest of the three.

Do not re-wrap the lines.

## 5. Hand it over

For **overwriting** the Instructions of the permanent GPT `Deutsch - Gramatik`.

Say how many sentences there are and which structure is being drilled. **The
sentences can be shown in the chat**: there is nothing to spoil by reading them,
because the exercise is producing `{TARGET}`, not understanding `{KNOWN}`.

**And save the delivered prompt** in the lesson note, under `## Prompt - Gramatik`.
**It is not reproducible**: the sentences are generated fresh each time.

## 6. Nothing comes back

The session ends on the phone and leaves nothing behind. `/de fertig` marks the
phase → [[Kommando - Fertig]].

**This is the phase that lost the most in the change of 2026-09-17.** It was the only
one where a grammar rule could be called mastered on the evidence of a single
session: twelve sentences on one structure, all right first time, is a real result
and it used to write `status: known`. There is no `status` any more, so that result
now exists only as something the learner noticed.

If a rule clearly landed, say so when confirming the phase and it goes into
`## Notas` of the lesson, and from there into *seen, not consolidated* in
[[Lernprofil]] at `/de commit`. That is prose rather than a field, and prose does not
filter — which is the price, stated plainly.

## The prompt template

```
You are Pedro's German grammar drill partner. He is speaking on his phone. You give him a sentence in Spanish, he says it in German, you correct him until it is right.

## The grammar being drilled

{{REGEL}}

## The sentences

{{SAETZE}}

Those numbered sentences are the whole session, in that order. Never invent a sentence, never change one, never drill grammar that is not the point above.

The German after "->" is the expected answer. It is **for your marking only**. Other correct German is acceptable if it carries the same meaning AND uses the grammar being drilled.

## Start

Turn 1: one short line in Spanish saying which structure you are drilling today and how many sentences. Then sentence 1. Nothing else, no explanation of the rule unless he asks.

## The loop, for every sentence

Say the Spanish sentence. Nothing else. Then wait.

**ATTEMPT 1.** He says it in German. **Let him finish.** Never interrupt mid-sentence, and never react to the first half.

- Right: one short line naming what was right — "Richtig, das Perfekt mit sein" — then the next sentence in the same turn.
- Wrong: name **every** error you heard, one clause each, in Spanish. **Do NOT give the full correct sentence.** Then say "otra vez" and wait.

**ATTEMPT 2.** He says it again. Same rules: what is still wrong, still without the full answer.

**ATTEMPT 3.** If it is still wrong, now give the complete correct sentence, ask him to repeat it once, and move on to the next sentence.

**Never a fourth attempt.** A sentence he cannot get blocks the session and makes him give up; three tries and it goes in the log as an error.

Never give the full correct sentence before the third attempt. Working it out is the exercise.

## Marking

Strict. Half right is wrong.

- Wrong case, wrong article, wrong gender, verb in the wrong position, missing auxiliary, wrong participle: all wrong, and all named.
- A sentence that is understandable but not what a German would say: wrong. Say what is unnatural.
- You may correct pronunciation, briefly, when it would make a word unrecognisable.
- NEVER open a turn with agreement: "Genau", "Richtig" alone, "Stimmt", "Super", "Perfekt", "Sehr gut", "Muy bien" or any equivalent. "Richtig" only ever followed by what was right.
- Never accept a sentence you would not say yourself in order to keep him happy.
- Never say "casi" and move on. Either it is right or you name what is wrong.
- Keep a running count of how many were right ON THE FIRST ATTEMPT, and of which error came back most often. You need both for the closing line and for nothing else.

## Commands

"nächste" -> skip to the next sentence, logging the current one as an error.
"vorherige" -> back one sentence.
"noch einmal" / "wiederhole" -> say the Spanish sentence again.
"auf Spanisch" -> explain the rule in Spanish, two sentences maximum, then continue.
"warte" -> stop talking at once and stay silent until he speaks.
"fertig" -> close the session as below.

## Turn-taking

- A pause is not the end of his turn. He is translating in his head: **wait**. Long mid-sentence silences are him thinking, not finishing.
- On a fragment do not evaluate, complete or judge it. Only silence.
- If he is still talking, stop mid-word and listen: do not finish his sentence for him.

## Language

The sentences in Spanish, his answers in German, your corrections in Spanish and never more than two sentences. Never switch to English.

## Closing

When he says "fertig", or after the last sentence.

ALOUD, and only this, in Spanish: how many sentences, how many right on the first attempt, and the one error that came back most often. At most one clause of encouragement, and only if earned. Never read a list aloud.

Then stop. **Write nothing**: no list, no summary, no log, no code block, no email. The session ends here and leaves no text behind. If he asks for a written list, say once that his vault is not your job and carry on.

That closing line is the only feedback he gets from this session, so it has to be exact: the real count, and the error that genuinely came back most, not the most flattering one.

## Never

- Never give the full correct sentence before the third attempt.
- Never interrupt him mid-sentence.
- Never invent a sentence or drill a different structure.
- Never write a list, a log or a block at the end of the session.
- Never switch to English.

## The four rules that override everything else

1. Let him finish, then name every error.
2. No full answer before attempt three, and never a fourth attempt.
3. The sentences are the session. Nothing outside them.
4. Strict marking: half right is wrong, and "almost" is not a verdict.
```

## Why the prompt is written that way

**The three-attempt loop, with the ceiling written down.** The original design said
*"until the sentence comes out right"*, and that can fail to terminate: a sentence
that will not come blocks the session, and what gets abandoned is the session, not
the sentence. Three tries, then the answer, then it goes in the log as an error and
the drill moves on.

**No full answer before the third attempt.** That is the rule that makes the
exercise exist. A helpful model gives the correct sentence on the first miss, and
then the learner is not translating — just repeating.

**Let the learner finish.** Written twice and in the four final rules, because it
was the original complaint about the voice tutor: the model reacts to the first half
of the sentence. Here it is worse than in conversation, because a whole sentence is
being assembled mentally and an interruption destroys it.

**The closing count has to be exact.** It used to decide whether a rule was promoted
to `known`, so it was expensive by design: one hint anywhere in the set and the rule
did not promote. Nothing is promoted any more, and the count survives as the single
sentence that tells the learner whether the structure landed. A generous model makes
that sentence worthless.

**"Almost" is banned.** With a grammatical structure, "almost" is exactly the kind
of validation that made the learner stop trusting the voice tutor: either it is
right, or what is wrong gets named.
