# Banco de Skills — 50+ skills curadas y listas para instalar

> Este es el banco completo que usamos en producción, ya auditado y optimizado. Cópialo entero o elige por categoría.

## Instalación

```bash
# Todo el banco a tu instalación global de Claude Code:
cp -R skills/* ~/.claude/skills/

# O selectivo:
cp -R skills/brainstorming skills/systematic-debugging skills/test-driven-development ~/.claude/skills/
```

> ⚠️ **Menos es más:** cada skill instalada agrega su descripción al contexto de cada sesión. Si no vas a usar una categoría (ej. legal o finanzas), no la instales. Ver criterios de curaduría en [docs/SKILLS-CATALOG.md](../docs/SKILLS-CATALOG.md).

## Qué hay (por categoría)

| Categoría | Skills |
|---|---|
| **Disciplina de ingeniería** (el núcleo) | brainstorming · systematic-debugging · test-driven-development · verification-before-completion · writing-plans · executing-plans · dispatching-parallel-agents · subagent-driven-development · requesting-code-review · receiving-code-review · using-git-worktrees · finishing-a-development-branch · using-superpowers · writing-skills · iterative-retrieval |
| **Web y calidad** | api-design · deployment-patterns · docker-patterns · e2e-testing · webapp-testing · security-review · credential-scanner · git-guardrails-claude-code · composition-patterns |
| **Diseño y frontend** | frontend-design · web-design-guidelines · ui-styling · ui-ux-pro-max · design-system · brand · banner-design · huashu-design · slides · video-prompt-builder |
| **Producto y planificación** | write-a-prd · shape · prd-to-plan · prd-to-issues · triage-issue |
| **Documentos y datos** | pdf · xlsx |
| **Legal** | contract-review (+ legal-agents, sus 5 sub-agentes) · legal-agreement · legal-plain · legal-terms |
| **Finanzas/contabilidad** | capital-accounts · fixed-assets · investments |
| **Meta/avanzado** | agent-orchestrator · continuous-learning-v2 · graphify |

## Optimizaciones ya aplicadas a esta copia

- ✅ Duplicados eliminados (había 2 pares repetidos en el banco original)
- ✅ Nombres de frontmatter normalizados (prefijos de marketplace removidos para invocación limpia)
- ✅ Frontmatter agregado donde faltaba
- ✅ Cero `tools:` en frontmatter (bug que bloquea herramientas MCP silenciosamente)
- ✅ Assets pesados removidos (~26MB de audio que no aportaba)
- ✅ Escaneo de secretos y referencias privadas: limpio
- ✅ `ui-ux-pro-max` viene con su expediente de auditoría de seguridad (ver su `PROVENANCE.md`) — los scripts que leían API keys externas fueron removidos en la auditoría

## Atribuciones

Estas skills provienen de proyectos open-source de la comunidad y de Anthropic. Crédito a sus autores:

| Fuente | Skills | Licencia |
|---|---|---|
| [obra/superpowers](https://github.com/obra/superpowers) | El núcleo de disciplina de ingeniería (brainstorming, TDD, debugging, planes, code review, worktrees...) | MIT |
| Anthropic ([anthropics/skills](https://github.com/anthropics/skills)) | pdf · xlsx · frontend-design · webapp-testing | ver repo |
| [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | ui-ux-pro-max (+ bundle: banner-design, brand, design-system, slides, ui-styling) | ver repo |
| Marketplaces de la comunidad de Claude Code | api-design, deployment-patterns, docker-patterns, e2e-testing, security-review, y otras | según cada plugin |
| huashu-design (comunidad) | huashu-design | ver su SKILL.md |

Si eres autor de alguna y quieres un ajuste de atribución o retiro, abre un issue — se atiende de inmediato.
