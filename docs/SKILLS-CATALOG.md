# Catálogo de Skills — el banco recomendado y de dónde instalarlo

> Las skills son conocimiento reutilizable que el agente invoca bajo demanda (`/nombre-skill`). Este catálogo documenta el banco curado que usamos en producción: qué hace cada una, por qué vale la pena y de dónde instalarla.
>
> **Por qué un catálogo y no copias:** la mayoría de estas skills son de terceros con su propio repositorio y licencia. Copiarlas aquí crearía duplicados que se pudren (regla de oro del framework) y problemas de atribución. Instálalas de su fuente — se mantienen actualizadas solas.

---

## Cómo se organizan las skills

| Nivel | Dónde viven | Qué va ahí |
|---|---|---|
| **Globales** | `~/.claude/skills/` y plugins | Genéricas, sirven a todo proyecto (debugging, TDD, diseño, PDF...) |
| **De proyecto** | `<proyecto>/.claude/skills/` | Dominio específico del proyecto (parametrizadas en el onboarding) |

**Regla:** las globales se REFERENCIAN desde los agentes del proyecto, nunca se copian adentro.

---

## Núcleo: disciplina de ingeniería (imprescindibles)

Fuente: plugin **Superpowers** (`github.com/obra/superpowers` — MIT). Es el conjunto que más calidad agrega por token: convierte "un chat que ayuda" en un proceso de ingeniería.

| Skill | Qué hace | Cuándo dispara |
|---|---|---|
| `brainstorming` | Explora intención y requisitos ANTES de crear algo | Antes de cualquier trabajo creativo |
| `systematic-debugging` | Diagnóstico por causa raíz antes de proponer fixes | Cualquier bug o comportamiento raro |
| `test-driven-development` | TDD real: test rojo → código → verde | Toda feature o bugfix |
| `verification-before-completion` | Prohíbe decir "listo" sin correr la verificación | Antes de todo claim de éxito |
| `writing-plans` | Planes de implementación multi-paso escritos | Specs de tareas grandes |
| `executing-plans` | Ejecuta planes con checkpoints de review | Al ejecutar esos planes |
| `dispatching-parallel-agents` | Cuándo y cómo paralelizar sub-agentes | 2+ tareas independientes |
| `subagent-driven-development` | Ejecución de planes con agentes independientes | Sprints con fan-out |
| `requesting-code-review` / `receiving-code-review` | Pedir y recibir review con rigor técnico (sin obedecer feedback ciegamente) | Antes de merge / al recibir feedback |
| `using-git-worktrees` | Worktrees aislados para trabajo paralelo seguro | Features que necesitan aislamiento |
| `finishing-a-development-branch` | Cierre estructurado: merge/PR/cleanup | Al terminar una rama |
| `writing-skills` | Meta: cómo escribir skills nuevas que funcionen | Al crear tus propias skills |

## Desarrollo web y calidad

| Skill | Qué hace | Fuente |
|---|---|---|
| `api-design` | Patrones REST: naming, status codes, paginación, versionado | marketplace de plugins de Claude Code |
| `deployment-patterns` | CI/CD, health checks, estrategias de rollback | marketplace |
| `docker-patterns` | Docker/Compose: seguridad, redes, volúmenes | marketplace |
| `e2e-testing` | Playwright: Page Object Model, CI, tests flaky | marketplace |
| `webapp-testing` | Testing interactivo de apps locales con Playwright | Anthropic |
| `security-review` | Checklist de seguridad para auth, inputs, secrets, endpoints | marketplace |
| `credential-scanner` | Escanea el proyecto por credenciales expuestas antes de publicar | marketplace |
| `git-guardrails-claude-code` | Hooks que bloquean comandos git destructivos | marketplace |

## Diseño y frontend

| Skill | Qué hace | Fuente |
|---|---|---|
| `frontend-design` | UI production-grade que evita la estética genérica de IA | Anthropic |
| `web-design-guidelines` | Audita UI contra guías de interfaz web | marketplace |
| `ui-styling` | shadcn/ui + Tailwind + accesibilidad | marketplace (ckm) |
| `design-system` | Tokens de 3 capas, specs de componentes | marketplace (ckm) |
| `brand` | Voz de marca, identidad visual, consistencia | marketplace (ckm) |
| `ui-ux-pro-max` | Inteligencia UI/UX: 50+ estilos, 161 paletas, 99 guías UX | marketplace |

## Producto y planificación

| Skill | Qué hace | Fuente |
|---|---|---|
| `write-a-prd` | PRD por entrevista + exploración del código | marketplace |
| `shape` | PRD completo auto-respondido con best practices (sin entrevista larga) | marketplace |
| `prd-to-plan` | PRD → plan multi-fase con vertical slices (tracer bullets) | marketplace |
| `prd-to-issues` | PRD → issues de GitHub independientes | marketplace |
| `triage-issue` | Bug → causa raíz → issue con plan de fix TDD | marketplace |

## Documentos y datos

| Skill | Qué hace | Fuente |
|---|---|---|
| `pdf` | Leer, crear, combinar, OCR de PDFs | Anthropic |
| `xlsx` | Spreadsheets: leer, crear, fórmulas, limpieza de data | Anthropic |
| `slides` | Presentaciones HTML con Chart.js | marketplace (ckm) |

## Instalación

```bash
# Dentro de Claude Code:
/plugin  →  buscar el marketplace/plugin  →  instalar

# Superpowers (el núcleo de disciplina):
# https://github.com/obra/superpowers — seguir su README

# Las skills de Anthropic vienen en el plugin oficial anthropic-skills
```

## Criterios para curar TU banco (lo aprendido)

1. **Menos y mejores.** 20 skills bien elegidas > 60 que compiten por disparar. Cada skill instalada agrega su descripción al contexto de CADA sesión — el menú tiene costo fijo.
2. **Auditar lo instalado periódicamente:** ¿disparó en el último mes? ¿su descripción colisiona con otra? ¿el frontmatter tiene `tools:` que bloquea MCP? (bug real: una línea `tools:` en el frontmatter de una skill/agente bloquea silenciosamente las herramientas MCP).
3. **Skills de dominio propio = del proyecto**, no globales. La skill de facturación fiscal de TU país no le sirve a tus otros 6 proyectos.
4. **Escribir skills propias solo cuando se repite el dolor.** La 3ra vez que explicas el mismo proceso a un agente → es una skill (`writing-skills` te guía).
5. **Antes de publicar cualquier skill/repo: `credential-scanner`.** Siempre.
