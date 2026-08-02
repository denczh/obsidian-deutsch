---
type: reference
updated: 2026-08-02
---

# Modos: la academia

Hasta ahora el sistema tenía un solo modo, hablar. Una academia real tiene
speaking, listening, repaso y ejercicios, y no hay razón para renunciar a ellos.

## La decisión de diseño: un modo, no un tema

La tentación es un GPT por tema —vivienda, trabajo, comida— porque el límite de
8000 caracteres invita a partir. **Es el eje equivocado.**

Entre "vivienda" y "trabajo" solo cambia la lista de palabras. Entre *hablar* y
*ejercicios* cambia todo: la longitud del turno, la política de corrección, el
formato de salida, el canal, si hay bloque de cierre. Ocho temas serían ocho
prompts casi idénticos y una regla nueva habría que replicarla ocho veces; cruzar
tema × modo serían cuarenta.

**El tema es contenido y el contenido ya vive aquí.** Se lo digo al empezar; el
turno 1 ya ofrece tres temas de [[Themenliste]].

## El límite de 8000 solo aprieta en voz

El modo voz no puede leer Knowledge files, así que todo lo que el tutor deba
saber va escrito en el prompt. **Los modos de texto sí pueden leer archivos**, y
por eso no llevan bloque *Learner values*: leen la fuente real en vez de una copia
que yo mantengo a mano. Esa es la razón de repartirlos entre dos plataformas.

## Los cinco modos

| Modo | Canal | Dónde vive | Qué necesita | Estado |
|---|---|---|---|---|
| **Sprechen** | voz | GPT | el prompt entero, 7742 car. | funcionando |
| **Hören** | voz | GPT | prompt **inverso**: monólogo permitido | pendiente |
| **Wiederholung** | texto | Project | fichero de export de la bóveda | [[Modus - Wiederholung\|listo]] |
| **Übungen** | texto | Project | mi vocabulario de L1 | pendiente |
| **Diktat** | voz + texto | Project | casi nada | pendiente |

**Sprechen y Hören son GPTs** porque necesitan voz y la Action de correo. Los
otros tres **viven en un Project** con los ficheros de la bóveda subidos: una sola
cosa que mantener y datos de verdad en vez de un bloque copiado.

**Hören es el que más me falta y el más interesante de escribir.** Su prompt es el
opuesto del de hablar: allí se prohíbe monologar y se limita a 2-3 frases, aquí se
necesita justo lo contrario, que cuente algo de treinta segundos y luego pregunte.
Mismas reglas de corrección, política de turno invertida. Y mi perfil dice
comprensión por delante de producción, así que es el hueco real.

## Lo que hace que esto sea una academia y no cuatro juguetes

**Todos los modos escupen el mismo bloque de cierre**, y por tanto alimentan el
mismo `error_count`, la misma [[Schwachstellen.base|Schwachstellen]] y el mismo
criterio de promoción de [[Niveaus]]. Un fallo escribiendo y un fallo hablando
pesan igual.

Lo único que se añade es el campo `mode` en la nota de sesión, para saber de dónde
vino cada error. Si un día `Schwachstellen` está llena de cosas que solo fallo por
escrito, eso es información, no ruido.

## La sección `OK`: la puerta de salida

Diseñar el modo repaso destapó una fuga del sistema original.

**Nada sacaba nunca un elemento de la cola de repaso.** `error_count` solo sube; el
paso 5 del procesado lo incrementa y ningún paso lo baja. Un error de hace tres
meses ya dominado seguiría apareciendo en `Schwachstellen` para siempre, y la cola
crecería sin techo hasta volverse inútil.

Falta un canal para lo que sale **bien**, y el modo repaso es exactamente donde
aparece. De ahí un sexto bloque, solo en `Wiederholung`:

```
=== OK ===
item | type
```

Y dos cambios pequeños que lo cierran:

- **`Schwachstellen` ahora filtra también `status != "known"`.** `error_count` se
  queda como registro histórico —es un hecho y sirve para ordenar por cuánto costó
  algo—; lo que retira un elemento es marcarlo `known`.
- **El paso de procesado del bloque `OK`**: un elemento en `learning` que sale
  correcto pasa a `known`; una palabra sin estrenar que sale correcta pasa a
  `learning` y desaparece de [[Nicht gesprochen.base|Nicht gesprochen]].

Sin esto, el criterio de promoción de [[Niveaus]] —*nada con `error_count` mayor
que 2*— era literalmente inalcanzable.

## Orden de construcción

1. **Wiederholung** primero, porque es el que rinde hoy: hay un error registrado y
   50 palabras sin estrenar. Y es texto, así que no pelea ni con los 8000
   caracteres ni con los cortes de voz.
2. **Übungen** después, reutilizando el mismo Project y el mismo export.
3. **Hören** cuando los dos de texto estén rodados, porque es un GPT nuevo y un
   prompt nuevo.
4. **Diktat** al final, si hace falta. Puede que `Übungen` ya lo cubra.

## Y el tema como unidad

Lo que pedía "una unidad tipo academia" no es un GPT: es **una nota de tema** con
su temario y una lista de qué modos he completado con él. Pendiente, y no urgente
hasta que haya más de un modo funcionando.

> **Cuidado con el nombre.** `L1` ya es el token de nivel, en carpetas, frontmatter
> y bloque de cierre. Un tema-unidad **no** es un nivel: si las dos cosas se llaman
> igual, las vistas se rompen en silencio. Los temas son `wohnen`, `beruf`… y son
> etiquetas.
