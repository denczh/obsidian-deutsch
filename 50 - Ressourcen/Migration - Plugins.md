---
type: reference
updated: 2026-09-27
deadline: 2026-12-11
---

# Porting the system off custom GPTs

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

**Custom GPTs stop running on 2026-12-11** → [[GPTs]]. This note is the plan for
getting off them, and it is temporary: delete it once the four are ported.

## What actually changes

Not the storage of a prompt. The shape of the thing that runs it.

| | A GPT | A skill inside a plugin |
|---|---|---|
| What it is | **a container you enter** | **a capability you invoke inside a normal chat** |
| How it starts | open the GPT, then voice | `@mention`, `+` → More, or ChatGPT picks it |
| Its instructions | the system prompt of that chat | one skill among whatever else is loaded |
| How it is updated | paste over Instructions, on the web | conversationally, via `@skill-creator` |
| Phone access | a link, added to the home screen | unknown |

**The third row is the risk.** Half of what these prompts do is negative — never say
"Genau", never write a list, never give the Spanish before it is asked for, never
approve an answer you repaired. Those rules work because the Instructions *are* the
session. A skill competing with ChatGPT's ordinary helpfulness may hold them less
firmly, and the failure would be quiet: a tutor that is slightly kinder than it should
be, which is the exact failure this whole system was built against → [[Lernprofil]].

## Do not migrate. Rebuild.

The **Migrate to plugin** button is a one-way door: the GPT goes read-only, cannot be
deleted, and no plugin ever converts back. What it buys is copying the Instructions
across automatically.

**We do not need that.** Every prompt in this system is generated from this vault by
`/de studium`, `/de vorlesen` and `/de gramatik`. The text sitting in a GPT is an
output, not a source. There is nothing in those containers worth a one-way door.

So: build the replacement alongside, with both alive, and switch when the replacement
is better. The GPTs keep working until 2026-12-11.

## Phase 0 — the probe

One throwaway skill, before any decision. **Use the Gramatik prompt**: phase 4 has
never run, nothing depends on it, and its prompt is not being iterated on.

1. In ChatGPT, `@skill-creator`, and give it the generated Gramatik prompt as the
   skill's instructions.
2. Then answer these four questions, in this order of importance:

| # | Question | Why it decides everything |
|---|---|---|
| 1 | **Can it be invoked by voice?** Does saying the skill's name start it, or does invoking need the screen? | The three drill phases are voice-only, walking. If invoking needs typing, the port is not a port. |
| 2 | **Does it hold a negative rule?** Give it a wrong answer with a repaired article and see whether it says *Falsch*. Say something vague and see whether it opens with "Genau". | This is the one that cannot be worked around by pasting harder. |
| 3 | **How few taps from the home screen?** | Two taps is the difference between doing a phase and not bothering → [[GPTs]]. |
| 4 | **Where does the lesson's word list go?** | See below. |

Write the four answers **here**, in this note, the day the probe runs. A probe nobody
recorded has to be run again.

The probe's own instructions are `Claude outputs/Sonda - Skill Drill (probe).txt`:
five items, the real negative rules, nothing from L002. It is an instrument, not a
lesson — it is meant to be thrown away.

> **Tell `@skill-creator` not to rewrite it.** It drafts skills from a description, and
> a paraphrase would invalidate the probe: the rules being tested are the exact
> negative ones — *never "Genau"*, *never repair an answer*, *write nothing at the end*.
> Say "use this text verbatim as the skill body, do not rewrite or summarise it", then
> read back what it saved before testing.

### Results — run on YYYY-MM-DD

| # | Question | Answer |
|---|---|---|
| 1 | Invoked by voice? | |
| 2 | Holds a negative rule? | |
| 3 | Taps from the home screen | |
| 4 | Does a pasted list get used? | |

**Verdict:** (port to skills · keep GPTs until December and paste into a normal voice
chat · look for another host)

## Phase 1 — the shape, once the probe answers

The likely good shape, and the reason to hope: a skill is **reusable**, and our prompts
are two things stuck together — **stable rules** and **this lesson's material**.

- The skill holds the rules: the modes, the marking, the language frame. Edited rarely,
  when a session exposes a bug, exactly as `Kommando - Studium` is edited now.
- The material — the numbered list, the story, the sentences — is **pasted as the first
  message of the session** instead of being baked into the container.

If that works, it is better than what we have: the 8000-character ceiling stops being a
ceiling on both at once, the rules stop being re-pasted every lesson, and
`/de studium` generates a list instead of a whole prompt. It would also end the
overwrite ritual that has already lost work twice.

**Do not assume it works.** Question 4 of the probe is exactly this: paste a list into
a chat where the skill is active and see whether the skill actually uses it.

## Phase 2 — rewrite the specs

Once the shape is known, the `Kommando - *` notes change in one place each: step 4,
*hand it over for overwriting the Instructions of the permanent GPT*. Everything else —
how the story is written, how the 30% is drawn, how the marking works — is untouched.
That is the payoff of keeping the logic in the vault rather than in the container
→ [[Lektionen]].

[[GPTs]] gets rewritten rather than edited: it describes a container that will not
exist.

## Phase 3 — port the four, in this order

1. **Gramatik** — already done as the probe.
2. **Vorlesen** — phase 3, also never run.
3. **Sprechen** — free conversation. Its prompt is permanent and hand-maintained, so it
   is the one that benefits most from becoming a skill → [[Modus - Sprechen]].
4. **Studium** — last. It is under active repair, and a prompt that changed seven times
   in ten days is the worst thing to have inside a container that is also changing.

Keep each GPT until its replacement has survived a real session. Two work-alikes for a
week is cheap; discovering in December that the rules do not hold is not.

## Phase 4 — before 2026-12-11

- Delete the four GPTs, or let them expire. Nothing in the vault points at them by link.
- Delete this note.
- If any GPT was migrated rather than rebuilt, its read-only ghost stays in the account
  for good. That is the other reason not to press the button.

## The deadline is not the real constraint

Eleven weeks is plenty for four prompts that are generated anyway. The real constraint
is that **the probe answers might be bad** — that a skill cannot be started by voice,
or will not be strict. If that is what comes back, the answer is not a better prompt;
it is a different host for the voice sessions, and that decision needs the weeks that
pressing the migrate button in December would not leave.

Run the probe early for that reason and no other.
