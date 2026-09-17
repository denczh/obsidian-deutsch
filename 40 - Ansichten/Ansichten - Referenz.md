---
type: reference
updated: 2026-09-17
---

# Views: what each one filters

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

The four views are `.base` files (Bases core plugin). If a version of Obsidian
does not recognise the `note.` prefix in filters, the view will come up empty:
rebuild it from the UI with the same filter, or drop the prefix in the file.

| View | Filter | Order |
|---|---|---|
| **Aktuelle Lektion** | `lektion == "L002"` | `created` desc |
| **Nach Niveau** | type vocab or grammar | `cefr`, then name |
| **Nach Thema** | type vocab or grammar | `theme`, then name |
| **Ohne Beispiel** | `type == "vocab"` and `example` empty | `created` asc |

## Notes

**There were six until 2026-09-17.** `Schwachstellen` filtered on `error_count > 0`
and `status != "known"` and was the revision queue — the view that actually got
studied. `Nicht gesprochen` filtered on `source != "voice-session"` and
`status == "new"` and listed words that had never come out of the learner's mouth.

Both were deleted with the three fields they read, which only the closing block ever
wrote → [[Lektionen]]. **A view whose filter names a field nothing writes is not a
view, it is a queue filling towards nothing** — exactly what `Nicht in Anki` was,
three days earlier. Deleting them was the honest move; the cost is that the vault no
longer answers *what do I keep getting wrong.*

**Aktuelle Lektion carries the lesson written inside the file.** When `L003` starts,
that line gets edited. It is the only thing edited by hand when a lesson changes,
and `/de lektüre` does it.

**Nach Niveau groups by `cefr`**, which is the difficulty of the word and not the
learner's level. It is the view for asking what is there at A11 and what is
missing. See [[Niveaus]].

**Nach Thema replaces a per-theme folder.** The theme lives in frontmatter as a
list; per-theme folders are exactly the competing hierarchy to avoid. For real
visual grouping, open the view and turn on *Group by* → `theme` in the UI.

## Two notes of the same word

One note is one sense ([[Bedeutungen]]), so the same word can appear twice in a
view — with different `cefr`, different `lektion` and different `status`. That is
not duplication.

To tell them apart at a glance, the views that can show both carry two extra
columns: **`pos`**, the grammatical category, and **`sense`**, the short label that
disambiguates the meaning. `Nach Niveau` has both; the others have `pos`.

**`note.level` was still listed as a column in four views** until 2026-09-12 — a
leftover from the September migration, and in one of them it was also the sort key.
An unknown field does not raise an error: the column just comes up blank and the
sort silently does nothing. Now they all use `cefr`. This is exactly the class of
failure the `/de commit` health check exists for.

## The views that were removed

**`Nicht in Anki`, 2026-09-14**, with the `anki` field it filtered on. Both came from
the source document and neither was ever used: no export step existed, so the field
was never written and the view returned zero. It was not harmless — the filter was
`anki == false` and `status != "new"`, so the first time anything reached `learning`
it would have started filling up on its own, a queue growing towards a destination
that did not exist.

**`Schwachstellen` and `Nicht gesprochen`, 2026-09-17**, with `status`,
`error_count` and `last_error`. Different cause, same shape: the closing block that
wrote those fields was retired, so both views would have frozen — one permanently
empty, the other permanently full — while still looking like features.

Three for three now, and the pattern is worth naming: **when a writer is removed, its
readers go in the same edit.** A view outliving its field is the hardest failure in
this vault to see, because nothing errors.

## Two tokens, two meanings, one spelling each

`cefr` is one of twelve: `A11 A12 A21 A22 B11 B12 B21 B22 C11 C12 C21 C22`. Never
`A2.1`, never `a11`. It decides the folder.

`lektion` is `L001`, `L002`… always three digits. Never `L1`. It is a field and
never a folder.

The moment a folder says `A21` and a note says `A2.1` is the moment `Nach Niveau`
splits in two without warning.
