---
name: estandarizar-formato-slides
description: >-
  Use when a MAS (Sistemas Multiagentes I) Slidev presentation in .md format is
  ready and needs to be verified and corrected to meet the course standard —
  including missing required slides (Repaso Rápido, Agenda de Hoy, Resumen,
  ¿Preguntas?), inconsistent frontmatter, section separators without subtitles,
  overflow in complex grids, or mixed inline/Tailwind styles. Trigger after
  finishing a slides.md file or when asked to review/standardize a presentation.
---

# Estandarizar Formato de Slides MAS

Revisa y corrige una presentación Slidev de la materia Sistemas Multiagentes I (72.79), asegurando que cumpla la estructura pedagógica obligatoria y el formato visual consistente del curso.

## Principio central

**El criterio de corrección es este skill, no los otros archivos del repositorio.** Los otros slides pueden estar también incompletos. No los uses como referencia.

## Paso 1 — Leer el archivo

Lee el archivo completo `slides/<slug>/slides.md` antes de evaluar nada.

## Paso 2 — Verificar estructura pedagógica obligatoria

Estos 4 elementos son **obligatorios** en todas las presentaciones MAS, en este orden:

```
Portada (ItbaCover)
  ↓
Repaso Rápido: Clase N-1
  ↓
Agenda de Hoy
  ↓
[Contenido de la clase]
  ↓
Resumen                    ← el más frecuentemente ausente
  ↓
¿Preguntas? (ItbaSeparator)
```

Si alguno falta, **crealo**. No es opcional.

### Portada

```markdown
---
layout: ItbaCover
---

# [Título Principal]

## Clase N · [descripción breve]

### Sistemas Multiagentes I · 72.79 · ITBA · 2026-S2

[Párrafo contextual de 1-2 oraciones]
```

### Repaso Rápido

```markdown
# Repaso Rápido: Clase N-1

<div class="text-xs mb-3" style="color:#0055b3">
Pensá la respuesta antes de hacer click para revelarla.
</div>

<div class="qz-item">
  <div class="qz-q">1. ¿Pregunta?</div>
  <v-click>
    <div class="qz-a">Respuesta.</div>
  </v-click>
</div>

<style>
.qz-item { margin-bottom:0.55rem; }
.qz-q { font-size:0.85rem; font-weight:600; color:#003366; }
.qz-a { margin-top:0.35rem; font-size:0.78rem; line-height:1.3; background:#f0f4f9; color:#003366; border-radius:0.4rem; padding:0.5rem 0.7rem; }
</style>
```

Mínimo 2 preguntas. El título **debe** ser exactamente `# Repaso Rápido: Clase N-1` (no "Recapitulación", no variantes).

### Agenda de Hoy

```markdown
# Agenda de Hoy

<v-clicks>

- **Tema 1** — descripción breve
- **Tema 2** — descripción breve

</v-clicks>
```

Título exacto: `Agenda de Hoy` (no "Agenda del Día" ni variantes).

### Resumen

```markdown
# Resumen

<v-clicks>

- Punto clave 1
- Punto clave 2
- Punto clave 3

</v-clicks>
```

4 a 6 puntos. Sintetiza los conceptos centrales de la clase.

### ¿Preguntas?

```markdown
---
layout: ItbaSeparator
---

# ¿Preguntas?

### Federico Castañeda & Ninfa Delgado

**Sistemas Multiagentes I · 72.79 · ITBA · 2026-S2**
```

Debe ser el último slide. Verifica que tenga los **3 elementos**: título `# ¿Preguntas?`, línea de autores `### Federico Castañeda & Ninfa Delgado`, y nombre del curso en negrita. Un slide con solo el título o sin autores está incompleto.

## Paso 3 — Verificar frontmatter YAML

```yaml
---
title: [Título no vacío]
addons:
  - ../templates/itba
theme: default
courseName: Sistemas Multiagentes I (72.79)
layout: ItbaCover
defaults:
  layout: ItbaDefault
---
```

- Indentación: **2 espacios** en `addons` y `defaults`
- `courseName` exacto: `Sistemas Multiagentes I (72.79)`
- `layout: ItbaCover` en el frontmatter global
- `defaults.layout: ItbaDefault`

## Paso 4 — Verificar separadores de sección

Cada separador de sección con número debe tener un subtítulo descriptivo:

```markdown
---
layout: ItbaSeparator
---

# 3 · El Problema

## Por qué los LLM olvidan todo
```

Si falta el `##`, agrégalo con una frase que describa el enfoque de esa sección.

## Paso 5 — Verificar separadores `---`

Cada `---` debe tener una línea en blanco antes y después:

```markdown
[contenido]

---

# Siguiente slide
```

Corrige cualquier `---` pegado a un comentario `-->` o a una línea de contenido.

## Paso 6 — Verificar problemas visuales

| Patrón | Riesgo | Corrección |
|---|---|---|
| Grid con 7+ columnas | Overflow horizontal | Reducir columnas o agregar `overflow-x: auto` |
| `<svg viewBox="0 0 1200 ..."` sin `width:100%` | Distorsión en pantallas menores a 1200px | Agregar `style="width:100%; height:auto"` |
| `transform: scale(0.X)` sobre un diagrama Mermaid | Pixelación | Usar `font-size` en el diagram init en su lugar |

## Paso 7 — Producir el archivo corregido

Aplica todas las correcciones al archivo `slides.md` original. El output es el mismo archivo actualizado, listo para `./slidev.sh dev <slug>`.

## Red flags — DETENTE si pensás esto

| Pensamiento | Realidad |
|---|---|
| "El contenido pedagógico está bien estructurado" | Contenido ≠ estructura obligatoria. Verifica los 4 elementos del Paso 2 igual. |
| "No hay placeholders ni TODOs, parece completo" | Ausencia de errores visibles no es cumplimiento del estándar. Revisa la checklist. |
| "Voy a usar otro slide del repo como referencia" | Los otros también pueden estar incompletos. Este skill es la referencia. |
| "El Resumen y ¿Preguntas? son detalles menores" | Son obligatorios. No son opcionales aunque el contenido sea excelente. |
| "Ya casi está lista, solo una revisión rápida" | "Rápida" no existe. Recorre los 7 pasos. |
