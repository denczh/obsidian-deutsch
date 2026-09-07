---
type: dashboard
---

# Dashboard - Deutsch

**Lección en curso:** L001 → [[Lektionen]] · [[Lernprofil]]

> **[[Lektionen]]** — el ciclo de cinco fases con sus comandos `/de`. Es el
> sistema actual. [[Workflow]] describe el modo de conversación libre, que sigue
> vivo pero fuera del ciclo.

## Vistas

| Vista | Para qué |
|---|---|
| [[Schwachstellen.base\|Schwachstellen]] | Mi cola de repaso. La vista más útil del sistema. |
| [[Aktuelle Lektion.base\|Aktuelle Lektion]] | El vocabulario de la lección en curso. |
| [[Nach Niveau.base\|Nach Niveau]] | Todo agrupado por dificultad CEFR de la palabra. |
| [[Nach Thema.base\|Nach Thema]] | Sustituye por completo a una carpeta por tema. |
| [[Nicht in Anki.base\|Nicht in Anki]] | Cola de exportación a repetición espaciada. |
| [[Ohne Beispiel.base\|Ohne Beispiel]] | Notas creadas con prisa y nunca terminadas. |
| [[Nicht gesprochen.base\|Nicht gesprochen]] | Palabras que no he dicho nunca: añadidas por el tutor o por mí. |

## El ciclo

```
/de lektüre    Claude escribe la historia y crea las notas       (en casa)
/de studium    GPT de vocabulario, tres modos                    (móvil)
/de vorlesen   GPT que lee la parte 2 y pregunta                  (móvil)
/de gramatik   GPT de traducción hablada                          (móvil)
/de commit     Claude procesa, commitea y archiva la lección     (en casa)
```

Cada comando comprueba que el anterior está superado, y "superado" significa que
existe su bloque de cierre. Detalle en [[Lektionen]].

## Operativa

- [[Workflow]] — **el proceso completo**, de la calle a la bóveda y de vuelta
- [[Lektionen]] — **el ciclo de lecciones y los cinco comandos**
- [[E-Mail-Format]] — el formato exacto de la lista de cierre, y los tipos de error
- [[Verarbeitung]] — los 6 pasos de lista → notas, el mismo día
- [[Niveaus]] — los doce tokens CEFR y quién los asigna
- [[Themenliste]] — los temas disponibles, como etiquetas

## Fuera del ciclo

| | Prompt | Dónde |
|---|---|---|
| [[Modus - Sprechen\|Sprechen]] | conversación libre caminando | GPT permanente |

Es lo único que sobrevive de la arquitectura de modos: no es ninguna de las cinco
fases y es lo único que es de verdad conversación. [[Modi]],
[[Modus - Hören|Hören]] y [[Modus - Wiederholung|Wiederholung]] quedan
sustituidos por el ciclo.

## Estructura

- `10 - Sitzungen/` una nota por sesión, con el bloque crudo como procedencia
- `10 - Lektionen/` una nota por lección: historia, fases y bloques crudos
- `20 - Wortschatz/<cefr>/` una nota por palabra, carpeta = dificultad CEFR
- `30 - Grammatik/<cefr>/` una nota por regla, etiqueta corta como título
- `40 - Ansichten/` las vistas (Bases)
- `50 - Ressourcen/` todo lo que no es atómico
- `90 - Vorlagen/` cinco plantillas: lección, sesión, gramática, vocabulario, verbos

**La regla única:** cada nota tiene exactamente un hogar. El tipo decide la
carpeta, el `cefr` es subcarpeta del tipo, el tema y la lección son campos y nunca
carpetas.

**`cefr` es la dificultad de la palabra** y decide la carpeta: doce tokens
cerrados. **`lektion` es cuándo entró** y es un campo, nunca una carpeta. No hay
campo `level`. Ver [[Lektionen]].

## Mantenimiento

- Ya no hay que sincronizar nada a mano: los prompts se generan desde la bóveda
  en cada lección. Ese era el coste recurrente del sistema y ha desaparecido.
- Cada domingo: releer el log de errores. Ese es el currículo de la semana.

Los cinco fallos que no dan error están listados al final de [[Workflow]].
