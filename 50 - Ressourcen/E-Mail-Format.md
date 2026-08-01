---
type: reference
---

# El formato de la lista

Es la interfaz entre la sesión de voz y la bóveda, y el texto de mayor
apalancamiento de todo el sistema. Plano, delimitado, sin prosa, sin markdown:
diseñado para que un script lo pueda parsear y para que nada se rompa si pasa
por un cliente de correo.

```
=== SESSION ===
date: YYYY-MM-DD
level: L1
themes: reisen, gesundheit

=== VOCAB ===
term | article | inflection | pos | translation
Bahnsteig | der | -e | noun | andén
umsteigen | - | stieg um, umgestiegen | verb | hacer transbordo

=== GRAMMAR ===
Perfekt mit sein | verbs
Wechselpräpositionen mit Akkusativ | prepositions

=== ERRORS ===
what I said | correction | type
ich bin gegangen zum Bahnhof | ich bin zum Bahnhof gegangen | word-order

=== END ===
```

## Reglas, y hay que enunciarlas como reglas

- Texto plano. Sin negritas, sin tablas markdown, sin viñetas, sin numeración.
- El carácter `|` nunca aparece dentro de un campo.
- Un campo que no aplica se escribe como un solo guion.
- Una línea por elemento. Sin líneas en blanco dentro de un bloque.
- Los cuatro encabezados de bloque siempre presentes, aunque el bloque esté vacío.
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
