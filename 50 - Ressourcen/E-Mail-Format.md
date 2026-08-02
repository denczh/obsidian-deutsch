---
type: reference
---

# El formato de la lista

Es la interfaz entre la sesión de voz y la bóveda, y el texto de mayor
apalancamiento de todo el sistema. Plano, delimitado, sin prosa, sin markdown:
diseñado para que un script lo pueda parsear y para que nada se rompa si pasa
por un cliente de correo.

## El bloque del modo Sprechen

```
=== SESSION ===
date: YYYY-MM-DD
level: L1
mode: sprechen
themes: reisen, gesundheit

=== VOCAB ===
term | article | inflection | pos | translation
Bahnsteig | der | -e | noun | andén
umsteigen | - | steigt um, stieg um, ist umgestiegen | verb | hacer transbordo

=== EXTRA ===
term | article | inflection | pos | translation
verpassen | - | verpasst, verpasste, hat verpasst | verb | perder (un tren)
pünktlich | - | - | adj | puntual
meistens | - | - | adv | la mayoría de las veces

=== GRAMMAR ===
Perfekt mit sein | verbs
Wechselpräpositionen mit Akkusativ | prepositions

=== ERRORS ===
what I said | correction | type
ich bin gegangen zum Bahnhof | ich bin zum Bahnhof gegangen | word-order

=== END ===
```

## El bloque del modo Wiederholung

Cada modo emite el mismo formato, pero solo los bloques que le corresponden. El
repaso no introduce vocabulario, así que no lleva `VOCAB` ni `EXTRA`; en cambio
lleva `OK`, que ningún otro modo produce. Ver [[Modi]].

```
=== SESSION ===
date: YYYY-MM-DD
level: L1
mode: wiederholung
themes: -

=== OK ===
item | type
Possessivartikel meine | grammar
warten | vocab

=== ERRORS ===
what he wrote | correction | type
ich helfe dich | ich helfe dir | case

=== END ===
```

**`OK` es la única puerta de salida de la cola de repaso.** Un elemento en
`learning` que aparece aquí pasa a `known` y desaparece de
[[Schwachstellen.base|Schwachstellen]]; uno en `new` pasa a `learning` y
desaparece de [[Nicht gesprochen.base|Nicht gesprochen]]. `error_count` no se
toca nunca: es el registro histórico de cuánto costó algo.

Por eso el prompt del repaso insiste tres veces en que **si hubo pista, no cuenta**.
Ese bloque escribe ascensos en la bóveda; un modelo generoso te daría por sabido
lo que no sabes.

**La línea `mode:`** es lo que permite saber después de dónde vino cada error. Si
un día `Schwachstellen` se llena de cosas que solo fallo por escrito, eso es
información, no ruido.

## Los tipos de error

El campo `type` de `ERRORS` era texto libre, y eso significa que a la décima
sesión habrá `word-order`, `wordorder` y `orden de palabras` conviviendo. Es un
campo que quiero poder filtrar, así que lleva vocabulario cerrado:

| `type` | Qué es |
|---|---|
| `article` | artículo equivocado o ausente |
| `gender` | género del sustantivo |
| `case` | caso equivocado tras preposición o verbo |
| `agreement` | concordancia: posesivo, adjetivo, plural |
| `word-order` | posición del verbo, orden de la subordinada |
| `verb-form` | conjugación, auxiliar, participio |
| `preposition` | preposición equivocada |
| `vocabulary` | palabra equivocada o inventada |
| `pronunciation` | pronunciación |
| `comprehension` | **no lo entendí.** Solo lo produce [[Modus - Hören\|Hören]] |

`comprehension` es de otra naturaleza que los demás y por eso merece la pena
separarlo: *no entendí* y *lo dije mal* son problemas distintos y se arreglan de
forma distinta. Si `Schwachstellen` se llena de `comprehension`, el diagnóstico es
que el input va demasiado rápido, no que falte vocabulario.

Amplía la lista cuando aparezca algo que no encaje, pero añádelo aquí primero.

## Los dos bloques de vocabulario

**`VOCAB` es lo que pasó en la conversación.** Palabras que el tutor introdujo en
voz alta, palabras que produje yo, palabras que pregunté. Nada más.

**`EXTRA` es criterio docente.** Palabras que no se dijeron pero que pertenecen a
la lección de hoy: el verbo que va con esos sustantivos, el adverbio que habría
hecho natural mi frase, el contrario del adjetivo que usé. Se estudian offline
desde las notas, así que no hacen falta dichas, hacen falta útiles.

La separación no es burocracia. Dentro de tres semanas, *lo que llegué a decir* y
*lo que me regalaron* son datos distintos: el primero mide mi producción, el
segundo mi deberes pendientes. Si van juntos, pierdo los dos.

Un elemento aparece en un bloque o en el otro, nunca en los dos. `EXTRA` tiene
exactamente las mismas cinco columnas y no tiene techo de cantidad; el límite de
6-10 palabras es solo para lo que se dice en voz alta.

En Obsidian se distinguen por el campo `source`: `voice-session` para VOCAB,
`tutor-extra` para EXTRA. La vista [[Nicht gesprochen.base|Nicht gesprochen]]
lista lo añadido que aún no he usado nunca.

## Categorías y flexión

`pos` es uno de: `noun`, `verb`, `adj`, `adv`, `prep`, `conj`, `pron`, `num`,
`phrase`.

**Nada de listas de solo sustantivos.** Son lo más fácil de listar y lo menos
útil de tener. El campo `inflection` se rellena según la categoría:

| Categoría | Qué va en `inflection` |
|---|---|
| `noun` | el plural: `-e`, `-en`, `Häuser`, o `-` si no tiene |
| `verb` | 3ª persona si es irregular, Präteritum, y Perfekt **con su auxiliar**: `fährt, fuhr, ist gefahren`. Separables partidos: `räumt auf, räumte auf, hat aufgeräumt` |
| `adj` | comparativo y superlativo solo si son irregulares: `besser, am besten` |
| todo lo demás | `-` |

El campo `article` es `-` para todo lo que no sea sustantivo. El término va
desnudo: `Bahnsteig`, nunca `der Bahnsteig`.

## Reglas, y hay que enunciarlas como reglas

- Texto plano. Sin negritas, sin tablas markdown, sin viñetas, sin numeración.
- El carácter `|` nunca aparece dentro de un campo.
- Un campo que no aplica se escribe como un solo guion.
- Una línea por elemento. Sin líneas en blanco dentro de un bloque.
- Todos los encabezados del modo, siempre presentes, aunque el bloque esté vacío.
  Cinco en `sprechen`, tres en `wiederholung`.
- La línea `mode:` siempre, y con el token exacto del modo.
- Nombres de campo sin acentos ni caracteres especiales, aunque el contenido sí
  los lleve. Así las claves de parseo sobreviven a cualquier codificación.

## Por qué el bloque ERRORS no es opcional

El vocabulario y la gramática hacen crecer el sistema. Los errores lo hacen
mejorar. Sin log de errores acumulas notas que nunca revisitas y la sección de
errores recurrentes del perfil se queda vacía para siempre. Este bloque es lo
que alimenta [[Schwachstellen.base|Schwachstellen]], y Schwachstellen es lo que
realmente estudias.

## Un campo que quizá quieras añadir

La lista va desnuda a propósito: el enriquecimiento es deliberado y ocurre al
subir las notas. La excepción a tener presente: la traducción y la flexión las
puedes buscar después; la frase que estabas diciendo cuando fallaste la palabra
no la puedes reconstruir. Si te encuentras sin recordar por qué una palabra está
en la lista, añade un quinto campo de contexto y acepta un bloque algo más largo.
