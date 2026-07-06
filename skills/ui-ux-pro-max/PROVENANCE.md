# Procedencia — ui-ux-pro-max (y bundle de diseño)

- Origen: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- Commit AUDITADO e instalado: b7e3af80f6e331f6fb456667b82b12cade7c9d35
- Auditoría de seguridad: 2026-05-30 (Security Engineer) — veredicto INSTALAR CON PRECAUCIONES.
  Código limpio, sin malware/telemetría/prompt-injection, no ejecuta nada en install.
- Instalado SOLO el contenido de .claude/skills/ (sin el CLI npm uipro-cli, sin postinstall).
- ELIMINADOS los 3 scripts que leen GEMINI_API_KEY: design/scripts/{cip,logo,icon}/generate.py
- Riesgo residual: single-maintainer en npm → re-AUDITAR el diff antes de cualquier upgrade. NO actualizar a ciegas.
- Encuadre recomendado: usar para guías de UX/accesibilidad/patrones/checklists. RESPETAR la marca
  esmeralda (#059669/#10b981) + navy (#0f172a), Inter + JetBrains Mono. NO reinventar la identidad del producto.

- 2026-05-30: ELIMINADA también la skill `design` completa (generación de logos/iconos con Gemini) — innecesaria para UX/UI y única que referenciaba llaves externas. Quedan 6 skills de UX/UI puro sin red ni keys.

- 2026-06-04: ACTUALIZADO a v2.5.0 (commit 07f4ef3). Re-auditado por security (auditoría interna de seguridad). design/ EXCLUIDA de nuevo (scripts Gemini ahora leen ~/.claude/.env — agravado). Skills seguras agregadas: banner-design, slides, brand, design-system, ui-styling. Backups: *.bak-20260604-191018
