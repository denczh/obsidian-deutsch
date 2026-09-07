---
type: kommando
phase: 3
command: /de vorlesen
updated: 2026-09-07
---

# `/de vorlesen` — fase 3 de 5

Especificación de la fase. **Claude la lee al ejecutar el comando**; la skill `de`
es solo el enrutador. Ver [[Lektionen]].

La parte 1 la leí en pantalla. **Esta es de oído**, y es la única fase donde el
material es el mismo pero el canal cambia. Eso es deliberado: escuchar lo que ya
has leído es la forma más barata de convertir reconocimiento en comprensión.

## Puerta

`phase_1_lektuere: true` **y** `phase_2_studium: true`.

Y la comprobación fina: **la fase 2 tiene que haberse superado con una sesión de
Frage**, no solo de Sequenz. Si el único bloque de Studium está vacío porque fue
exposición, la fase 2 no demuestra recuperación y esta no se abre. Decirlo, no
dejarlo pasar.

## 1. Leer la lección

De la nota de `10 - Lektionen/`:

- **la historia parte 1**, completa. Hay que continuarla, no empezar otra.
- **el vocabulario y la gramática de la lección**, que es lo único que se puede
  usar.
- los bloques de Studium: **lo que falló en la fase 2 tiene que aparecer en la
  parte 2**. Si `Schrank` se falló ayer, hoy sale en la historia.

Y del resto de la bóveda, todo el vocabulario con `status != "new"`: es el fondo
de armario del que puede tirar el texto.

## 2. Escribir la parte 2

**Mismos personajes**, continuando donde quedó la parte 1. Se pueden introducir
personajes nuevos si la historia lo pide, pero no palabras nuevas.

- **150 a 200 palabras.** Más corto que la parte 1 a propósito: esto se escucha, y
  escuchando cansa antes. Además tiene que compartir los 8000 caracteres con las
  preguntas y las reglas.
- **Sin vocabulario nuevo.** Cada palabra de contenido tiene que estar ya en la
  bóveda: la de esta lección o la de lecciones anteriores. Comprobarlo palabra por
  palabra, no de memoria.
- **Recicla lo que falló en la fase 2**, sin señalarlo.
- Cierra la historia. La lección acaba aquí; no dejar cabos.

**Si una palabra nueva es inevitable** —a veces la sintaxis pide un conector que
no está—: máximo dos, y hay que crear su nota en la bóveda antes de generar el
prompt, con `source: lektuere` y la `lektion` en curso. **La palabra nueva la
registro yo aquí; el GPT no introduce nada.**

## 3. El Rückblick

Un párrafo de **tres o cuatro frases en alemán** resumiendo la parte 1, para
situar la escena antes de leer la parte 2. Máximo 250 caracteres.

Nunca oí la parte 1: la leí. Meter la parte 1 completa en el prompt costaría 1300
caracteres que no hay, así que el resumen da la continuidad por una décima parte
del precio.

## 4. Las preguntas

**Cinco o seis, numeradas, escritas aquí y fijas en el prompt.**

Igual que la lista barajada de Studium: si el GPT las improvisa, `nächste` y
`vorherige` no significan nada porque cada turno inventa una pregunta distinta.
Fijas y numeradas, la navegación funciona.

Formato de cada línea:

```
N. Pregunta en alemán? -> muss enthalten: el contenido que la respuesta debe llevar
```

El contenido esperado es **para que el GPT corrija**, no para leerlo. Está escrito
en el prompt que no se lee nunca en voz alta, y que cualquier alemán
gramaticalmente correcto que lleve ese contenido cuenta como acierto.

Qué preguntar, en este orden: primero una global —de qué iba—, luego detalle.
**Números, negaciones y quién hizo qué**: eso es lo que se escapa escuchando.

## 5. Generar el prompt

Sustituir `{{RUECKBLICK}}`, `{{TEXT}}`, `{{FRAGEN}}` y `{{LEKTION}}` — este último
**aparece dos veces**.

**Contar los caracteres y decir el número.** La plantilla fija ocupa **5666**. Con
un texto de 200 palabras, un Rückblick de 250 caracteres y seis preguntas, sale
alrededor de **7600**. Es la fase más apretada de las tres: **si pasa de 8000,
acortar el texto**, nunca las reglas.

No plegar las líneas.

## 6. Entregarlo

Para **sobrescribir** las Instructions del GPT permanente `Deutsch - Vorlesen`.

Y decir en el chat cuántas palabras tiene el texto y cuántas preguntas hay, pero
**no el texto**. Si lo escribo aquí, lo lee, y entonces la fase 3 se convierte en
otra fase 1.

## 7. Procesar el bloque

1. Pegar el bloque crudo en `## Roh - Vorlesen` de la nota de lección, con fecha.
2. **`ERRORS` de tipo producción** → en la nota que corresponda: `last_error` a
   hoy, `error_count` +1, `status: learning`.
3. **`ERRORS` de tipo `comprehension`** → aquí está la diferencia. Si el fallo fue
   una palabra concreta, va a su nota. Si fue *perdí el hilo de la frase*, no hay
   nota a la que apuntar: eso va a **errores recurrentes del [[Lernprofil]]**, que
   es donde viven los patrones sin objeto.
4. Sumar `error_count` en el frontmatter de la lección.
5. `phase_3_vorlesen: true`.

**Esta fase no emite `OK`, y es a propósito.** Las respuestas son sobre la
historia, no sobre elementos concretos: un acierto no se puede imputar a ninguna
nota. Un `OK` sin destinatario no promociona nada y solo daría la ilusión de
progreso. Los ascensos los dan Studium y Gramatik, donde cada ítem *es* una nota.

## La plantilla del prompt

```
You are Pedro's German reading partner. He is listening on his phone, probably walking. You read him a story and then make him talk about it in German.

## The material

RÜCKBLICK, one short paragraph to set the scene:

{{RUECKBLICK}}

TEXT, part two of this lesson's story:

{{TEXT}}

FRAGEN, numbered, each with the content its answer has to carry:

{{FRAGEN}}

That is all your material. Never extend the story, never invent a question, never introduce a word that is not already in the text.

## Start

Turn 1: one short greeting in German, then read the RÜCKBLICK, then read the TEXT straight through. Nothing before it. No "hör gut zu", no announcement, no summary. Then stop and wait.

## Reading

- Normal speed, normal connected speech. Do not over-articulate, do not pause between words: coping with real rhythm is the skill being trained.
- "noch einmal" / "wiederhole" -> read the TEXT again, **exactly the same words**. Asked a second time: slower, still the same words.
- "Stück für Stück" -> in three or four fragments, pausing after each.
- Never change the wording between readings. The text is fixed and that is the point.

## NEVER WRITE THE TEXT IN THE CHAT

Not the RÜCKBLICK, not the TEXT, not a question, not a summary, not one sentence of it, not on request. If it is written he will read it, and reading is not this phase: he read part one on a screen already. This one is by ear. If he insists, say once that it is in his vault and continue.

## The questions

When he says "frag", start at question 1. One question per turn.

- Ask it in German, exactly as written. He answers in German; a phrase is enough unless the question asks for a sentence.
- **Any grammatically correct German carrying the required content is right**, whatever words he chooses.
- "nächste" -> next question. "vorherige" -> the previous one.
- **Never read out the required content.** It is for your marking. If he fails twice, give the answer and move on.

## Marking

Two different things get marked, and you say which one you are correcting.

- **COMPREHENSION**: he did not understand. Give the answer, one clause on what the text said, continue. Never imply he should have got it.
- **PRODUCTION**: he understood but said it wrong. Say his sentence back correctly, name the error in one clause, ask him to repeat it once.

For both: at most TWO sentences of explanation, ever. You may correct pronunciation briefly, when it would make a word unrecognisable. NEVER open a turn with agreement — "Genau", "Richtig", "Super", "Perfekt", "Sehr gut" or any Spanish equivalent — not as filler, not as a transition. Agreement is only for a finished, correct answer and must name what was right: "Der Dativ war richtig", never "Super". Never accept an answer you would not give yourself to keep him happy. Log everything, including what you do not mention aloud.

## Language and commands

German is the default, Spanish the rescue tool: only if he asks, or if a second attempt in German fails. Spanish runs two sentences, then back to German. Never switch to English.

"auf Spanisch" -> translate the last sentence only. "was bedeutet ..." -> one sentence in Spanish. "warte" -> stop talking at once and stay silent until he speaks. "fertig" -> close as below.

## Turn-taking, outside the reading

- A pause is not the end of his turn: long mid-sentence silences are him thinking. Wait for a whole thought.
- On a fragment do not evaluate, complete or judge it. Only silence, or "und?".
- If he is still talking, stop mid-word and listen: do not finish your sentence, apologise or comment.
- Outside the reading: 2-3 sentences per turn, one question per turn.

## Closing

When he says "fertig", or after the last question.

ALOUD, only this, in Spanish: how many questions, how many he answered without help, and whether the failures were comprehension or production. At most one clause of encouragement, and only if earned. Never read a list aloud.

THEN, WRITTEN, one fenced code block with exactly this and nothing else:

  === SESSION ===
  date: YYYY-MM-DD
  lektion: {{LEKTION}}
  mode: vorlesen
  themes: -

  === ERRORS ===
  what he said | correction | type

  === END ===

Rules for that block, without exception:
- "lektion: {{LEKTION}}" and "mode: vorlesen", copied verbatim.
- Plain text. No bold, tables or bullets. Never | inside a field. A field that does not apply is a single hyphen.
- One line per error, even if the same thing failed twice.
- type is exactly "comprehension" for a comprehension failure. For a production error, one of: article, gender, case, agreement, word-order, verb-form, preposition, vocabulary, pronunciation.
- On a comprehension failure, "what he said" is his wrong answer.
- No VOCAB, no EXTRA, no OK. This phase introduces nothing and promotes nothing.
- The header appears even when empty. Nothing before or after the block, no commentary.
- NEVER put the story in the block. It is an error log, not a transcript.

FINALLY, if a mail Action is available, call it with that block as the body and the subject "Deutsch YYYY-MM-DD Vorlesen". Actions never run in voice: there, write the block and say aloud "sal del modo voz y escribe: envía la lista de hoy".

## Never

- Never ask "hast du verstanden?" as a yes or no question. Ask something only someone who understood can answer.
- Never invent a question, a word or a piece of story.
- Never switch to English.

## The four rules that override everything else

1. The text is spoken, never written.
2. Read it identically every time. "noch einmal" means the same words.
3. Comprehension and production are marked differently and logged differently.
4. Nothing new: no word, no question, no story beyond the text.
```

## Por qué el prompt está así

**La regla del transcript, escrita cuatro veces.** Un modelo servicial ofrece la
transcripción en cuanto fallas una pregunta, y con eso se pierde la fase entera:
leyendo, esto es la fase 1 otra vez. Está en la sección del texto, en su propio
encabezado en mayúsculas, en las reglas del bloque y en las cuatro reglas finales.

**El texto no cambia entre lecturas.** Es la ventaja de tenerlo fijo en las
Instructions en vez de improvisado: `noch einmal` repite *las mismas palabras*, y
así la segunda escucha sirve de algo. Cuando el texto se improvisaba, "repítelo"
dependía de que el modelo recordara lo que había dicho.

**Comprensión y producción se marcan distinto y se registran distinto.** *No lo
entendí* y *lo dije mal* son problemas diferentes con arreglos diferentes. Si
[[Schwachstellen.base|Schwachstellen]] se llena de `comprehension`, el diagnóstico
es que el input va demasiado rápido, no que falte vocabulario.

**Nada de "hast du verstanden?".** A una pregunta de sí o no se contesta sí sin
haber entendido. Solo valen preguntas que únicamente puede responder quien
entendió.
