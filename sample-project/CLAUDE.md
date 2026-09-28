# {{NOMBRE_PROYECTO}} — Proyecto

> **IMPORTANTE — PRIMERA SESION:**
> Si este CLAUDE.md todavia tiene `{{placeholders}}`, es un proyecto nuevo sin configurar.
> Ejecuta el **Onboarding** (abajo) antes de cualquier otra cosa.

---

## Onboarding — Primera Sesion

Cuando detectes `{{NOMBRE_PROYECTO}}` en el titulo, ejecuta este flujo automaticamente:

### Paso 1: Presentacion
> "Veo que este es un proyecto nuevo. Voy a hacerte unas preguntas para configurar todo. Solo toma unos minutos."

### Paso 2: Entrevista (usa AskUserQuestion donde aplique)
1. **Nombre del proyecto**
2. **Descripcion corta** — que hace en 1-2 oraciones
3. **Tipo** — app movil / web app / API / SaaS / automatizacion / scraper / otro
4. **Stack tecnico** — si ya esta definido (lenguaje, framework, DB); si no, proponer
5. **Modelo de negocio** — suscripcion, comision, freemium, interno, etc.
6. **Usuarios principales** — quien lo usa
7. **Problema que resuelve**
8. **Idioma de la UI** — espanol RD / espanol neutro / EN / multi
9. ⚠️ **Dominio delicado** — ¿toca dinero/impuestos (fiscal-contable), leyes (legal), salud (HIPAA/PHI)?
   - Fiscal/contable o legal critico → el agente de dominio va en `claude-opus-5` (regla de oro: "un descuadre fiscal no perdona") y tiene **VETO** en su area
   - HIPAA/PHI o datos sensibles regulados → usar LLM local (ej. Ollama / LM Studio / LiteLLM en tu propia red) para ese dato; NUNCA enviarlo a APIs externas
10. **Web publica / SEO** — ¿habra sitio indexable? (activa agentes SEO)
11. **Canales** — WhatsApp, Email, Push, SMS, Telegram
12. **Algo especial** — reglas, restricciones, integraciones

### Paso 3: Equipo de agentes (banco → proyecto)
Los templates viven en `.claude/agent-templates/` y `.claude/skill-templates/` (NO se cargan como agentes vivos — es a proposito).

1. Segun las respuestas, decide el roster (tabla "Equipo de Agentes" abajo como guia).
2. **Copia** de `agent-templates/` a `.claude/agents/` SOLO los que apliquen, y **llena TODOS los `{{placeholders}}`** con el contexto real del proyecto (mision, stack, paths, convenciones). Igual con `skill-templates/` → `.claude/skills/`.
3. **Reglas duras al generar agentes** (lecciones campaña 2026-07-05/06):
   - ❌ NUNCA linea `tools:` en el frontmatter (restringe y bloquea los `mcp__*` silenciosamente). Si un agente necesita MCP especifico, listar los `mcp__*` explicitos — de resto, omitir y hereda todo.
   - ❌ NUNCA copiar skills/agentes globales al proyecto (dup se pudre; el global se invoca igual desde aqui). Referenciar, no duplicar.
   - ❌ NUNCA dejar un template sin llenar en `.claude/agents/` — lo que no aplica se queda en `agent-templates/`.
   - ✅ Modelos vigentes: `claude-sonnet-5-5` (implementacion) / `claude-opus-5` (orquestacion, creative, dominio delicado). Verificar contra la doc oficial de Anthropic si estos nombres rotaron.

### Paso 4: Documentacion
- Este `CLAUDE.md` — reemplazar todos los `{{placeholders}}`; **borrar esta seccion Onboarding completa** al terminar (self-cleaning)
- `docs/PRD.md`, `docs/TTD.md`, `docs/SPRINT.md` (Sprint 0), `docs/CHANGELOG.md` (v0.0.1), `docs/SOP.md` (ajustar)
- `.claude/HANDOFF.md` + `.claude/SECURITY-LOG.md` — entrada inicial

### Paso 5: Memoria (disciplina desde el dia 1)
1. Escribe la primera memoria: `~/.claude/projects/<dir-codificado-de-este-path>/memory/project_state_<fecha>_onboarding.md` con el resumen de la configuracion (frontmatter: name/description/metadata type: project).
2. Crea/actualiza el `MEMORY.md` indice de ese dir: **1 linea por memoria, hooks cortos** (el indice se trunca al cargar si pasa ~24KB — mantenerlo LEAN siempre).
3. La seccion "ESTADO ACTUAL" de este CLAUDE.md + la regla anti-engorde (abajo) quedan activas desde el inicio.

### Paso 6: Git + GitHub
1. Verificar `.gitignore` PRIMERO (secretos, `.env`, builds, backups)
2. `git init` + commit inicial
3. Preguntar al dueño: ¿repo privado en GitHub ya? → `gh repo create NOMBRE --private --source=. --description "..."` + push
4. Confirmar URL

### Paso 7: Resumen final
Mostrar al dueño: roster de agentes creado, docs generados, memoria sembrada, repo. Preguntar si ajusta algo.

---

## Que es {{NOMBRE_PROYECTO}}
{{DESCRIPCION — se llena en onboarding}}

### Problema que resuelve
{{PROBLEMA — se llena en onboarding}}

### Modelo de negocio
{{MODELO_NEGOCIO — se llena en onboarding}}

---

## ⭐ ESTADO ACTUAL — {{FECHA}}
{{Se llena en cada cierre de sesion. SOLO el estado mas reciente vive aqui completo.}}

## 📜 Historial de sesiones — detalle en memoria (recall por relevancia)

> **Regla de mantenimiento (anti-engorde):** al cerrar sesion ("guarda todo"), el ESTADO ACTUAL nuevo reemplaza al anterior; el anterior baja a **1 linea con puntero** aqui. El detalle completo SIEMPRE va al `project_state_*.md` de memoria ANTES de comprimir. Este archivo se mantiene ≤ ~12K tokens.

- (vacio — se llena con el tiempo)

---

## Stack Tecnico
{{STACK — se llena en onboarding}}

---

## Estructura del Proyecto

| Carpeta | Contenido |
|---------|-----------|
| `docs/` | Documentacion completa (PRD, TTD, SOP, etc.) |
| `.claude/agents/` | Agentes VIVOS del proyecto (solo los que aplican, llenos) |
| `.claude/agent-templates/` + `.claude/skill-templates/` | Banco de templates (no cargan; borrar post-onboarding si se quiere) |
| `.claude/reports/` | Reportes por agente por tarea |

## Documentacion del Proyecto

| Documento | Ubicacion | Que contiene |
|-----------|-----------|--------------|
| **PRD** | `docs/PRD.md` | Vision, funcionalidades por fase, modelo de negocio, riesgos |
| **TTD** | `docs/TTD.md` | Arquitectura, stack, schema DB, endpoints |
| **SOP** | `docs/SOP.md` | Procesos de trabajo, git flow, deploys, seguridad |
| **SPRINT** | `docs/SPRINT.md` | Sprint actual, tareas completadas y pendientes |
| **CHANGELOG** | `docs/CHANGELOG.md` | Historial de cambios por version |
| **HANDOFF** | `.claude/HANDOFF.md` | Pendientes entre agentes/sesiones |
| **SECURITY-LOG** | `.claude/SECURITY-LOG.md` | Auditorias y fixes de seguridad |

---

## Reglas Criticas

### 1. Security tiene VETO
Todo codigo nuevo, dependencia o integracion debe poder pasar auditoria de Security antes de mergear/deployar.

### 2. Git Flow
- NUNCA modificar main directamente — ramas `feature/` `fix/` `hotfix/`
- `git pull` al inicio de cada tarea; TODA sesion termina con commit + push

### 3. Idioma
- Comunicacion con el dueño: {{IDIOMA_COMUNICACION}} · Codigo: ingles · UI: {{IDIOMA_UI}}

### 4. Higiene de agentes/skills (lecciones 2026-07)
- Sin `tools:` en frontmatter (salvo `mcp__*` explicitos) · sin dups de globales · sin templates sin llenar vivos · modelos vigentes

---

## Principios de Codificacion (Karpathy-Inspired)

### 1. Pensar antes de codificar
**No asumir. No esconder la confusion. Sacar a la luz los tradeoffs.**
- Suposiciones explicitas; ante duda, preguntar. Peticion ambigua → presentar interpretaciones. Camino mas simple → decirlo.

### 2. Simplicidad primero
**El minimo codigo que resuelve el problema. Nada especulativo.**
- Cero features/abstracciones/configurabilidad no pedidas. 200 lineas que caben en 50 → reescribir.

### 3. Cambios quirurgicos
**Tocar solo lo necesario. Limpiar solo el desorden propio.**
- No refactorizar lo no-roto. Respetar estilo existente. Codigo muerto ajeno → mencionar, no borrar. Huerfanos propios → limpiar.

### 4. Ejecucion orientada a meta
**Definir criterios verificables. Loop hasta que pase.**
- Definir verificacion ANTES de codear. Correr tests tras el cambio. Sin manera de verificar → decirlo, no afirmar "listo". Falla → causa raiz, no `--no-verify` ni catch silencioso.

---

## Equipo de Agentes

Roster guia (el onboarding copia SOLO los que aplican desde `agent-templates/`):

| Agente | Rol | Modelo | Aplica si... |
|--------|-----|--------|---|
| **WebDev** | Backend + Frontend implementer | sonnet-5 | siempre (proyectos con codigo) |
| **Creative** | Diseno UI/UX, branding, copy | opus-4-8 | siempre que haya UI |
| **MobileDev** | App movil (Flutter / RN) | sonnet-5 | hay app movil |
| **Security** | Auditoria + compliance — VETO | sonnet-5 | siempre |
| **DevOps** | Deploy e infraestructura | sonnet-5 | siempre que haya deploy |
| **SEO** | Coordinador SEO/GEO/AI-search | sonnet-5 | hay web publica |
| **Code Reviewer** | Review contra plan + Karpathy | sonnet-5 | siempre |
| **Tester** | QA (unit/integration/E2E) | sonnet-5 | siempre |
| **Content** | Content writer + AI-citation | sonnet-5 | hay contenido editorial |
| **Data Analyst** | SQL, reportes (SELECT-only) | sonnet-5 | hay DB |
| **{{DOMINIO}}** | Experto dominio delicado (tipo "Alex") — VETO en su area | **opus-4-8** | fiscal/legal/salud (crear a mano, sin template) |

### Agentes globales (en `~/.claude/agents/` — invocables desde cualquier proyecto, NO duplicar)
- `Database Optimizer` · `Security Engineer` · `DevOps Automator` · `Compliance Auditor` (SOC2/ISO/HIPAA/PCI)
- **SEO consolidados (6):** `seo-technical` (crawl+schema+sitemap) · `seo-content` (E-E-A-T+GEO) · `seo-performance` (CWV+visual+imagenes) · `seo-local` (GBP+maps) · `seo-dataforseo` (data live) · `seo-google` (CrUX/GSC/GA4)

---

## Comandos del dueño

### "Guarda todo en memoria" (protocolo de cierre de sesion)
1. **Memoria PRIMERO**: `project_state_<fecha>.md` con TODO el detalle → dir de memoria del proyecto + linea en su `MEMORY.md` (hook corto)
2. **CLAUDE.md**: ESTADO ACTUAL nuevo; el anterior baja a 1 linea-puntero en Historial (regla anti-engorde)
3. HANDOFF.md + SPRINT.md + CHANGELOG.md (+ SECURITY-LOG/PRD/TTD/SOP solo si cambiaron de verdad)
4. Reportes en `.claude/reports/` si hubo sub-agentes
5. Git commit + push
6. Confirmar al dueño: 1 linea por archivo tocado

> No preguntar archivo por archivo. Hacerlo todo y reportar. No inventar entradas en docs que no cambiaron.

## Handoff Protocol
Si un agente depende del trabajo de otro: pausa y avisa.
