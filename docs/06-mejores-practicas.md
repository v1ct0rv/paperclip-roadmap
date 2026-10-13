# 06 · Mejores prácticas para equipos de software

Cada punto está etiquetado: **[OFICIAL]** documentación de Paperclip, **[COMUNIDAD]** experiencia reportada, **[CRITERIO]** síntesis propia.

## 1. Diseño de la organización

- **Empieza con 3–4 agentes, no más** [COMUNIDAD, libro *Headcount Zero* cap. 8]: un "hacedor", un "verificador" y un coordinador. Plano hasta 5 agentes; capas sólo cuando pasas más tiempo revisando output que pensando dirección.
- **Humanos en la junta y como aprobadores** [CRITERIO]: el Tech Lead es el approver de todo PR. No añadas CTO agent hasta tener > 5 agentes ingenieros o hasta que los humanos no puedan revisar a diario.
- **Cada AGENTS.md es un "job posting"** [COMUNIDAD]: Título, Deberes, Autoridad, Límites, Estándares, Reporte, ruta de escalado, y **qué NO hace**. Los fracasos publicados se remontan a instrucciones de un párrafo (Aron Prins); las empresas que funcionan tienen "souls" de tres páginas y repos que "se leen como manuales de operación para un equipo mixto humano+agente" [OFICIAL, estudio de uso].
- **Regla anti-fabricación en todos los prompts** [COMUNIDAD, Effloow]: "Si necesitas datos reales y no existen, escribe `[TBD: awaiting real data]`". Sin ella los agentes inventan métricas, testimonios y estados.
- **Revisor de otra familia de modelos que el coder** [CRITERIO, y así lo hace la plantilla gstack con su skill `codex`]: Codex codifica, Claude revisa, o viceversa. Juicio independiente.
- **Sin delegación explícita el CEO no delega** [OFICIAL]: "si AGENTS.md no menciona delegación, el CEO no sabrá descomponer metas".
- Bundle de instrucciones [OFICIAL]: `AGENTS.md` (qué haces), `SOUL.md` (quién eres, < 40 líneas), `HEARTBEAT.md` (checklist numerado por heartbeat), `TOOLS.md` (cuaderno vivo de herramientas). Plantillas en [`templates/`](../templates/).

## 2. Definición de tareas (donde se gana o pierde todo)

- **Contrato de tarea** [OFICIAL, blog de dotta]: Resultado / Prueba / Límites / Condiciones de parada / Dueño.
- **Criterios de aceptación observables** (Given/When/Then) y definición de "hecho" con evidencia [OFICIAL, skill `qa-acceptance`: "'se ve bien' sin evidencia no es un pase"].
- **Tamaño S/M**: un issue = un PR ≤ 400 líneas netas; L se parte en sub-issues con `parentId` y bloqueos (`blocked by`) entre tareas que tocan los mismos archivos [COMUNIDAD, política de tamaño de PRs con IA: 400 soft / 800 hard, 20 archivos].
- **Comentario `blocked` debe nombrar quién desbloquea** [OFICIAL]. Un agente que no puede pasar CI marca `blocked` y nombra el check.
- **No uses menciones `@` para asignar** [OFICIAL]: cada mención cuesta un heartbeat. Usa asignación y sub-issues.
- **Etiqueta `agent-ready` en Jira/GitHub** [CRITERIO]: sólo los issues con reproducción clara, alcance acotado y tests existentes. Ajusta la etiqueta cada retro con evidencia de aceptación.

## 3. Gobierno y revisión

- **Política de ejecución en todo issue de código** [OFICIAL]: stage `review` (QA o Reviewer agent) + stage `approval` (humano). `maxReviewRounds: 3` escala a humano. El runtime intercepta el `done` del ejecutor y lo convierte en `in_review`; se excluye el auto-review.
- **Merge sólo por humanos vía branch protection + checks requeridos** [OFICIAL + CRITERIO]: en el conector de GitHub, "crear PR: Allowed, merge: Off". Ojo [OFICIAL]: los controles "Ask first" del conector **no limitan `git`/`gh` en shell**; la prohibición de merge también va en AGENTS.md y en la protección de rama.
- **`in_review` sólo con CI verde** [COMUNIDAD]: instruye `gh pr checks --watch` antes de cambiar estado.
- **Decisions / `request_confirmation` para lo irreversible** [OFICIAL]: migraciones de esquema, deploys, bumps de dependencias, borrados.
- **Watchdog en cada épica** [OFICIAL]: "verifica que cada subtarea produjo un PR fusionado; si algo lleva > 1 día bloqueado, escala al CTO".
- **Rampa de gobierno** [COMUNIDAD, Headcount Zero]: semanas 1–4 todo con gate y revisión diaria; meses 2–6 gate sólo en áreas nuevas; > 6 meses sólo excepciones.
- **Dos ventanas de revisión humana al día** y SLA de primera respuesta [COMUNIDAD]: los PRs de agentes esperan 5.3× más en ser tomados (benchmark LinearB 2026); el cuello de botella se vuelve el humano ("coordination tax", blog oficial de Michael Nguyen).

## 4. Concurrencia y workspaces

- **Workspaces aislados (git worktree por issue) ANTES del segundo agente ingeniero** [COMUNIDAD + OFICIAL]: flag experimental `enableIsolatedWorkspaces` + política de proyecto con `branchTemplate: task/{{issueKey}}-{{issueSlug}}`. El default compartido corrompe trabajo (issue #3335).
- **`maxConcurrentRuns: 1`** en todo agente que lanza procesos [COMUNIDAD, issues #6955 y #7641: el default 20 causó OOM en un Mac de 32 GB y colas de 235 runs].
- **Checkout atómico** [OFICIAL]: `409` significa que otro run posee el issue; nunca reintentar.
- **Los worktrees no garantizan merges limpios** [OFICIAL]: regla para coders: "no resuelvas conflictos a ciegas; marca `blocked` nombrando los archivos".
- Bugs abiertos del ciclo de vida de worktrees (#13616, sept 2026): actualiza con frecuencia y no trates `workspace_validation_failed` como fallo real.

## 5. Costos y heartbeats

- **Presupuesto con `hardStopEnabled` el día 1** en company y por agente [OFICIAL]; sin política no hay tope (issue #4027).
- **El tope actúa entre runs** [COMUNIDAD, #11276]: exposición real = tope + un run por slot. Complementa con límites en las consolas de Anthropic/OpenAI.
- **Timers sólo en managers y en horario laboral**; ICs con `wakeOnAssignment` y timer apagado [OFICIAL, patrón "event-driven loop"]. Intervalo > duración típica de un run (un CTO de 130 min con intervalo 20 min encoló 235 runs).
- **Modelo por rol** [COMUNIDAD, Apidog: reduce 40–60 % el gasto]: Haiku/mini para triage, status, resúmenes; Sonnet/GPT-5.x para código S/M; Opus/Fable para planificar y revisar.
- **Suscripciones para los coders de volumen** [COMUNIDAD]; desde el PR #10544 Paperclip valora los runs con suscripción con una tarifa de referencia (antes marcaba US$0).
- **Presupuestos iniciales** [OFICIAL + COMUNIDAD]: CEO US$50, directores US$30, especialistas US$15–25; empresa US$50–100 para empezar, US$100–500 con 5–10 agentes activos.
- **Ritual semanal de 30 min** por agente (Costs → Providers, Activity CSV) y un análisis profundo mensual de un flujo [COMUNIDAD].

## 6. Repositorio "agent-friendly"

- `AGENTS.md` del repo **mínimo y orientado a comandos** ("si un linter lo aplica, no lo repitas"); evita listas de "no hagas"; trátalo como código versionado [COMUNIDAD, ASDLC: archivos de contexto generados por LLM redujeron el éxito y subieron el costo > 20 %]. Claude Code lee `AGENTS.md` si no hay `CLAUDE.md` (2.1.277, sept 2026); `CLAUDE.md` gana.
- **Skills en lugar de contexto siempre activo**: procedimientos repetibles del repo (cómo correr migraciones, cómo desplegar a staging) van en `skills/<slug>/SKILL.md` de la empresa, importados y versionados; los skills externos de GitHub deben fijarse a un SHA de commit [OFICIAL].
- **Spec-driven**: el plan/spec es el artefacto primario; el CEO/CTO agent lo escribe, un humano lo aprueba (plan approval), y de ahí salen los issues [OFICIAL + COMUNIDAD].
- Etiqueta `ai-generated`/`agent` automática por prefijo de rama (actions/labeler) y check de tamaño de PR en CI [COMUNIDAD].
- CI en GitHub Actions sigue siendo la puerta real; Paperclip sólo gobierna estados de issues [CRITERIO].

## 7. Seguridad operativa

- Versión ≥ 2026.916.1; modo `authenticated`; UI detrás de Tailscale o TLS; puerto 3100 cerrado [OFICIAL + CVEs].
- `PAPERCLIP_SECRETS_STRICT_MODE=true`; todo credencial como `secret_ref`; GitHub App o cuenta de servicio, nunca el PAT personal de un dev [OFICIAL].
- "Trata cualquier secreto ligado a un agente como expuesto a ese agente" [OFICIAL]. Credenciales de despliegue a producción **nunca** en el env de un agente: se despliega por CI al hacer merge [CRITERIO].
- Preset `low_trust_review` + sandbox para agentes que leen PRs externos, diffs de dependencias o tickets de clientes [OFICIAL].
- Usuario de sistema dedicado para el servidor; agentes de clientes sensibles en sandbox (Daytona/e2b/Modal) o contenedor por cliente [CRITERIO].
- Backup diario de DB + export nocturno + `master.key` fuera del servidor [OFICIAL].

## 8. Bucle de calidad y memoria

- Evidencia obligatoria en cada comentario de cierre: comandos, salida de tests, screenshots [OFICIAL].
- Convierte un rechazo en regla **sólo si se repite**; guarda lecciones en una memoria de empresa versionada (agente "Learning") y promuévelas a skills, no a un AGENTS.md cada vez más gordo [OFICIAL, blog].
- Plugin de memoria a largo plazo (Hindsight) si los agentes repiten errores entre heartbeats [COMUNIDAD].
- Mide desde el día 1 (no hay benchmarks públicos): issues cerrados/semana, PRs fusionados/semana, minutos humanos de revisión por PR, costo por PR fusionado, tasa de rechazo [CRITERIO].
- Evals: Paperclip trae evals/feedback de agentes; la práctica del sector es evaluar trayectorias, no sólo el output final [OFICIAL].

## Checklist de arranque (imprímelo)

- [ ] Node 24.11+, `test-drive` probado, VPS 4 vCPU/8 GB, Postgres 17 externo, backups + `master.key` fuera
- [ ] `authenticated` + Tailscale/TLS, versión fijada, strict mode de secretos, GitHub App/cuenta de servicio
- [ ] Plantilla `superpowers` o `gstack` importada y recortada a 3–4 agentes; AGENTS.md tipo "job posting" con anti-fabricación y reglas PR-driven
- [ ] Repo piloto con CI, branch protection, AGENTS.md mínimo, 10 issues `agent-ready`
- [ ] Workspaces aislados + `maxConcurrentRuns: 1` + presupuestos con hard stop (company y agentes) + límites en consola del proveedor
- [ ] Execution policy (review agente + approval humano) en todo issue de código; watchdog en épicas
- [ ] Notificaciones a Slack (plugin comunitario o rutina) y dos ventanas de revisión al día
- [ ] Hoja de métricas: PRs, rondas de review, costo por PR, minutos humanos
