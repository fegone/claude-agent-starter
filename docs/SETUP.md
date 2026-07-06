# Setup — de cero a equipo de agentes funcionando

> Guía paso a paso. Si eres un agente de IA montando esto para tu usuario: sigue los pasos en orden y NO te saltes el onboarding (paso 3) — las respuestas del dueño determinan el roster y los modelos.

---

## Requisitos

- [Claude Code](https://claude.com/claude-code) instalado (CLI, app de escritorio o extensión de IDE)
- Git y una cuenta de GitHub (opcional pero recomendado)

## Paso 1 — Clonar y copiar el template

```bash
git clone https://github.com/fegone/claude-agent-starter.git
cp -R claude-agent-starter/sample-project ~/proyectos/mi-app
cd ~/proyectos/mi-app
```

## Paso 2 — Abrir Claude Code

```bash
claude
```

## Paso 3 — Dejar que corra el onboarding

El `CLAUDE.md` del template detecta sus propios `{{placeholders}}` y ejecuta el flujo automáticamente:

1. **Entrevista** — nombre, descripción, tipo de proyecto, stack, modelo de negocio, usuarios, idioma de la UI, **dominio delicado** (dinero/leyes/salud — cambia modelos y crea agente experto con VETO), web pública (activa SEO), canales.
2. **Roster** — copia de `agent-templates/` a `.claude/agents/` SOLO los agentes que aplican, llenando todos los placeholders con tu contexto real.
3. **Documentación** — genera PRD, TTD, SPRINT (Sprint 0), CHANGELOG y ajusta el SOP.
4. **Memoria** — siembra la primera memoria del proyecto y su índice.
5. **Git** — verifica `.gitignore`, `git init`, commit inicial, y pregunta si crear repo en GitHub.
6. **Auto-limpieza** — borra la sección de onboarding del CLAUDE.md al terminar.

> 💡 Si prefieres hacerlo manual, abre el CLAUDE.md y sigue los pasos del onboarding tú mismo.

## Paso 4 — Instalar el banco de skills recomendado

Ver [SKILLS-CATALOG.md](SKILLS-CATALOG.md). Mínimo recomendado: el plugin **Superpowers** (disciplina de ingeniería: TDD, debugging sistemático, verificación) — es el que más calidad agrega.

## Paso 5 — Primera tarea real

Pide algo concreto y observa el sistema:

```
"Implementa el registro de usuarios con email y contraseña"
```

Deberías ver: el Orquestador planifica → despacha a WebDev → Tester escribe tests → Security audita → Code Reviewer revisa contra el plan → merge. Si el proyecto es chico, varios roles los ejecuta la misma sesión — la estructura es la misma.

## Paso 6 — Cerrar la sesión bien

Al terminar de trabajar, di:

```
"guarda todo"
```

Y verifica que pase: memoria escrita → CLAUDE.md actualizado (estado nuevo, viejo comprimido) → HANDOFF con pendientes → commit + push → reporte de 1 línea por archivo. Ese cierre es lo que hace que la próxima sesión arranque sabiendo todo.

---

## Estructura final de tu proyecto

```
mi-app/
├── CLAUDE.md              ← config viva del proyecto (se mantiene ≤ ~12K tokens)
├── .claude/
│   ├── settings.json      ← permisos (deny de comandos destructivos incluido)
│   ├── AGENT-PROTOCOL.md  ← reglas de coordinación
│   ├── HANDOFF.md         ← pendientes entre sesiones
│   ├── SECURITY-LOG.md    ← historial de auditorías
│   ├── agents/            ← TUS agentes vivos (solo los que aplican, llenos)
│   ├── agent-templates/   ← banco restante (borrable post-onboarding)
│   ├── skill-templates/   ← ídem
│   └── reports/           ← 1 reporte por agente por tarea
├── docs/
│   ├── PRD.md             ← visión y funcionalidades por fase
│   ├── TTD.md             ← arquitectura, stack, schema, endpoints
│   ├── SOP.md             ← procesos: git flow, deploys, seguridad
│   ├── SPRINT.md          ← sprint actual
│   └── CHANGELOG.md       ← historial por versión
└── (tu código)
```

## Personalización típica

| Quieres... | Toca... |
|---|---|
| Agregar un agente que no está en el banco | Crea `.claude/agents/<rol>.md` siguiendo la anatomía de [AGENTS-GUIDE.md](AGENTS-GUIDE.md) |
| Cambiar el modelo de un agente | Campo `model:` en su frontmatter (asignar por costo del error) |
| Endurecer permisos | `.claude/settings.json` → `permissions.deny` |
| Reglas de negocio propias | Sección "Reglas Críticas" del CLAUDE.md |
| Un experto de dominio (fiscal/legal/salud) | Créalo a mano con el modelo MÁXIMO y dale VETO en su área |

## Problemas comunes

| Síntoma | Causa | Fix |
|---|---|---|
| Un agente no encuentra herramientas MCP | Línea `tools:` en su frontmatter | Borrarla (hereda todo) o listar los `mcp__*` explícitos |
| El Orquestador nunca despacha a un agente | `description:` vaga sin triggers | Agregar triggers literales ("implementa", "endpoint", "migration") |
| Sesiones cada vez más lentas/caras | CLAUDE.md o índice de memoria engordaron | Aplicar la dieta de [MEMORY-SYSTEM.md](MEMORY-SYSTEM.md) |
| Claude "olvidó" lo de ayer | No se cerró con "guarda todo" | Cerrar SIEMPRE con el protocolo; reconstruir el estado y guardarlo ahora |
| Dos agentes se pisaron el código | Paralelo sin aislamiento | Worktrees SIEMPRE para trabajo paralelo ([ORCHESTRATION.md](ORCHESTRATION.md)) |
