---
type: reference
updated: 2026-09-07
---

# Processing: block to vault

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

> This describes processing a [[Modus - Sprechen|free conversation]] session, which
> lives outside the lesson cycle. Inside the cycle the same steps are done by
> `/de commit`. See [[Lektionen]].

Same day, while the conversation is still fresh. Twenty minutes of backlog is fine;
a week of backlog means the notes never get written.

1. **Create `10 - Sitzungen/YYYY-MM-DD.md`** from `V - Sitzung`. Paste the raw
   block at the bottom under the *Roh* heading. That is the provenance: if a note
   later looks wrong, the original is there.

2. **For each VOCAB line**, a note in `20 - Wortschatz/<cefr>/`, where `cefr` is the
   difficulty of the word → [[Niveaus]]. And `lektion` with the current lesson, or
   `-` when it comes from free conversation. The template depends on `pos`:
   - `pos: verb` → **`V - Verb`**, with conjugation and preposition government.
   - anything else → **`V - Wortschatz`**.

   Fill `term`, `article`, `inflection`, `pos`, `translation`, `cefr`, `lektion`
   and `theme`. Then add the one thing the list does not carry: **an example
   sentence in `{TARGET}`, written by the learner**. That act of production is
   worth more than the note.

   Both templates write `type: vocab`. A verb is not a different type of note, it
   is the same note with more fields filled.

2b. **For each EXTRA line**, the same, but with `source: tutor-extra`. These are
   words that were never said: the example here is not a memory, it is an exercise.
   Writing the sentence is the first time the word gets used. They show up in
   [[Nicht gesprochen.base|Nicht gesprochen]] until their `status` changes.

3. **For each GRAMMAR line**, create *or update* a note in `30 - Grammatik/<cefr>/`.
   Rules recur across sessions: look for an existing note first. Write the two or
   three sentence explanation in `{KNOWN}` and two examples while the session is
   fresh; a bare label will mean nothing three weeks later.

4. **For each ERRORS line**, find the note it belongs to and set `last_error` to
   today, increment `error_count`, set `status: learning`. If no note exists, create
   one: a mistake is evidence that the item matters.

4b. **For each OK line** (only `/de studium` and `/de gramatik` produce these): if
   it was `learning` it becomes `known`; if it was `new` it becomes `learning`.
   **`error_count` is never touched.** This is the only step that removes anything
   from [[Schwachstellen.base|Schwachstellen]].

5. **Update [[Lernprofil]]**: append to recurring mistakes, refresh recent themes.

6. **Commit and push.**

> Step 4 is the one that repays the effort. Everything else records what happened;
> step 4 is what turns the vault into a revision system. And step 4b is what stops
> it growing forever.

## Accelerators

- The **Templates** core plugin points at `90 - Vorlagen`. Steps 2 and 3 are one
  command plus typing.
- A **parser script** could turn a pasted block into stub notes with the
  frontmatter already filled, leaving only the example sentences and the
  explanations. Worth writing once the format has settled.

## Export to spaced repetition

Filter `anki = false` in [[Nicht in Anki.base|Nicht in Anki]], export `term` and
`translation` to CSV, flip the flag. Or use the spaced-repetition plugin and stay
in the vault. A chat is a poor drilling tool and a good conversation partner; let
each do its own job.
