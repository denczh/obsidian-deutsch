---
type: kommando
phase: 5
command: /de commit
updated: 2026-09-07
---

# `/de commit` — fase 5 de 5

Especificación de la fase. **Claude la lee al ejecutar el comando**; la skill `de`
es solo el enrutador. Ver [[Lektionen]].

Cierra la lección. Es la única fase que **escribe en git a propósito** y la única
que mira la bóveda entera en vez de solo la lección.

## Puerta

Las cuatro: `phase_1_lektuere`, `phase_2_studium`, `phase_3_vorlesen`,
`phase_4_gramatik`, todas en `true`.

Si falta alguna, decir cuál y con qué comando se hace. **No ofrecer cerrar a
medias**: una lección incompleta cerrada es una lección que nunca vas a volver a
abrir, y su material se queda sin practicar para siempre.

## 1. Procesar lo que quede pendiente

Puede que haya bloques sin procesar: los de una sesión repetida de Studium, o uno
que se pegó tarde. Antes de cerrar:

- Pedir los bloques que falten. Si un `## Roh - *` de la nota de lección está
  vacío pero su fase está en `true`, algo se marcó sin evidencia: decirlo.
- Procesar cada bloque como manda su especificación: `ERRORS` sube `error_count`
  y pone `status: learning`; `OK` sube `learning → known` y `new → learning`.

## 2. Chequeo de salud de la bóveda

**Esta es la parte que justifica que la fase exista.** Es el único momento del
ciclo en que se mira todo después de que todo haya pasado, así que es donde se
cazan los fallos silenciosos. Ninguno de estos da error por sí solo.

| Comprobación | Qué se rompe si falla |
|---|---|
| YAML válido en todas las notas | la nota desaparece de todas las vistas |
| `cefr` es uno de los doce tokens | [[Nach Niveau.base\|Nach Niveau]] se parte |
| la carpeta coincide con el `cefr` | la nota vive donde no dice que vive |
| `lektion` con formato `L\\d{3}` | [[Aktuelle Lektion.base\|Aktuelle Lektion]] no la ve |
| ningún campo `level` residual | resto de la migración de septiembre de 2026 |
| los seis `.base` parsean | la vista aparece vacía y parece que no hay datos |
| cero enlaces wiki rotos | notas huérfanas que creías conectadas |
| `example` no vacío en el vocabulario nuevo | la palabra no tiene contexto |
| `Aktuelle Lektion` filtra la lección que se cierra | mirabas la lección anterior |

Y dos números que hay que mirar aunque no sean errores:

- **`Schwachstellen` creciendo lección tras lección** sin que nada llegue a
  `known`: significa que las fases 2 y 4 no están dando `OK`, y entonces la cola
  de repaso solo sube. Es el fallo que tapamos en agosto; puede volver por otra
  puerta.
- **`Nicht gesprochen` creciendo**: palabras que entran y nunca se usan. Si sube
  cada lección, el 70/30 de Studium no está haciendo su trabajo.

Reportar lo que salga. **No arreglar en silencio**: si algo está mal, decirlo y
proponer el arreglo.

## 3. Cerrar la lección

En la nota de `10 - Lektionen/`:

- `closed` con la fecha de hoy.
- `phase_5_commit: true`.
- Los contadores finales: `vocab_count`, `grammar_count`, `error_count`,
  `ok_count`.
- Comprobar que están los tres bloques crudos y los **tres prompts** guardados.
  Si falta un prompt, decirlo: no es recuperable, porque ni el barajado ni la
  historia ni las frases se pueden regenerar igual.

**No se borra nada.** La limpieza que pedía el diseño original no tiene objeto:
los prompts y los bloques *son* la procedencia, y no hay andamiaje transitorio en
la bóveda porque los prompts nunca se guardaron en ficheros aparte. La nota de
lección se queda donde está, marcada como cerrada.

Dentro de seis meses, lo que te dirá qué historia te enseñó `Bahnsteig` es
exactamente esa nota.

## 4. Actualizar el Lernprofil

- **Errores recurrentes**: los patrones que aparecieron en dos o más fases. No la
  lista completa de errores —eso ya está en las notas— sino lo que se repite.
  Especialmente los `comprehension` sin nota a la que apuntar.
- **Temas recientes**: añadir el tema de esta lección, dejar los cinco últimos.
- **La calibración CEFR**: revisarla, no tocarla por rutina. Si en tres lecciones
  seguidas casi todo sale a la primera, el nivel de producción va por detrás de la
  realidad y el material sale fácil. Si casi nada sale, va por delante. **Proponer
  el cambio, no aplicarlo**: es el único número del sistema que decide cómo de
  difícil es todo lo demás.

## 5. Commit

Mensaje con forma, no `update`:

```
Lektion L002 abgeschlossen: <tema>, <N> Woerter, <M> Regeln, <E> Fehler
```

`obsidian-git` ya ha ido commiteando estados intermedios cada diez minutos, así
que este commit no salva nada que no estuviera salvado. **Su valor es el
marcador**: en el historial se ve dónde acaba cada lección y qué produjo.

Push, y comprobar que no queda nada sin subir.

## 6. Cerrar el turno

Un resumen corto:

- qué produjo la lección: palabras, reglas, errores, aciertos
- qué salió del chequeo de salud
- qué queda pendiente, si algo
- **que `/de lektüre` está listo para la lección siguiente**

Nada de felicitaciones largas. Una lección cerrada es una lección cerrada.

## Lo que esta fase no hace

- **No abre la siguiente.** Eso es `/de lektüre`, y es una decisión tuya, no una
  consecuencia automática de cerrar.
- **No borra nada.**
- **No toca la calibración CEFR por su cuenta.** Solo la propone.
