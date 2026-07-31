---
type: reference
---

# Vistas: qué filtra cada una

Las cinco vistas son archivos `.base` (plugin Bases, ya activado). Si tu versión
de Obsidian no reconoce el prefijo `note.` en los filtros, la vista aparecerá
vacía: en ese caso recréala desde la interfaz con estos mismos filtros, o borra
el prefijo `note.` en el archivo.

| Vista | Filtro | Orden |
|---|---|---|
| **Schwachstellen** | `error_count > 0` y tipo vocab o grammar | `last_error` desc |
| **Aktuelles Niveau** | `level == "A12"` | `created` desc |
| **Nach Thema** | tipo vocab o grammar | `theme`, luego nombre |
| **Nicht in Anki** | `anki == false` y `status != "new"` | nivel, luego nombre |
| **Ohne Beispiel** | `type == "vocab"` y `example` vacío | `created` asc |

## Notas

**Schwachstellen es la vista que de verdad estudias.** El documento la define
como "`last_error` no vacío"; aquí uso `error_count > 0`, que es equivalente y
más robusto (un campo numérico no se rompe si escribes la fecha en otro
formato). El orden sigue siendo por `last_error`.

**Aktuelles Niveau lleva el nivel escrito dentro del archivo.** Cuando promociones
de A12 a A21, edita `40 - Ansichten/Aktuelles Niveau.base` y cambia esa línea.
Es lo único de la bóveda que hay que tocar a mano al subir de nivel.

**Nach Thema sustituye a una carpeta por tema.** El tema vive en frontmatter como
lista; las carpetas por tema son justo la jerarquía en competencia que hay que
evitar. Si quieres agrupación visual real, abre la vista y activa *Group by* →
`theme` desde la interfaz.

**Los 12 tokens de nivel, una sola grafía en todas partes:**

`A11 A12 A21 A22 B11 B12 B21 B22 C11 C12 C21 C22`

Nunca `A2.1`. El momento en que las carpetas dicen `A21` y las notas dicen
`A2.1` es el momento en que cada vista filtrada se parte silenciosamente en dos.
