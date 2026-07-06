# SOP — Standard Operating Procedures — {{NOMBRE_PROYECTO}}

> **Ultima actualizacion:** —

---

## 1. Flujo de Trabajo (Git Flow)

### Ramas
- `main` — produccion, NUNCA modificar directamente
- `feature/xxx` — funcionalidades nuevas
- `fix/xxx` — correcciones
- `hotfix/xxx` — correcciones urgentes

### Proceso
1. Crear rama `feature/nombre-descriptivo` desde `main`
2. Desarrollar con commits atomicos
3. Push a remote
4. Review de Security antes de merge
5. Merge a main

---

## 2. Proceso de Desarrollo por Sprint

### Inicio de Sprint
1. Revisar `docs/SPRINT.md` para prioridades
2. Leer `.claude/HANDOFF.md` para pendientes
3. Asignar agentes segun AGENT-PROTOCOL

### Durante el Sprint
1. Cada agente trabaja en su area
2. Reportes en `.claude/reports/[agente]-[fecha]-[tarea].md`
3. Actualizar HANDOFF.md al terminar tarea

### Cierre de Sprint
1. Actualizar `docs/SPRINT.md`
2. Actualizar `docs/CHANGELOG.md`
3. Security review final
4. Mover pendientes al siguiente sprint

---

## 3. Documentos del Proyecto

| Documento | Ubicacion | Cuando actualizar |
|-----------|-----------|-------------------|
| CLAUDE.md | `/CLAUDE.md` | Cambios de vision, stack, reglas |
| PRD | `docs/PRD.md` | Nuevas funcionalidades, cambio de alcance |
| TTD | `docs/TTD.md` | Cambios de arquitectura, schema, endpoints |
| SOP | `docs/SOP.md` | Nuevos procesos |
| SPRINT | `docs/SPRINT.md` | Inicio/cierre de cada sprint |
| CHANGELOG | `docs/CHANGELOG.md` | Cada cambio relevante |
| HANDOFF | `.claude/HANDOFF.md` | Al terminar cada tarea o sesion |
| SECURITY-LOG | `.claude/SECURITY-LOG.md` | Cada auditoria o fix |
| Reportes | `.claude/reports/*.md` | Cada agente al terminar tarea |

---

## 4. Seguridad — SOP

### Antes de cada deploy
- [ ] No hay credenciales en codigo
- [ ] Security agent aprobo
- [ ] Backup de base de datos
- [ ] Tests pasando

### Incidentes
1. Detectar → 2. Contener → 3. Documentar en SECURITY-LOG → 4. Corregir → 5. Post-mortem
