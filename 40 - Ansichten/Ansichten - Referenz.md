---
type: reference
---

# Vistas: qué filtra cada una

Las siete vistas son archivos `.base` (plugin Bases, ya activado). Si tu versión
de Obsidian no reconoce el prefijo `note.` en los filtros, la vista aparecerá
vacía: en ese caso recréala desde la interfaz con estos mismos filtros, o borra
el prefijo `note.` en el archivo.

| Vista | Filtro | Orden |
|---|---|---|
| **Schwachstellen** | `error_count > 0`, `status != "known"`, tipo vocab o grammar | `last_error` desc |
| **Aktuelle Lektion** | `lektion == "L001"` | `created` desc |
| **Nach Niveau** | tipo vocab o grammar | `cefr`, luego nombre |
| **Nach Thema** | tipo vocab o grammar | `theme`, luego nombre |
| **Nicht in Anki** | `anki == false` y `status != "new"` | `cefr`, luego nombre |
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
es marcarlo `known`. Y lo que justifica marcarlo `known` es el bloque `OK` de
`/de studium` y `/de gramatik`. Ver [[Lektionen]].

**Aktuelle Lektion lleva la lección escrita dentro del archivo.** Al empezar
`L002`, edito `40 - Ansichten/Aktuelle Lektion.base` y cambio esa línea. Es lo
único que hay que tocar a mano al pasar de lección, y lo hago yo en `/de lektüre`.

**Nach Niveau agrupa por `cefr`**, que es la dificultad de la palabra y no mi
nivel. Es la vista para preguntarse qué tengo de A11 y qué me falta. Ver
[[Niveaus]] y [[Lektionen]].

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

**Dos tokens, dos significados, una sola grafía cada uno:**

`cefr` es uno de los doce: `A11 A12 A21 A22 B11 B12 B21 B22 C11 C12 C21 C22`.
Nunca `A2.1`, nunca `a11`. Decide la carpeta.

`lektion` es `L001`, `L002`… con tres dígitos siempre. Nunca `L1`. Es un campo y
nunca una carpeta.

El momento en que una carpeta dice `A21` y una nota dice `A2.1` es el momento en
que `Nach Niveau` se parte silenciosamente en dos.
