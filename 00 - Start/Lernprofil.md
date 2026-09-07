---
type: profile
updated: 2026-09-07
lektionen_done: 0
cefr_production: A12
cefr_comprehension: A22
---

# Learner profile — Pedro

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

**Current lesson:** L001 · **Updated:** 2026-09-07

> This file is the only authoritative source on calibration. Claude reads it when
> generating every lesson prompt, so it does not need copying anywhere — with one
> exception: [[Modus - Sprechen]] has a permanent prompt whose learner values have
> to be refreshed by hand every 4-6 sessions.

## Calibration

The one thing a model cannot infer: **how hard to speak.**

| For what | Level | Note |
|---|---|---|
| What I am asked to produce | **A12** | short sentences, word order unstable |
| What I am spoken to in | **A22** | I lose the thread at normal speed |

**The gap is deliberate.** I produce at A12; I am spoken to at A22. Understanding
more than I can say is normal and that gap is the mechanism by which a dormant
language comes back. **Never collapse the two into one number.**

This pair is the only CEFR that enters a prompt. The `cefr` field on a vocabulary
note is a different thing: the difficulty of that word. See [[Niveaus]].

**Reviewed at every `/de commit`**, and only changed deliberately. If almost
everything comes out right first time for three lessons running, production is
behind reality. If almost nothing does, it is ahead.

## Seen, not consolidated

- [[Possessivartikel meine]] — possessive in the nominative, `mein` / `meine`
- [[Dativ mit in]] — `in` + dative for location

## Pending, in suggested order

1. Gender of household nouns: the feminines are the ones I get wrong.
2. `in` + accusative (movement), as a contrast to the dative already seen.
3. Dative plural with `-n` (`in den Regalen`).

## Recurring mistakes to watch

Filled from the `ERRORS` blocks. Patterns, not the full list — the full list is in
the notes and in [[Schwachstellen.base|Schwachstellen]].

- **Possessive agreement with gender** — *Mein Tür* → *Meine Tür* (2026-08-01).
  See [[Possessivartikel meine]].

## Recent themes (last five lessons)

Filled from the lesson notes. `/de lektüre` skips the last two.

- `wohnen` — Haus und Wohnung (2026-08-01, L001)

## Preferences

These shape every generated prompt. They are instructions, not decoration.

- Correction batched, not after every sentence.
- Grammar explained in `{KNOWN}` while production is below B1.
- I would rather talk a lot and be corrected briefly than be taught.
- No written exercises and no homework in voice phases: I am walking.
- **No empty validation.** No "Genau", "Super" or equivalent as a turn opener.
  Being told I am right when I am not does not encourage me — it irritates me and
  it costs the tutor my trust in every correction that follows. Praise only works
  if it is specific and the sentence was finished and correct.
- **My mid-sentence pauses are not the end of my turn.** I speak slowly because my
  production level is low. Wait. (2026-08-01)

> Those last two are the ones that have to be restored every single time a prompt
> is rewritten. They are what makes a session usable.
