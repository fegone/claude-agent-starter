# Sistema de Memoria — cómo no perder nada sin inflar el contexto

> El problema real de trabajar con agentes en proyectos largos no es la inteligencia — es la **amnesia entre sesiones** y el **engorde del contexto**. Este sistema resuelve ambos con capas y una regla de compresión disciplinada.

---

## Las capas

```
┌─────────────────────────────────────────────────────────┐
│ CAPA 1 — Siempre cargada (barata, liviana)              │
│  · CLAUDE.md del proyecto (≤ ~12K tokens SIEMPRE)       │
│  · MEMORY.md índice (1 línea por memoria, ≤ ~20KB)      │
├─────────────────────────────────────────────────────────┤
│ CAPA 2 — Se carga bajo demanda (el detalle)             │
│  · project_state_YYYY-MM-DD_tema.md  (cierres de sesión)│
│  · reference-*.md   (datos durables: configs, gotchas)  │
│  · feedback-*.md    (correcciones del dueño + el porqué)│
├─────────────────────────────────────────────────────────┤
│ CAPA 3 — Documentación viva del repo                    │
│  · PRD / TTD / SOP / SPRINT / CHANGELOG                 │
│  · HANDOFF.md (pendientes) · SECURITY-LOG.md            │
│  · .claude/reports/ (reportes por agente)               │
└─────────────────────────────────────────────────────────┘
```

**El principio:** lo que se carga en CADA sesión debe ser un índice, no una enciclopedia. El detalle vive en archivos que se abren solo cuando la tarea los necesita.

## Dónde vive la memoria

- **Memoria del agente:** `~/.claude/projects/<ruta-codificada-del-proyecto>/memory/` — `MEMORY.md` (índice) + archivos `.md` individuales.
- **Documentación del repo:** en el proyecto mismo (versionada en git).

## Tipos de archivo de memoria

| Prefijo | Qué guarda | Ejemplo |
|---|---|---|
| `project_state_FECHA_tema.md` | Cierre de sesión: qué se hizo, qué quedó pendiente, decisiones | `project_state_2026-07-06_deploy-modulo-pagos.md` |
| `reference-*.md` | Datos durables: configs, IPs, gotchas técnicos, cómo se arregló algo | `reference-postgres-bind-mount-gotcha.md` |
| `feedback-*.md` | Correcciones del dueño CON el porqué y cómo aplicarlas | `feedback-no-tocar-repo-de-otro-agente.md` |

Cada archivo lleva frontmatter (`name`, `description`, `metadata.type`) y el índice `MEMORY.md` le da **una línea con hook corto** — suficiente para decidir si abrirlo, nunca el contenido completo.

---

## La regla anti-engorde (la más importante)

El CLAUDE.md de un proyecto activo tiende a crecer sin control: cada sesión agrega "estado actual" encima del anterior hasta que carga 50K tokens de historia muerta en cada sesión. La regla:

1. **Solo el ESTADO ACTUAL más reciente vive completo** en el CLAUDE.md.
2. Al cerrar sesión, el estado anterior **baja a 1 línea-puntero** en la sección "Historial de sesiones", apuntando a su `project_state_*.md`.
3. **ANTES de comprimir, el detalle completo se escribe a memoria.** Comprimir sin guardar = perder. El orden es sagrado: memoria primero, compresión después.
4. Umbrales de mantenimiento (chequear en cada cierre):
   - CLAUDE.md > ~48KB → aplicar dieta (verificando bloque por bloque que cada dato esté cubierto en memoria; si falta algo → adenda a memoria PRIMERO; backup del archivo antes de recortar)
   - MEMORY.md > ~20KB → compactar hooks del índice (1 línea por memoria; NUNCA borrar links)

## El protocolo de cierre — "guarda todo"

Cuando el dueño dice "guarda todo" (o cualquier señal de fin de sesión), se ejecuta COMPLETO, sin preguntar archivo por archivo:

1. **Memoria PRIMERO** — `project_state_FECHA_tema.md` con TODO el detalle de la sesión + línea nueva en `MEMORY.md`
2. **CLAUDE.md** — ESTADO ACTUAL nuevo; el anterior baja a 1 línea-puntero (regla anti-engorde)
3. **HANDOFF.md** — pendientes para la próxima sesión
4. **SPRINT.md / CHANGELOG.md / SECURITY-LOG.md** — solo si cambiaron DE VERDAD (no inventar entradas)
5. **Reportes** en `.claude/reports/` si trabajaron sub-agentes
6. **Git commit + push**
7. **Verificación final** — reportar al dueño 1 línea por archivo tocado

> Criterio: tocar solo lo que genuinamente cambió. Una sesión puramente operativa no necesita entrada en CHANGELOG.

## Reglas de higiene

- **Verificar antes de actuar sobre memoria vieja** — una memoria refleja lo que era cierto cuando se escribió. Si nombra un archivo, config o flag, confirmar que sigue existiendo antes de recomendarlo.
- **Actualizar en vez de duplicar** — antes de crear una memoria nueva, buscar si ya existe una que cubra el tema; borrar las que resultaron incorrectas.
- **No guardar lo que el repo ya registra** — estructura del código, historia de git, contenido del CLAUDE.md. La memoria es para lo que NO se deriva del código.
- **Fechas absolutas** — "ayer" no significa nada en 3 semanas. Siempre `2026-07-06`, nunca "hoy".
- **Enlazar memorias relacionadas** — `[[nombre-de-otra-memoria]]` crea el grafo que permite navegar el conocimiento.
