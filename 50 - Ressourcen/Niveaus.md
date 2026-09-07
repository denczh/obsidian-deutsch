---
type: reference
updated: 2026-09-07
---

# Niveles: la dificultad de las palabras

> **Reescrita el 2026-09-07.** Esta nota describía una escala propia `L1, L2…` que
> medía *mi* posición y decidía las carpetas. Ya no existe. Historial al final.

El campo **`cefr`** clasifica **la dificultad de la palabra** en la escala
internacional, y decide en qué carpeta vive:

```
20 - Wortschatz/A11/  30 - Grammatik/A11/
```

No mide dónde estoy yo. Mide lo avanzado que es el vocabulario. `sein` es `A11`
aunque yo lleve tres años; `aufbewahren` es `B11` aunque lo aprendiera el primer
día.

## Por qué así, y no como antes

Tuve una escala propia (`L1`, `L2`…) precisamente para no depender de una tabla
externa. El problema no era la escala: era que **decía dos cosas a la vez** —
cuándo aprendí algo y lo difícil que era— y luego colisionó con la secuencia de
lecciones, que dice la primera. Dos campos con el mismo nombre es lo que rompe
vistas en silencio.

Al separarlos, cada uno hace una cosa bien:

| Campo | Qué significa | Dónde vive |
|---|---|---|
| **`cefr`** | dificultad de la palabra | **decide la carpeta** |
| **`lektion`** | en qué lección entró | campo, **nunca carpeta** |

Y la CEFR resulta ser buena para esto aunque fuera mala para lo otro: es un
**conjunto cerrado de doce**, es estable, y describe la palabra, no al estudiante.
La pregunta "¿esto es A21 o A22?" ahora sí tiene respuesta, porque es una pregunta
sobre alemán y no sobre mí.

## Los doce tokens

`A11 A12 A21 A22 B11 B12 B21 B22 C11 C12 C21 C22`

Una sola grafía, siempre. Nunca `A2.1`, nunca `a11`, nunca `A2`. El momento en que
una carpeta dice `A21` y una nota dice `A2.1` es el momento en que
[[Nach Niveau.base|Nach Niveau]] se parte en dos sin avisar.

**Las carpetas se crean cuando una palabra cae ahí**, no antes. Hoy hay cinco
pobladas: `A11`, `A12`, `A21`, `A22`, `B11`.

## Quién asigna el `cefr`

Yo, al crear la nota en `/de lektüre`. Y hay que decirlo: **es un juicio
aproximado.** No hay una lista oficial palabra por palabra, así que dos personas
razonables discreparían en los bordes.

Eso está bien para lo que sirve —agrupar, navegar, decidir qué es pronto y qué es
tarde— y no está bien para nada que dependa de precisión. Ninguna vista crítica
filtra por `cefr`: [[Schwachstellen.base|Schwachstellen]] no lo mira, y el drill
de `/de studium` tampoco. Si un día una palabra te parece mal clasificada,
arrástrala de carpeta y cambia el campo; no rompes nada.

## Lo que se perdió al quitar la escala propia

**El criterio de promoción.** Antes había una definición checkable de *he
mejorado*: cinco estructuras consolidadas y nada con `error_count` mayor que 2. Ya
no hay nada equivalente, porque las lecciones miden actividad, no capacidad.

Lo que sigue midiendo dominio, elemento por elemento:

- `status: new → learning → known`, movido por los bloques `ERRORS` y `OK`.
- [[Schwachstellen.base|Schwachstellen]], que se vacía solo cuando algo llega a
  `known`.

Es suficiente para saber qué estudiar. No es suficiente para saber si estoy
mejorando. Si dentro de tres meses lo echo de menos, la reparación natural es un
criterio sobre `known` — *el 80% de A11 y A12 en `known`* — no resucitar el campo.

## Y la calibración del tutor

Un modelo no sabe qué es `L001` ni le sirve el `cefr` de una palabra suelta para
decidir a qué dificultad hablarme. Eso sigue siendo el par CEFR del
[[Lernprofil]]: **producción A12, comprensión A22.**

Vive solo ahí, es mi entrada cuando genero cada prompt, y no entra en ninguna nota
de vocabulario. Ver [[Lektionen]].

## Historial

| Fecha | Qué |
|---|---|
| 2026-07-31 | doce carpetas CEFR, `level` = mi nivel |
| 2026-08-01 | escala propia `L1`, una carpeta, `level` = mi posición |
| 2026-09-07 | `cefr` = dificultad de la palabra, `lektion` = cuándo entró |
