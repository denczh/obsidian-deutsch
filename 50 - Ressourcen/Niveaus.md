---
type: reference
active_level: L1
updated: 2026-08-01
---

# Niveles: mi escala

La bóveda no usa CEFR. Usa una escala propia y abierta: **`L1`, `L2`, `L3`…**,
tantos niveles como hagan falta, definidos por lo que yo he consolidado y no por
una tabla externa.

**Nivel activo: `L1`.**

## Por qué

La escala CEFR describe a un estudiante genérico y reparte doce casillas entre
"no sé nada" y "casi nativo". Mi caso no es ese: vengo con comprensión por
delante de producción y con huecos que no siguen el orden de ningún libro.
Colgar mis carpetas de una escala ajena obliga a decidir si una palabra es "A21
o A22", una pregunta que no tiene respuesta y que no cambia nada de lo que hago
después.

`L1` no significa principiante. Significa *donde estoy hoy*.

## Reglas del token

- Una sola grafía: `L`, mayúscula, y un número sin ceros delante.
- Nunca `l1`, `L-1`, `L 1`, `Nivel 1`, `L1.5`.
- El token aparece en exactamente tres sitios: el nombre de la carpeta, el campo
  `level` del frontmatter, y la línea `level:` del bloque de cierre del tutor.
- Las carpetas se crean **cuando promociono**, no antes. No hay carpetas vacías
  esperando.

## Criterio de promoción

> Paso de `Ln` a `Ln+1` cuando tengo **cinco estructuras en *Consolidado*** del
> [[Lernprofil]] y **`Schwachstellen` no contiene nada con `error_count` mayor
> que 2**.

La segunda mitad es la que importa. Sin ella, promocionar es solo acumular
sesiones; con ella, promocionar significa que lo viejo ya no sangra.

Una estructura entra en *Consolidado* cuando la produzco correctamente, sin que
me la pidan, en tres sesiones distintas. Si la vuelvo a fallar, retrocede.

## Procedimiento al promocionar

Tres sitios a mano, en este orden:

1. **Crear las carpetas** `20 - Wortschatz/L2` y `30 - Grammatik/L2`.
2. **Editar `40 - Ansichten/Aktuelles Niveau.base`**: cambiar
   `note.level == "L1"` por `"L2"`.
3. **Editar [[Lernprofil]]**: `active_level`, el nivel activo del cuerpo, y
   sobre todo el par de calibración CEFR — es lo que hace que el tutor te
   empiece a exigir más.

Y después, en las Instructions del GPT: la línea `level: L2` del bloque de
cierre, y los dos valores de producción y comprensión.

**Las notas viejas no se mueven.** `L1` es un registro histórico de lo que
aprendí ahí, no una etiqueta de dificultad que haya que mantener al día. Si
reclasificas algo, es porque te equivocaste al archivarlo, no porque hayas
mejorado.

## La única CEFR que queda

Un modelo no sabe qué es `L1`. Por eso las Instructions del tutor siguen
llevando dos referencias:

| Para qué | Valor hoy |
|---|---|
| Nivel al que me pide producir | `A12` |
| Nivel al que me habla | `A22` |

**Viven solo en [[Lernprofil]] y en [[GPT-Anweisungen]].** No entran en ninguna
nota, ni en ningún nombre de carpeta, ni en el bloque de cierre. Ese bloque
escribe `level: L1` y nada más.

## Historial

| Nivel | Desde | Hasta | Calibración CEFR |
|---|---|---|---|
| `L1` | 2026-08-01 | — | A12 / A22 |
