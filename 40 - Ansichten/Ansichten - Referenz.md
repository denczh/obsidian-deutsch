---
type: reference
updated: 2026-09-07
---

# Views: what each one filters

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

The seven views are `.base` files (Bases core plugin). If a version of Obsidian
does not recognise the `note.` prefix in filters, the view will come up empty:
rebuild it from the UI with the same filter, or drop the prefix in the file.

| View | Filter | Order |
|---|---|---|
| **Schwachstellen** | `error_count > 0`, `status != "known"`, type vocab or grammar | `last_error` desc |
| **Aktuelle Lektion** | `lektion == "L001"` | `created` desc |
| **Nach Niveau** | type vocab or grammar | `cefr`, then name |
| **Nach Thema** | type vocab or grammar | `theme`, then name |
| **Nicht in Anki** | `anki == false` and `status != "new"` | `cefr`, then name |
| **Ohne Beispiel** | `type == "vocab"` and `example` empty | `created` asc |
| **Nicht gesprochen** | `source != "voice-session"` and `status == "new"` | `created` asc |

## Notes

**Schwachstellen is the view that actually gets studied.** The original spec
defined it as "`last_error` is not empty"; `error_count > 0` is equivalent and does
not break if a date gets written in a different format one day. The sort is still
by `last_error`.

**The second filter, `status != "known"`, is the exit door** (added 2026-08-02).
Without it nothing ever left this view: `error_count` only goes up, so a mistake
mastered three months ago kept showing up forever and the queue grew without limit.
`error_count` stays as the historical record — it is a fact, and it is what to sort
by for how much trouble something gave. What retires an item is marking it `known`,
and what justifies that is the `OK` block of `/de studium` and `/de gramatik`. See
[[Lektionen]].

**Aktuelle Lektion carries the lesson written inside the file.** When `L002` starts,
that line gets edited. It is the only thing edited by hand when a lesson changes,
and `/de lektüre` does it.

**Nach Niveau groups by `cefr`**, which is the difficulty of the word and not the
learner's level. It is the view for asking what is there at A11 and what is
missing. See [[Niveaus]].

**Nicht gesprochen is the counterweight to the EXTRA block.** A tutor can add words
that were never said, with no limit. The obvious risk is that they pile up as
homework nobody does. This view lists them while they are still `status: new`; use
one in a session, move it to `learning`, and it disappears. If it grows without
stopping, the problem is not the view.

It filters on `source != "voice-session"` rather than on `tutor-extra`, so it also
catches hand-added words (`source: manual`) and lesson words that have never been
produced (`source: lektuere`). Any word that has not come out of the learner's
mouth in a session is an unused word, wherever it came from.

**Nach Thema replaces a per-theme folder.** The theme lives in frontmatter as a
list; per-theme folders are exactly the competing hierarchy to avoid. For real
visual grouping, open the view and turn on *Group by* → `theme` in the UI.

## Two tokens, two meanings, one spelling each

`cefr` is one of twelve: `A11 A12 A21 A22 B11 B12 B21 B22 C11 C12 C21 C22`. Never
`A2.1`, never `a11`. It decides the folder.

`lektion` is `L001`, `L002`… always three digits. Never `L1`. It is a field and
never a folder.

The moment a folder says `A21` and a note says `A2.1` is the moment `Nach Niveau`
splits in two without warning.
