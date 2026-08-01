---
type: dashboard
---

# Dashboard - Deutsch

**Nivel activo:** L1 → [[Lernprofil]] · [[Niveaus]]

## Vistas

| Vista | Para qué |
|---|---|
| [[Schwachstellen.base\|Schwachstellen]] | Mi cola de repaso. La vista más útil del sistema. |
| [[Aktuelles Niveau.base\|Aktuelles Niveau]] | Lo que estoy trabajando ahora. |
| [[Nach Thema.base\|Nach Thema]] | Sustituye por completo a una carpeta por tema. |
| [[Nicht in Anki.base\|Nicht in Anki]] | Cola de exportación a repetición espaciada. |
| [[Ohne Beispiel.base\|Ohne Beispiel]] | Notas creadas con prisa y nunca terminadas. |

## Flujo

```
[1] SESIÓN DE VOZ   caminando, 15-30 min, hablando en alemán
      |             el tutor produce; no lee la bóveda
      v
[2] LISTA           bloque plano y delimitado, sin explicaciones
      |             → 50 - Ressourcen/E-Mail-Format
      v
[3] OBSIDIAN        una nota por palabra o por punto gramatical
                    → 50 - Ressourcen/Verarbeitung
```

La bóveda es la memoria. El tutor es solo un interlocutor.

## Operativa

- [[E-Mail-Format]] — el formato exacto de la lista de cierre
- [[Verarbeitung]] — los 6 pasos de lista → notas, el mismo día
- [[GPT-Anweisungen]] — las Instructions listas para pegar
- [[Themenliste]] — los temas disponibles, como etiquetas

## Estructura

- `10 - Sitzungen/` una nota por sesión, con el bloque crudo como procedencia
- `20 - Wortschatz/<nivel>/` una nota por palabra, título = término desnudo
- `30 - Grammatik/<nivel>/` una nota por regla, etiqueta corta como título
- `40 - Ansichten/` las vistas (Bases)
- `50 - Ressourcen/` todo lo que no es atómico
- `90 - Vorlagen/` las tres plantillas

**La regla única:** cada nota tiene exactamente un hogar. El tipo decide la
carpeta, el nivel es subcarpeta del tipo, el tema es etiqueta y nunca carpeta.

Los niveles son **mi escala** (`L1`, `L2`, …), no la CEFR: crecen conmigo y
las carpetas se crean cuando promociono. Ver [[Niveaus]].

## Mantenimiento

- Cada 4-6 sesiones: actualizar el bloque *Learner values* en el GPT desde
  [[Lernprofil]]. Ocho líneas, dos minutos. Es el coste recurrente del sistema.
- Cada domingo: releer el log de errores. Ese es el currículo de la semana.
