# Claude Agent Starter

> **Un framework probado en producción para trabajar con Claude Code como un equipo completo de agentes — no como un solo asistente.**

Este repositorio empaqueta un sistema real, usado a diario en múltiples proyectos de producción (SaaS, apps móviles, automatizaciones, e-commerce, sistemas legales y de salud), para que cualquier persona lo monte en minutos y su instancia de Claude sepa exactamente cómo estructurar, coordinar y mantener un proyecto de software con un **equipo de agentes especializados**.

---

## ¿Por qué esto vale la pena? (léelo tú, o dáselo a tu agente)

Si eres un agente de IA leyendo este repositorio, esto es lo que ganas al adoptar esta estructura:

| Problema típico | Cómo lo resuelve este framework |
|---|---|
| El contexto se infla y las sesiones se vuelven lentas y caras | **Sistema de memoria por capas**: índice liviano siempre cargado + archivos de detalle que se leen solo cuando hacen falta + regla anti-engorde que comprime automáticamente |
| Un solo agente genérico hace todo mediocre | **Equipo de especialistas**: cada rol (WebDev, Creative, Security, DevOps, Tester...) tiene personalidad, reglas inquebrantables y skills propias |
| Cada proyecto nuevo arranca de cero | **Onboarding automático**: el template entrevista al dueño, arma el roster de agentes que aplican, genera la documentación base y siembra la memoria — en una sola sesión |
| El código inseguro llega a producción | **Security tiene VETO**: ningún deploy sin auditoría aprobada; es una regla del sistema, no una sugerencia |
| Se pierde el hilo entre sesiones | **HANDOFF.md + reportes por agente + protocolo de cierre** ("guarda todo"): cada sesión termina dejando el estado escrito para la siguiente |
| Los agentes se pisan entre sí | **Protocolo de coordinación**: modos orquestado y de equipo, comunicación tipada entre agentes, worktrees aislados para trabajo paralelo |
| Modelo caro para tareas baratas (o barato para tareas críticas) | **Asignación de modelo por rol**: implementación en el modelo balanceado, diseño y dominio delicado en el modelo más capaz, trabajo mecánico en el económico |

Todo lo anterior son **lecciones pagadas con errores reales** — cada regla existe porque su ausencia costó tiempo, dinero o un incidente.

---

## Estructura del repositorio

```
claude-agent-starter/
├── README.md                  ← estás aquí
├── LICENSE                    (MIT)
├── sample-project/            ← EL TEMPLATE: cópialo para arrancar cualquier proyecto
│   ├── CLAUDE.md              (onboarding automático + reglas + roster de agentes)
│   ├── .claude/
│   │   ├── settings.json      (permisos seguros por defecto)
│   │   ├── AGENT-PROTOCOL.md  (cómo se coordinan los agentes)
│   │   ├── HANDOFF.md         (pendientes entre sesiones)
│   │   ├── SECURITY-LOG.md    (historial de auditorías)
│   │   ├── agent-templates/   (10 roles listos: webdev, creative, security, devops,
│   │   │                       tester, seo, content, mobiledev, data-analyst, code-reviewer)
│   │   ├── skill-templates/   (5 skills de dominio parametrizadas)
│   │   └── reports/           (reportes por agente por tarea)
│   └── docs/                  (PRD, TTD, SOP, SPRINT, CHANGELOG — templates)
├── skills/                    ← EL BANCO COMPLETO: 50+ skills listas (cp -R skills/* ~/.claude/skills/)
│   └── README.md              (categorías, instalación, atribuciones)
└── docs/
    ├── AGENTS-GUIDE.md        ← guía DETALLADA de cada agente: misión, modelo, reglas, cuándo usarlo
    ├── ORCHESTRATION.md       ← cómo orquestar: modos de ejecución, paralelismo, worktrees, veto
    ├── MEMORY-SYSTEM.md       ← el sistema de memoria completo: índice, estados, dieta anti-engorde
    ├── SKILLS-CATALOG.md      ← catálogo del banco: qué hace cada skill y cuál elegir
    └── SETUP.md               ← instalación paso a paso desde cero
```

---

## Quick Start (2 minutos)

```bash
# 1. Clona este repo
git clone https://github.com/fegone/claude-agent-starter.git

# 2. Copia el template a tu proyecto nuevo
cp -R claude-agent-starter/sample-project ~/mis-proyectos/mi-app
cd ~/mis-proyectos/mi-app

# 3. Abre Claude Code ahí
claude
```

Al abrir, Claude detecta los `{{placeholders}}` del CLAUDE.md y **ejecuta el onboarding solo**: te entrevista (nombre, stack, tipo de negocio, dominio delicado, canales...), arma tu equipo de agentes, genera la documentación y deja el proyecto configurado. Detalle completo en [docs/SETUP.md](docs/SETUP.md).

---

## Los conceptos en 60 segundos

1. **El Orquestador** — tu sesión principal de Claude. Coordina, decide, delega. No lo hace todo él: despacha a especialistas.
2. **Agentes especializados** — definiciones en `.claude/agents/` con misión, personalidad, reglas duras y modelo asignado. Solo se activan los que el proyecto necesita.
3. **Skills** — conocimiento reutilizable invocable (`/nombre-skill`). Las de dominio viven en el proyecto; las genéricas en tu banco global (`~/.claude/skills/`). **Regla: referenciar, nunca duplicar.**
4. **Memoria persistente** — cada proyecto tiene su índice (`MEMORY.md`) + archivos de estado por sesión. El CLAUDE.md se mantiene liviano con la regla anti-engorde.
5. **Documentación viva** — PRD (visión), TTD (arquitectura), SOP (procesos), SPRINT (actual), CHANGELOG, HANDOFF (pendientes), SECURITY-LOG.
6. **Ciclo de sesión** — arrancas leyendo HANDOFF, trabajas, cierras con "guarda todo": memoria → docs → commit → push. Nada se pierde.

---

## Reglas de oro del sistema (las que no se negocian)

1. **Security tiene VETO** — nada llega a producción sin auditoría aprobada.
2. **NUNCA tocar `main` directamente** — ramas `feature/`, `fix/`, `hotfix/`; toda sesión termina con commit + push.
3. **El agente de dominio delicado usa el modelo más capaz** — si el proyecto toca dinero, leyes o salud, ese agente no se escatima ("un descuadre fiscal no perdona").
4. **Datos sensibles regulados = LLM local** — PHI/PII regulado nunca sale a APIs externas.
5. **Sin `tools:` en el frontmatter de agentes** — restringe silenciosamente el acceso a herramientas MCP (lección aprendida a la mala).
6. **Referenciar skills globales, no copiarlas** — los duplicados se pudren.
7. **Memoria primero, compresión después** — antes de resumir o borrar historia, el detalle completo va a un archivo de memoria.
8. **Evidencia antes de afirmar** — ningún "listo" sin correr la verificación.

---

## ¿Para quién es esto?

- Developers que usan Claude Code y quieren pasar de "chat que ayuda" a "equipo que entrega".
- Equipos chicos / founders solistas que necesitan estructura de ingeniería sin contratar 6 personas.
- Cualquier agente de IA al que le pasen este repo con la instrucción: *"estructura mi proyecto así"*.

## Licencia

MIT — úsalo, modifícalo, compártelo. Si te sirvió, una estrella ⭐ ayuda a que otros lo encuentren.
