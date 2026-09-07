---
type: kommando
phase: 1
command: /de lektüre
updated: 2026-09-07
---

# `/de lektüre` — fase 1 de 5

Especificación de la fase. **Claude la lee al ejecutar el comando**; la skill `de`
es solo el enrutador. Si quiero cambiar cómo funciona la fase, edito esta nota, no
la skill. Ver [[Lektionen]].

## Puerta

Ninguna. Es la primera fase. Pero **no se puede abrir una lección nueva si la
anterior está sin cerrar**: si la última nota de `10 - Lektionen/` tiene
`closed:` vacío, hay que decírmelo y ofrecer dos salidas —continuar esa lección o
abandonarla explícitamente— antes de crear otra.

## 1. Leer la bóveda

Antes de preguntar nada:

- `10 - Lektionen/` → la última lección, su número y su estado. La nueva es
  `L{XXX}` con `XXX` = último + 1, tres dígitos siempre.
- `00 - Start/Lernprofil.md` → el par de calibración CEFR: **producción** y
  **comprensión**. Es lo único que dice a qué dificultad escribir.
- `20 - Wortschatz/*/` y `30 - Grammatik/*/` → todo lo que ya tengo. Necesito la
  lista de `term` para no volver a introducir nada, y el reparto por `cefr` para
  saber dónde estoy.
- Las debilidades: `error_count > 0` y `status != "known"`. La historia debe
  **reciclarlas** a propósito.
- Los temas ya usados: el campo `thema` de las lecciones anteriores y el `theme`
  del vocabulario.

## 2. Preguntar el tema

Ofrecer **tres temas** de [[Themenliste]], **saltando los de las dos últimas
lecciones**. Si pido un tema repetido a propósito, aceptarlo: entonces la historia
tiene que ir a un rincón distinto del tema y las palabras nuevas tienen que ser de
verdad nuevas, no sinónimos de las que ya tengo.

Aceptar también un tema libre que no esté en la lista. Si es recurrente, añadirlo
a [[Themenliste]] antes de seguir; un token inventado sobre la marcha es una
etiqueta huérfana.

## 3. Escribir la historia

**Una narración con personajes que interactúan**, en dos partes. La parte 1 es
esta fase; la parte 2 la escribe `/de vorlesen` y tiene que poder continuarla.

Reglas de la historia:

- **Entre 150 y 250 palabras.** Suficiente para que el vocabulario aparezca en
  contexto, corto para leerlo dos veces sin esfuerzo.
- **Calibrada al nivel de comprensión del [[Lernprofil]]**, no al de producción.
  El punto es entender más de lo que sé decir.
- **Personajes con nombre**, dos o tres, que hablen entre ellos. El diálogo es lo
  que hace que las expresiones suenen a lengua y no a lista.
- **Termina abierta.** La parte 2 continúa: que quede algo por resolver.
- **Recicla al menos tres debilidades** de la bóveda, sin señalarlas.
- Nada de glosarios ni negritas dentro del texto. Es una historia, no una lección.

Después de la historia en alemán, **la traducción al español debajo**, separada.
El orden importa: leer el alemán dos veces antes de mirar el español. Si la
traducción va al lado, se lee solo la traducción.

## 4. El vocabulario nuevo

**Entre 10 y 15 elementos**, más **1 o 2 puntos de gramática**. Se van a drillar
en `/de studium`, así que caben más que en una sesión hablada.

**Nada de listas de solo sustantivos.** El reparto que hay que buscar:

- **verbos**, incluidos separables y los que rigen caso o preposición
- **adverbios**, sobre todo de frecuencia, tiempo y grado
- **adjetivos**, en pares de contrarios cuando salga natural
- **expresiones y frases hechas**, como un solo elemento (`pos: phrase`)
- **conectores y preposiciones**
- sustantivos, sí, pero no la mitad de la lista

**Nunca inventar** una palabra, un género o una forma para rellenar una categoría.
Si no estoy seguro de un género, usar otra palabra.

## 5. Crear las notas

Una nota por elemento, desde la plantilla que corresponda:

| `pos` | Plantilla | Carpeta |
|---|---|---|
| `verb` | `V - Verb` | `20 - Wortschatz/<cefr>/` |
| cualquier otro | `V - Wortschatz` | `20 - Wortschatz/<cefr>/` |
| gramática | `V - Grammatik` | `30 - Grammatik/<cefr>/` |

Campos que hay que rellenar sin excepción:

- `cefr` — la **dificultad de la palabra**, uno de los doce tokens. Decide la
  carpeta. Ver [[Niveaus]].
- `lektion` — `L{XXX}`, la lección que se está creando.
- `source: lektuere` — provenance nueva: la palabra viene de una historia, no de
  una conversación ni de una lista.
- `example` — **la frase de la historia donde aparece la palabra**, literal.
- `theme` — el token del tema.
- `translation` — en español.
- `article` e `inflection` según [[E-Mail-Format]]. Verbos con auxiliar; separables
  partidos.

**Sobre `example`:** antes se dejaba vacío para que la frase la escribiera Pedro.
Ahora la escribe la historia, y es mejor: es una frase real, en contexto, con los
personajes. La producción se ha movido a `/de vorlesen` y `/de gramatik`, que son
habladas, y hablar vale más que escribir una frase en una nota.
[[Ohne Beispiel.base|Ohne Beispiel]] deja de ser una lista de deberes y pasa a ser
lo que su nombre dice: un detector de notas hechas con prisa.

En el cuerpo de cada nota de gramática, la explicación en español de 2-3 frases y
dos ejemplos, **uno de ellos de la historia**.

## 6. Crear la nota de lección

`10 - Lektionen/L{XXX}.md` desde `V - Lektion`, con:

- `thema`, `started` con la fecha de hoy, `phase_1_lektuere: true`
- la **historia parte 1** completa, en alemán, bajo su encabezado
- la lista de vocabulario y gramática introducidos, con enlaces
- `vocab_count` y `grammar_count`

La parte 2 queda vacía: la escribe la fase 3.

## 7. Actualizar la vista

En `40 - Ansichten/Aktuelle Lektion.base`, cambiar el filtro a la lección nueva.
Es lo único que hay que tocar a mano al pasar de lección.

## 8. Cerrar el turno

Mostrar en el chat: la historia en alemán, la traducción debajo, y la tabla del
vocabulario nuevo con `cefr` y traducción. Decir cuántas notas se han creado y en
qué carpetas.

**No hacer commit.** Eso es `/de commit`, y solo cuando las cuatro fases estén
hechas. `obsidian-git` va commiteando por su cuenta de todas formas; el commit de
la fase 5 es el que marca la lección cerrada.

## Lo que no hace esta fase

- No pide que hable ni que produzca nada. Es input puro y en silencio.
- No genera ningún GPT. El primero lo genera `/de studium`.
- No emite bloque de cierre: yo escribo las notas directamente, no hay nada que
  parsear.
