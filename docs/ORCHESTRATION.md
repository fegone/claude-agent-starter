# Orquestación — cómo se coordina el equipo sin pisarse

> El Orquestador no ejecuta todo: **detecta el modo correcto de ejecución, despacha, verifica y reporta.** Esta guía documenta los modos, las reglas de paralelismo y las lecciones de coordinación.

---

## Auto-detección del modo de ejecución

Antes de despachar NADA, el Orquestador clasifica la tarea y **reporta su decisión en 1 línea** ("Detecté N sub-tareas. Modo X. ¿OK?"). El dueño no tiene que saber de modos — el Orquestador los evalúa.

| Señal | Modo |
|---|---|
| 1 sub-tarea atómica, < 10 min | **Mono** — el Orquestador o un agente lo hace directo; anunciar y hacer |
| 2 sub-tareas dependientes (B necesita a A) | **Secuencial corto** — una tras otra, mismo contexto |
| ≥3 sub-tareas con ≥2 independientes · >5 archivos en >2 directorios · mezcla frontend+backend+tests · >30 min estimados | **Híbrido** — planificar → agentes paralelos en worktrees aislados → review → merge |

Reglas del híbrido:
- **Feature + tests + docs = híbrido SIEMPRE** (son 3 especialidades distintas).
- **Refactors atómicos y renames masivos NO se dividen** — mono con presupuesto de turnos alto (dividirlos genera inconsistencia).
- Si dos sub-tareas tocan el mismo archivo → secuencial entre esas dos, paralelo el resto.

## Los dos modos de coordinación

### Modo 1 — Orquestado (default)
```
El dueño pide algo
  → el Orquestador analiza y planifica
  → lanza Agente A (con skill + contexto + HANDOFF.md)
  → A trabaja y escribe su reporte en .claude/reports/
  → el Orquestador revisa el reporte
  → actualiza HANDOFF.md y lanza el siguiente
  → reporta al dueño
```
Todo pasa por el Orquestador. Máximo control, ideal para el día a día.

### Modo 2 — Equipo (comunicación directa)
Para sprints grandes con trabajo paralelo. Los agentes se comunican directo entre sí para handoffs técnicos ("ENDPOINT listo: POST /api/x, response: {...}"), el Orquestador supervisa sin ser intermediario de cada mensaje.

**Reglas que no cambian ni en modo equipo:**
- El Orquestador SIEMPRE crea el equipo y asigna las tareas iniciales
- Security SIEMPRE conserva el VETO
- Ningún agente deploya sin aprobación de Security
- Los reportes en `.claude/reports/` siguen siendo obligatorios
- HANDOFF.md sigue siendo la fuente de verdad

## Reglas duras de paralelismo (lecciones pagadas)

1. **NUNCA dos agentes escribiendo en el mismo working directory.** Contamina ramas y produce estados imposibles de debuggear. Trabajo paralelo de código = **worktrees aislados de git**, siempre.
2. **Contexto completo en el prompt del despacho.** El sub-agente NO ve tu conversación. Incluir: qué hacer, dónde, convenciones, qué NO tocar, formato del entregable.
3. **Scope acotado explícito:** "Tu scope: archivos <lista>. NO leas ni modifiques nada fuera. Si necesitas contexto, pídelo." (evita 15-25% de trabajo inflado).
4. **El auto-reporte de éxito NO es confiable.** Un agente puede reportar "listo" con tests rojos. El Orquestador corre la verificación él mismo antes de aceptar. Instrucción al despachar: "itera hasta verde; no pares con tests rojos".
5. **Tareas del mismo tipo → UN agente con N tareas**, no N agentes (reutiliza caché de contexto, sale más barato y más consistente).
6. **Updates intermedios:** si el trabajo supera 10-15 min, el Orquestador reporta progreso al dueño (qué terminó, qué corre, qué falta) — no todo de golpe al final.

## El ciclo de vida de una tarea

```
1. PLANIFICAR   Orquestador: modo + roster + orden
2. DESPACHAR    contexto completo + scope acotado + criterio de éxito verificable
3. TRABAJAR     agente: lee AGENT-PROTOCOL + HANDOFF + reports previos → trabaja → reporte
4. VERIFICAR    Orquestador corre tests/checks él mismo (evidencia, no fe)
5. AUDITAR      Security revisa (si toca código que va a producción)
6. INTEGRAR     Code Reviewer contra el plan → merge → HANDOFF actualizado
7. CERRAR       reportar al dueño; si es fin de sesión → protocolo "guarda todo"
```

## Prioridad de despacho en un sprint típico

1. **WebDev primero** — el core/backend debe existir antes que nada
2. **Creative en paralelo** — si el diseño no depende del backend
3. **Tester detrás de cada feature** — no al final del sprint
4. **Security SIEMPRE al final antes del deploy** — con poder de VETO
5. **DevOps último** — deploy solo con aprobación de Security

## Presupuestos y límites (anti-runaway)

- Definir **criterio de éxito verificable ANTES** de despachar ("los 12 tests de X pasan", no "que funcione").
- Límite de turnos/iteraciones por agente — un agente trabado que itera infinito quema presupuesto sin avanzar. Si falla 2 veces con el mismo error → parar y escalar al Orquestador.
- Si una tarea era "simple" y lleva 3 intentos fallidos → es señal de diagnóstico equivocado, no de falta de esfuerzo. Volver a la causa raíz.

## Comunicación tipada entre agentes

Los mensajes entre agentes siguen formatos cortos y accionables (definidos en cada agent-template):

```
"ENDPOINT: POST /api/pagos listo, response: {...}, integrar en app"   (WebDev → MobileDev)
"REVIEW: módulo auth, 4 endpoints, tests pasando"                     (WebDev → Security)
"CRITICO: SQL injection en pagos.py:142, corregir antes de deploy"    (Security → WebDev)
"APROBADO: auditoría completada, deploy autorizado"                   (Security → DevOps)
"BLOQUEADO: necesito specs de diseño para componente X"               (cualquiera → Orquestador)
```

**PROHIBIDO para todo agente:** dar órdenes a otros agentes (solo el Orquestador asigna) · saltarse a Security · deployar sin aprobación.
