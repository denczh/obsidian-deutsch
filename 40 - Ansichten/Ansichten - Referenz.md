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
| **Schwachstellen** | `error_count > 0`, `status != "known"`, tipo vocab o grammar | `last_error` desc |
| **Aktuelles Niveau** | `level == "L1"` | `created` desc |
| **Nach Thema** | tipo vocab o grammar | `theme`, luego nombre |
| **Nicht in Anki** | `anki == false` y `status != "new"` | nivel, luego nombre |
| **Ohne Beispiel** | `type == "vocab"` y `example` vacío | `created` asc |
| **Nicht gesprochen** | `source != "voice-session"` y `status == "new"` | `created` asc |

## Notas

**Schwachstellen es la vista que de verdad estudias.** El documento la define
como "`last_error` no vacío"; aquí uso `error_count > 0`, que es equivalente y
más robusto (un campo numérico no se rompe si escribes la fecha en otro
formato). El orden sigue siendo por `last_error`.

**El segundo filtro, `status != "known"`, es la puerta de salida** (añadido
2026-08-02). Sin él nada abandonaba nunca esta vista: `error_count` solo sube,
así que un error de hace tres meses ya dominado seguía apareciendo para siempre y
la cola crecía sin límite. `error_count` se queda como registro histórico —es un
hecho, y sirve para ordenar por cuánto te costó algo—; lo que retira un elemento
es marcarlo `known`. Y lo que justifica marcarlo `known` es el bloque `OK` del
modo [[Modus - Wiederholung|Wiederholung]]. Ver [[Modi]].

**Aktuelles Niveau lleva el nivel escrito dentro del archivo.** Cuando promociones
de `L1` a `L2`, edita `40 - Ansichten/Aktuelles Niveau.base` y cambia esa línea.
Es uno de los tres sitios que hay que tocar a mano al subir de nivel; los otros
dos están listados en [[Niveaus]].

**Nicht gesprochen es el contrapeso del bloque EXTRA.** El tutor puede añadir
palabras que nunca dijiste, sin límite de cantidad. El riesgo obvio es que se
acumulen como deberes que nunca haces. Esta vista las lista mientras siguen en
`status: new`; en cuanto uses una en una sesión, ponla en `learning` y desaparece.
Si crece sin parar, el problema no es la vista.

Filtra por `source != "voice-session"`, no por `tutor-extra`, para que también
recoja lo que añado a mano (`source: manual`). Cualquier palabra que no haya
salido de mi boca en una sesión es una palabra sin estrenar, venga de donde venga.

**Nach Thema sustituye a una carpeta por tema.** El tema vive en frontmatter como
lista; las carpetas por tema son justo la jerarquía en competencia que hay que
evitar. Si quieres agrupación visual real, abre la vista y activa *Group by* →
`theme` desde la interfaz.

**Los tokens de nivel son mi escala, una sola grafía en todas partes:**

`L1`, `L2`, `L3`… Sin punto, sin espacio, sin cero delante. Nunca `l1` ni `L-1`
ni `Nivel 1`. El momento en que las carpetas dicen `L1` y las notas dicen `L 1`
es el momento en que cada vista filtrada se parte silenciosamente en dos.

Nada de CEFR entra en la bóveda. Las dos referencias `A12` / `A22` viven solo en
[[Lernprofil]] y en las Instructions del GPT, para calibrar cómo te habla.
