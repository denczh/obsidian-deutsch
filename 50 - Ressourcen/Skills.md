---
type: reference
updated: 2026-09-27
---

# The four skills

> `{TARGET}` = German · `{KNOWN}` = Spanish · `{LEARNER}` = Pedro → [[Configuration]]

Three of the five phases run on the phone, plus free conversation outside the cycle.
Since 2026-09-27 each of those is a **skill in ChatGPT**, not a custom GPT. Phases 1
and 5 are Claude at the desk and use neither.

**A skill is a container I install by hand; only its contents are generated.** That
sentence is unchanged from the GPT era, and so is everything downstream of it: the
`/de` commands, the specifications, the 8000-character ceiling, the overwrite rule.
What changed is the box.

| Skill | Phase | Instructions | Status | Spec |
|---|---|---|---|---|
| `Studium` | 2 | generated every lesson | **verified 2026-09-27** | [[Kommando - Studium]] |
| `Vorlesen` | 3 | generated every lesson | not built yet | [[Kommando - Vorlesen]] |
| `Gramatik` | 4 | generated every lesson | not built yet | [[Kommando - Gramatik]] |
| `Sprechen` | outside the cycle | **written once, refreshed by hand** | not built yet | [[Modus - Sprechen]] |

Only `Studium` has been run. The other three are prompts that exist on paper: Vorlesen
and Gramatik have never been generated at all, because their phases have never run, and
Sprechen's prompt is permanent and still lives in its own note.

## The format

A skill is a folder, and ChatGPT will hand it back as a zip:

```
studium/
  SKILL.md              the whole thing
  agents/openai.yaml    display_name, for the picker
```

`SKILL.md` is YAML frontmatter and then the prompt, verbatim:

```
---
name: studium
description: German vocabulary drill partner for Pedro using a fixed 21-item
  Spanish-to-German voice practice list. Use when Pedro wants to run the Studium
  vocabulary session, including Sequenz exposure mode, Frage recall mode, navigation
  commands, strict marking, pronunciation handling, and the prescribed German closing.
---

You are Pedro's German vocabulary drill partner, running by voice on his phone.
...
```

**`description` is not decoration.** It is what ChatGPT reads to decide whether the
skill applies to what was just said, which is how a skill gets picked without being
named. It replaces the GPT's Description field, which changed nothing.

## Building one

Via `@skill-creator` in a text chat, with the generated prompt pasted in.

**Say "use this text verbatim as the skill body; do not rewrite or summarise it."**
It is a drafting tool and will otherwise paraphrase. That matters more here than
anywhere else in this system: half of what these prompts do is negative — *never
"Genau"*, *never repair an answer and approve it*, *write nothing at the end* — and a
polite paraphrase drops exactly those.

It obeyed on 2026-09-27: the `studium` skill came back **byte-identical** to the
prompt handed to it, all 6644 of them. Check anyway, by reading back what it saved.

## Where the built ones live

`Claude outputs/skills/<name>/` — the folder ChatGPT expects, ready to install:

| Folder | Built from | State |
|---|---|---|
| `studium/` | `## Prompt - Studium` of the current lesson | **installed and verified** |
| `sprechen/` | the fenced block in [[Modus - Sprechen]] | built, never installed |

`vorlesen/` and `gramatik/` do not exist yet. Their prompts have never been generated,
because phases 3 and 4 have never run and their gates will not open until 2 and 3 are
confirmed → [[Kommando - Fertig]]. They get built the first time their phase runs,
which is the right moment anyway: a skill with a placeholder prompt in it is a skill
that will be installed and forgotten.

That folder is **output, not source**. The source is the `Kommando - *` notes plus the
lesson. Delete it whenever it gets confusing; `/de studium` writes it again.

## Overwrite, never recreate

Every lesson, Claude generates a prompt that **replaces** the body of an existing
skill. Same rule as before, same reason: the invocation, the history and the habit
live in the container.

> **Unverified:** whether a downloaded skill folder can be re-uploaded. If it can, the
> update loop gets better than the GPT one ever was — Claude writes `SKILL.md`, you
> upload the folder, no pasting into a phone. Try it on the next `/de studium` and
> record the answer in [[Migration - Plugins]].

## What a skill is, and is not

A GPT was a container you entered: its Instructions were the system prompt of that
chat. A skill is invoked **inside an ordinary conversation**, by `@mention`, from
`+` → More, or because ChatGPT judged it relevant.

That difference was the real risk of the port, and the reason for the probe: rules
that work by being the whole system prompt might hold less firmly as one skill among
whatever else is loaded. The Studium probe says they hold. **Three of the four skills
have not been tested for this**, and the test is one wrong article — say *"die Regal"*
where the answer is *das Regal* and see whether the verdict is *Falsch*.

## When something goes wrong

**The skill ignores a rule.** Note it in the lesson note under *Notes*, then tighten
that one line in the phase specification — not in the skill by hand, because the
prompt gets regenerated next lesson and the edit would vanish. The specifications are
the `Kommando - *` notes.

**The prompt does not fit.** The cap is 8000 characters. Shorten the material — the
word list, the story, the sentences — **never the rules**. And before either: look for
rationale, which belongs in the specification rather than in the prompt, and for whole
features that could go. Removing one mode on 2026-09-27 freed more than ten days of
compression → [[Kommando - Studium]].

**A skill behaves strangely and it is not obvious why.** The exact prompt that was
installed is in the lesson note under `## Prompt - *`. It is not reproducible: the
draw is random and the stories and sentences are generated fresh.

## The GPTs

Retired here on 2026-09-27, ahead of OpenAI retiring them on 2026-12-11. Four
containers, one mail Action and a webhook, all gone. The port cost four commands and
four pastes, because the prompts were outputs of this vault rather than things kept
inside the containers — which was the whole point of the September rewrite
→ [[Lektionen]].

What is still worth deleting by hand, in the ChatGPT editor: the four GPTs themselves,
and the Make webhook, which carried no authentication.
