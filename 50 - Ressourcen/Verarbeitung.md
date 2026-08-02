---
type: reference
---

# Procesamiento: lista → bóveda

El mismo día, mientras aún recuerdas la conversación. Veinte minutos de atraso
está bien; una semana de atraso significa que las notas no se escriben nunca.

1. **Crear `10 - Sitzungen/YYYY-MM-DD.md`** desde `V - Sitzung`. Pegar el bloque
   crudo abajo, bajo el encabezado *Roh*. Esa es tu procedencia: si una nota
   luego parece mal, el original está ahí.

2. **Por cada línea VOCAB**, una nota en `20 - Wortschatz/<nivel>/`. La plantilla
   depende de `pos`:
   - `pos: verb` → **`V - Verb`**, con conjugación y régimen de preposiciones.
   - todo lo demás → **`V - Wortschatz`**.

   Rellenar `term`, `article`, `inflection`, `pos`, `translation`, `level` y
   `theme` desde la línea. Después añadir lo único que la lista no trae: **una
   frase de ejemplo en alemán, escrita por ti**. Ese acto de producción vale más
   que la nota.

   Las dos plantillas escriben `type: vocab`. Un verbo no es otro tipo de nota,
   es la misma nota con más campos rellenos.

2b. **Por cada línea EXTRA**, lo mismo, pero con `source: tutor-extra`. Son
   palabras que no dijiste: el ejemplo aquí no es un recuerdo, es un ejercicio.
   Escribir la frase es la primera vez que usas la palabra. Aparecen en
   [[Nicht gesprochen.base|Nicht gesprochen]] hasta que les cambies el `status`.

3. **Por cada línea GRAMMAR**, crear *o actualizar* una nota en
   `30 - Grammatik/<nivel>/`. Las reglas se repiten entre sesiones: busca primero
   si ya existe. Escribir la explicación de 2-3 frases en español y dos ejemplos
   mientras la sesión está fresca; una etiqueta desnuda no te dirá nada en tres
   semanas.

4. **Por cada línea ERRORS**, localizar la nota a la que pertenece y poner
   `last_error` a hoy, incrementar `error_count`, poner `status: learning`. Si no
   existe la nota, crearla: un error es evidencia de que el elemento importa.

5. **Actualizar [[Lernprofil]]**: añadir a errores recurrentes, refrescar temas
   recientes, aplicar cualquier promoción que el tutor haya propuesto.

6. **Commit y push.**

> El paso 4 es el que paga el esfuerzo. Todo lo demás registra lo que pasó; el
> paso 4 es lo que convierte la bóveda en un sistema de repaso.

## Aceleradores, cuando la rutina manual ya sea familiar

- El plugin **Templates** está activado y apunta a `90 - Vorlagen`. Los pasos 2 y
  3 son un comando más escribir.
- Un **script parser** puede convertir el bloque pegado en notas stub con el
  frontmatter ya rellenado, dejándote solo las frases de ejemplo y las
  explicaciones. Merece la pena hacerlo hacia la décima sesión, cuando tu propio
  formato ya se haya estabilizado.

## Exportar a repetición espaciada

Cuando te apetezca: filtra por `anki = false` en
[[Nicht in Anki.base|Nicht in Anki]], exporta `term` y `translation` a CSV, y
voltea el flag. Un chat es una mala herramienta de drill y un buen interlocutor.
Que cada uno haga su trabajo.
