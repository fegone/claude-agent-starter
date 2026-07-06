# {{NOMBRE_PROYECTO}} — Protocolo de Agentes

## Modos de coordinacion

### Modo 1: Orquestado (default)
El Orquestador coordina todo. Cada agente reporta al Orquestador, este pasa contexto al siguiente.

```
El dueño pide algo
    -> el Orquestador analiza y planifica
    -> el Orquestador lanza Agente A (le pasa skill + contexto + HANDOFF.md)
    -> Agente A trabaja y escribe REPORTE en /reports/
    -> el Orquestador revisa el reporte
    -> el Orquestador actualiza HANDOFF.md
    -> el Orquestador lanza siguiente agente
    -> el Orquestador reporta al dueño
```

### Modo 2: Agent Teams (comunicacion directa)
Los agentes se comunican entre si via SendMessage. El Orquestador supervisa pero no es intermediario.
**Usar cuando:** Sprints grandes con trabajo paralelo.

**Reglas del Modo Team:**
- El Orquestador SIEMPRE crea el team y asigna tareas iniciales
- Los agentes pueden comunicarse directo SOLO para handoffs tecnicos
- Security SIEMPRE tiene VETO
- Ningun agente puede hacer deploy sin aprobacion de Security + el Orquestador
- Reportes en /reports/ siguen siendo OBLIGATORIOS
- HANDOFF.md sigue siendo la fuente de verdad

---

## Reglas para TODOS los agentes

### Al iniciar:
1. Lee tu SKILL.md
2. Lee CLAUDE.md del proyecto
3. Lee SECURITY-LOG.md antes de modificar cualquier archivo
4. Lee HANDOFF.md para ver pendientes
5. Lee reportes anteriores en `/reports/` si existen

### Al terminar:
1. Escribe reporte en `/reports/[agente]-[fecha]-[tarea].md`
2. Actualiza HANDOFF.md con el estado de tus tareas
3. Si encontraste algo que otro agente debe saber, escribelo en el reporte

### Formato de reporte:
```markdown
# Reporte: [Agente] — [Tarea]
Fecha: YYYY-MM-DD

## Archivos modificados
- path/to/file — descripcion

## Archivos creados
- path/to/file — proposito

## Tests
- X tests nuevos, Y total passing

## Notas para otros agentes
- [Agente]: detalle

## Pendiente
- Lo que no se pudo completar y por que
```

## Orden de prioridad
1. **WebDev** primero — backend/core debe existir
2. **Creative** en paralelo si es solo UI sin dependencia
3. **Security** SIEMPRE al final antes de deploy (poder de VETO)
4. **DevOps** ultimo — deploy solo despues de aprobacion

## Equipo

| Agente | Rol |
|--------|-----|
| **WebDev** | Backend + Frontend |
| **Creative** | Diseno UI/UX |
| **Security** | Auditoria de seguridad |
| **DevOps** | Deploy e infraestructura |

## Idioma
- Comunicacion: espanol
- Codigo: ingles (estandar)
