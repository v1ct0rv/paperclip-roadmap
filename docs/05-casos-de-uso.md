# 05 · Casos de uso reales para equipos de desarrollo

Cada caso incluye: quién lo hace hoy (evidencia), cómo se configura en Paperclip, y qué medir. Los casos 1–3 son los que Paperclip documenta y usa consigo mismo ("Paperclip on Paperclip": su propia web y producto los construyen agentes con revisión humana de PRs).

## Evidencia de uso real (no marketing)

- Investigación oficial (marzo 2026, commits con trailer `Co-Authored-By: Paperclip`): **14,267 commits en 884 repos de 562 owners**. Mediana de 3 commits por repo; el top 100 concentra el 70 % → muchos prueban, pocos lo adoptan en serio. Perfil dominante: founders técnicos y equipos pequeños, stacks TypeScript/Next.js. Fuente: https://paperclip.ing/blog/who-is-using-paperclip-on-github/
- Ejemplos con cientos de commits: propra-app (640, proptech), dc1-platform (541, marketplace GPU), lexreclama-web (202, legaltech España), ghostwritersinc (263, negocio de contenido), quant-zero (208, finanzas).
- Ecosistema comunitario: [awesome-paperclip](https://github.com/gsxdsm/awesome-paperclip) (923 ★) con plugins de GitHub Issues sync bidireccional, Linear, Slack, Discord, memoria (Hindsight), `paperclip-mcp` (expone la API como MCP para Claude Code/Desktop).

---

## Caso 1 · "PR nocturno": del issue al pull request sin humanos hasta el review

**Quién**: es el flujo canónico documentado ([connect-agent-to-github](https://docs.paperclip.ing/how-to/connect-agent-to-github/)) y el que usa el propio equipo de Paperclip.

**Setup**
- Project workspace → repo, `git_worktree` por issue.
- Coder (`codex_local` o `claude_local`) con `AGENTS.md` PR-driven + skill `github-pr-workflow`.
- Issue con criterios de aceptación → asignación → wake → branch `PAP-123-…` → commit → `gh pr create` → `in_review`.
- Execution policy: review por QA agent (tests, screenshots) → approval humana → merge en GitHub → webhook mueve a `done`.

**Medir**: PRs/día, % fusionados sin cambios, rondas de revisión, costo por PR fusionado, minutos humanos por PR.

**Trampas**: sin `git_worktree` los coders se pisan; sin `GH_TOKEN` el run se bloquea con `push_write_credential_missing`; el adapter de Claude Code no trae `gh`, instálalo en el host/imagen.

## Caso 2 · Backlog de deuda técnica y migraciones repetitivas

**Quién**: patrón típico de agencias y equipos con productos maduros (migrar librería UI, Node 18→24, tests de snapshot, tipado estricto).

**Setup**
- Tech Lead escribe UNA "meta de proyecto" con métrica: "Migrar los 45 componentes de `ui/legacy` a `@acme/ds` con tests, antes de fin de mes".
- CTO agent con skill `task-planning` la descompone en 45 sub-issues (`parentId`) de tamaño S; 2 coders en paralelo (worktrees).
- Watchdog en la issue padre: cuando todos los hijos están en reposo, revisa y abre follow-ups.
- Skill de empresa `migracion-ds/SKILL.md` con el mapeo de componentes y ejemplos "antes/después" → se inyecta en cada run y reduce muchísimo la variabilidad.

**Medir**: sub-issues cerrados/día, regresiones detectadas por CI, costo total vs estimación humana.

## Caso 3 · QA agent como puerta de calidad

**Quién**: rol estándar del pod `product-engineering` del catálogo oficial (CTO + Senior Coder + QA).

**Setup**
- QA (`claude_local`, Sonnet) con skill `qa-acceptance`: convierte el issue en criterios observables, corre la app/tests en el worktree, adjunta screenshots.
- Execution policy `review` stage con QA como participante; `maxReviewRounds: 3` → si el coder no converge, escala al humano.
- Para UI: adapter con `chrome`/skill `agent-browser` para pruebas E2E ligeras.

**Medir**: bugs encontrados por QA antes del review humano, falsos positivos.

## Caso 4 · Triage y respuesta a bugs (Jira + Sentry) fuera de horario

**Setup**
- Rutina 22:00 (cron, `skip_if_active`) despierta al agente "Soporte L1": lee Jira (MCP de Atlassian: buscar/leer/comentar) filtrando `type = Bug AND status = Open AND labels = agent-ready`, reproduce en el worktree, escribe el test que falla y un PR con fix propuesto; comenta en Jira con el link (escrituras en "ask first" al inicio).
- Sentry → webhook firmado → rutina → issue para el mismo agente.

**Medir**: tiempo hasta primera respuesta, % de PRs de fix aceptados.

**Limitación**: no hay sync bidireccional Jira↔Paperclip nativo; el "espejo" lo hace el agente. El plugin comunitario de GitHub Issues sí es bidireccional si tu backlog vive en GitHub.

## Caso 5 · Documentación y releases que no se atrasan

**Setup**
- Agente "Docs & Release" (`codex_local`, modelo mini) con skills `doc-maintenance` y `release-announcement`.
- Rutina webhook `pull_request.closed` (github_hmac) → issue "Actualizar docs afectados por PR #N".
- Rutina semanal viernes 16:00 → changelog + borrador de release notes → aprobación humana → publicación.

**Medir**: docs desactualizados detectados en retro, tiempo de preparación de release.

## Caso 6 · Onboarding a un repo desconocido (agencias)

**Setup**: nueva cuenta → org clonada por import → primer issue "Explora el repo y escribe `ARCHITECTURE.md` + comandos de build/test + riesgos" para el CTO agent → revisión humana → ese doc se convierte en el skill de empresa para todos los agentes de esa org.

**Medir**: horas humanas de onboarding ahorradas.

## Caso 7 · Daily brief y visibilidad para management

**Setup**: rutina 07:00 → CEO/CTO agent con skill `status-card-query` consulta `/dashboard`, issues y costos → publica en Slack (rutina notificadora) un brief: qué se cerró, qué está bloqueado, qué aprobaciones esperan, gasto del mes. Reemplaza el standup para el trabajo de agentes.

---

## Qué NO delegar a agentes (todavía)

- Cambios de arquitectura o de contratos de API públicos sin diseño humano previo.
- Migraciones de datos en producción (sólo el plan y el dry-run).
- Seguridad/auth/pagos sin revisión senior obligatoria y tests específicos.
- Merges: nunca. Los docs de Paperclip lo repiten: "no es una herramienta de code review, trae tu proceso".
