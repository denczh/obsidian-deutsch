---
type: reference
updated: 2026-09-07
---

# Themes

> `{TARGET}` = German · `{KNOWN}` = Spanish → [[Configuration]]

**A theme is a tag, never a folder.** It goes in the frontmatter as a list:
`theme: [reisen, alltag]`. The [[Nach Thema.base|Nach Thema]] view replaces a
per-theme folder completely.

Use exactly these tokens, one spelling, the same discipline as the `cefr` tokens.

| Token | `{TARGET}` | `{KNOWN}` |
|---|---|---|
| `alltag` | Alltag | vida diaria |
| `beruf` | Beruf und Arbeit | trabajo |
| `einkaufen` | Einkaufen und Geld | compras y dinero |
| `essen-trinken` | Essen und Trinken | comida y bebida |
| `familie` | Familie und Beziehungen | familia y relaciones |
| `gesundheit` | Gesundheit | salud |
| `wohnen` | Haus und Wohnung | casa y vivienda |
| `reisen` | Reisen und Urlaub | viajes y vacaciones |

Extend the list when a lesson produces a theme that does not fit — but **add it
here first.** A token invented on the fly is an orphan tag.

## How the themes are used

`/de lektüre` offers three themes at the start of a lesson and skips the ones used
in the last two lessons, so a story does not land on the same ground twice in a
row. The `{TARGET}` column is the name; the `{KNOWN}` one is what goes in
parentheses while the production level is low.

A theme also becomes the `theme` field of every note the lesson creates, which is
what makes [[Nach Thema.base|Nach Thema]] usable later.
