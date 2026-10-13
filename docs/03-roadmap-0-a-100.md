# 03 · Roadmap de 0 a 100: de la instalación a la puesta en marcha

> Duración estimada total: **6–8 semanas** hasta un equipo de agentes operando sobre repos reales con revisión humana. Cada fase tiene un criterio de salida ("done") medible. Las fases 0–2 son comunes a los tres escenarios; a partir de la fase 3 consulta [04-escenarios.md](04-escenarios.md).

```
Fase 0        Fase 1         Fase 2          Fase 3           Fase 4          Fase 5           Fase 6
Decidir  ──▶  Laboratorio ─▶ VPS/Docker  ──▶ Primer repo  ──▶ Equipo de   ──▶ Integraciones ─▶ Operación
(2-3 días)    (1 semana)     (2-4 días)      real (1 sem)     ingeniería      Jira/Slack       y escala
                                                              (1-2 sem)       (1 sem)          (continuo)
```

---

## Fase 0 · Decisión y prerrequisitos (2–3 días)

**Objetivo**: saber qué vas a pedirle a los agentes y con qué cuentas.

- [ ] Elige **un repo piloto** de bajo riesgo (herramienta interna, sitio de marketing, un microservicio con buena cobertura de tests). Nunca el core de un cliente en el piloto.
- [ ] Escribe 5–10 issues "candidatas" que un dev junior podría resolver en < 4 horas cada una (bugs con reproducción clara, endpoints CRUD, tests faltantes, migraciones de librerías).
- [ ] Inventario de credenciales: suscripción Claude Max / ChatGPT Pro (para correr con suscripción) y API keys de respaldo (Anthropic, OpenAI). Decide **quién paga qué**.
- [ ] Repo piloto con: CI que corra tests + lint en cada PR, `main` protegido (require PR + checks + 1 review), `AGENTS.md` o `CLAUDE.md` con comandos de build/test.
- [ ] Define el **presupuesto mensual tope** del experimento (sugerido: US$100–300 el primer mes).

**Done**: existe un doc de una página con: repo piloto, 10 issues, presupuesto, responsable humano (el "board").

---

## Fase 1 · Laboratorio local en tu Mac (1 semana)

**Objetivo**: entender el ciclo CEO → tarea → agente → PR sin arriesgar nada.

### 1.1 Instalar

```bash
# Requisitos: Node.js 24.11+ (usa mise/nvm), Claude Code y/o Codex CLI instalados y logueados, gh CLI
node -v                      # >= 24.11
claude --version && codex --version && gh auth status

# Opción A (5 minutos, todo por defecto, Postgres embebido en ~/.paperclip):
npx paperclipai onboard --yes
# → http://localhost:3100

# Opción B: prueba aislada y desechable con CEO pre-cargado
ANTHROPIC_API_KEY=sk-... npx paperclipai test-drive        # o --harness codex
```

Gotchas documentados: no correr como root (Postgres embebido lo rechaza); en Apple Silicon puede faltar el symlink de `libzstd.1.dylib`/`liblz4.1.dylib`; si npm falla, `npx --registry https://registry.npmjs.org paperclipai onboard --yes`.

### 1.2 Crear organización y CEO

1. Wizard → nombre de la organización → Continue.
2. **Goals** → una meta con métrica y plazo, p. ej. *"Cerrar 20 issues del backlog de `repo-piloto` con PRs aprobados antes del 31 de octubre"*.
3. **Agents → New Agent**: nombre (el primero es CEO automáticamente), adapter `claude_local`, working directory absoluto (`~/paperclip-workspace`), modelo grande para el CEO, heartbeat **1 vez por hora** (empieza apagado; enciéndelo tú), budget US$30–50/mes.
4. Ejecuta "Run Heartbeat" manualmente. El CEO propone una estrategia → **apruébala o pide revisión**. Lee el transcript completo: así aprendes qué contexto le falta.

### 1.3 Instalar un equipo del catálogo o una empresa-plantilla

```bash
# Equipos bundled (dentro de la app)
paperclipai teams browse --kind bundled
paperclipai teams inspect core-exec-team          # CEO + CTO + QA + proyecto inicial
paperclipai teams preview core-exec-team --company-id <id>
paperclipai teams install core-exec-team --company-id <id>

# Empresas completas de software del repo oficial paperclipai/companies (agentes + skills + rutinas)
npx companies.sh add paperclipai/companies/gstack        # CEO → CTO → Staff Eng, Release Eng, QA (27 skills)
npx paperclipai company import --from ./superpowers       # CEO → Lead Eng (TDD), Code Reviewer, Release Eng
```

Plantillas recomendadas para software: **gstack** (5 agentes, pipeline plan → revisión estructural → release → QA en navegador headless), **superpowers** (4 agentes, TDD estricto, "evidencia antes de afirmaciones"), **clawteam-engineering** (Tech Lead → Backend, Frontend, QA, DevOps). Evita **fullstack-forge** (49 agentes) para empezar. Los agentes importados llegan **pausados**; actívalos uno a uno.

### 1.4 Conectar el repo piloto (≈20 min según docs oficiales)

1. Project → Workspace: `cwd` absoluto del clon, `repoUrl`, `repoRef: main`, `isPrimary`, modo **Isolated**.
2. Activa workspaces aislados. Sigue detrás de un flag experimental de instancia y luego se configura por proyecto:
   ```bash
   curl -X PATCH $API/api/instance/settings/experimental -H "Authorization: Bearer $KEY" \
        -d '{"enableIsolatedWorkspaces": true}'
   curl -X PATCH $API/api/projects/$PROJECT_ID -H "Authorization: Bearer $KEY" -d '{"executionWorkspacePolicy":{
        "enabled":true,"defaultMode":"isolated_workspace","allowIssueOverride":true,
        "workspaceStrategy":{"type":"git_worktree","baseRef":"main","branchTemplate":"task/{{issueKey}}-{{issueSlug}}"}}}'
   ```
   Sin esto todos los agentes comparten el mismo `cwd` y se corrompen el trabajo (issue #3335 reporta 222 workspaces sobre un mismo directorio).
2b. En **cada agente que lanza procesos** fija `runtimeConfig.heartbeat.maxConcurrentRuns: 1` (el default es 20 y ha provocado OOM y colas de 235 runs; issues #6955 y #7641). Ver `templates/agent-runtime-config.json`.
3. Credencial: PAT fine-grained (Contents R/W + Pull requests R/W) guardado como **secret de la empresa** y ligado en el `env` del agente como `GH_TOKEN` y `GITHUB_TOKEN` (`secret_ref`).
4. Copia [`templates/AGENTS.coder.md`](../templates/AGENTS.coder.md) al agente coder y añade el skill bundled `github-pr-workflow`.
5. Crea 1 issue con `assigneeAgentId` = coder → observa: worktree en `<cwd>/.paperclip-worktrees/<ISSUE-ID>`, branch, commit, PR, issue en `in_review`.

**Done**: 3 issues del piloto terminaron en PRs que **tú** fusionaste; conoces el costo real por PR (pestaña Costs).

---

## Fase 2 · Servidor: VPS + Docker (2–4 días)

**Objetivo**: instancia compartida por el equipo, con login, TLS y backups. (Elegiste ir directo a VPS; si prefieres saltar la fase 1, hazla al menos con `test-drive` un día.)

### 2.1 Máquina

- VPS 4 vCPU / 8 GB RAM / 80 GB SSD (Hetzner CPX31, DigitalOcean, Vultr ≈ US$20–40/mes). Los docs oficiales estiman US$5–35/mes en total de infraestructura.
- Ubuntu 24.04, Docker + Compose, `gh` CLI, dominio apuntando (`paperclip.tudominio.com`).
- **No abras el puerto 3100**; sólo 80/443 vía Caddy/Nginx.

### 2.2 Despliegue

```bash
git clone https://github.com/paperclipai/paperclip.git && cd paperclip
docker build --target production -t paperclip-local .     # --target production es OBLIGATORIO
# copia templates/docker-compose.vps.yml, templates/Caddyfile y templates/.env.vps.example (→ .env)
docker compose up -d
curl https://paperclip.tudominio.com/api/health            # {"status":"ok"}
npx paperclipai auth bootstrap-ceo --base-url https://paperclip.tudominio.com   # URL de invitación del primer usuario
```

Variables críticas: `PAPERCLIP_DEPLOYMENT_MODE=authenticated`, `PAPERCLIP_DEPLOYMENT_EXPOSURE=public`, `PAPERCLIP_AUTH_PUBLIC_BASE_URL` (si falta, `doctor` falla con *auth.publicBaseUrl is required*), `PAPERCLIP_ALLOWED_HOSTNAMES`, `TRUST_PROXY`, `DATABASE_URL` externo (el Postgres embebido es sólo para local).

### 2.3 Endurecimiento mínimo

> Contexto: en 2026 Paperclip acumuló 12 advisories de seguridad, incluidos **CVE-2026-41679** (RCE sin autenticación, CVSS 10.0, con módulo de Metasploit) y **CVE-2026-77087** (RCE drive-by por DNS rebinding contra instalaciones locales por defecto, CVSS 9.6, julio). Corregidos desde v2026.416.0. Instala **siempre** ≥ 2026.916.1 y nunca expongas la UI sin autenticación.

- Respalda `PAPERCLIP_HOME/instances/default/secrets/master.key` fuera del servidor: **sin esa llave pierdes todos los secretos**.
- Backups: `paperclipai db:backup --dir /backups --retention-days 14` en cron + export nocturno de la empresa (`paperclipai company export`, rutina `0 2 * * *`).
- `PAPERCLIP_SECRETS_STRICT_MODE=true` (default en modo authenticated): obliga a que `*_API_KEY/*_TOKEN/*_SECRET` sean referencias a secretos, nunca texto plano en la config del agente.
- Preferible: `authenticated` + `private` detrás de **Tailscale** para el equipo; `public` sólo si necesitas webhooks entrantes (GitHub, Slack chat) y entonces con Caddy/Nginx + `PAPERCLIP_ALLOWED_HOSTNAMES`.
- Telemetría: `PAPERCLIP_TELEMETRY_DISABLED=1` si quieres.
- Agentes con shell: corren **dentro del contenedor** con acceso al filesystem de sus workspaces. Para clientes con datos sensibles usa sandboxes (Daytona/e2b/Modal) o una VM por cliente.
- Codex ACP abre red por defecto: `PAPERCLIP_CODEX_ACP_NETWORK_ACCESS="false"` si no la necesitas.
- Rotación: agent API keys por run (JWT corto), PATs fine-grained por repo con expiración.

### 2.4 Credenciales de modelos

- **Connections** (UI) → agrega suscripción Claude Max y/o Codex (login de dispositivo) y API keys de respaldo.
- Regla práctica: suscripciones para coders de alto volumen; API key con budget duro para el CEO/CTO (menos volumen, más costoso por token).

**Done**: 2+ personas del equipo entran con login, un heartbeat corre en el servidor, backup restaurado en prueba una vez.

---

## Fase 3 · Primer repo real con revisión humana (1 semana)

**Objetivo**: 10 PRs fusionados, medir calidad y costo.

- Org mínima: **CTO agent + 1 Coder (Codex o Claude) + 1 QA**. Sin CEO al principio si el board eres tú: menos capas = menos costo y ruido.
- Política de ejecución por issue: review por QA agent + approval por humano (plantilla `execution-policy.review-gate.json`). `maxReviewRounds: 3` (default) para que un loop QA↔coder escale a un humano.
- Heartbeats: coders **sin timer** (sólo wake por asignación/comentario); CTO cada 1–2 h en horario laboral (rutina cron), no 24/7.
- Presupuestos: company US$300, CTO US$40, coder US$25, QA US$15 (plantilla `budget-policies.json`); aviso al 80 %, pausa al 100 %. **Ojo**: el tope se aplica *entre* runs, no a mitad de uno (issue #11276): la exposición real es tope + un run por slot concurrente. Por eso `maxConcurrentRuns: 1` y límites de gasto también en la consola de Anthropic/OpenAI.
- Métricas a capturar cada día: PRs abiertos / fusionados / rechazados, rondas de revisión promedio, costo por PR fusionado, tiempo humano de revisión por PR.

**Done**: ≥ 10 PRs fusionados, tasa de rechazo < 30 %, costo por PR conocido, un post-mortem escrito de lo que falló.

---

## Fase 4 · Equipo de ingeniería completo (1–2 semanas)

Amplía sólo donde la fase 3 mostró un cuello de botella ("Start small, add agents as gaps appear", guía oficial).

| Rol | Adapter sugerido | Modelo | Heartbeat | Skills |
|---|---|---|---|---|
| CEO (opcional) | claude_local | Opus / Fable | 1×/día (rutina 07:00) | task-planning, status-card-query |
| CTO / EM | claude_local | Opus | 1–2 h laboral | task-planning, issue-triage |
| Backend coder | codex_local | gpt-5.x | on-demand | github-pr-workflow |
| Frontend coder | claude_local | Sonnet | on-demand | github-pr-workflow, wireframe |
| QA | claude_local | Sonnet | on-demand | qa-acceptance |
| Reviewer (senior) | claude_local | Opus | on-demand | github-pr-workflow |
| Doc/Release | codex_local | gpt-5-mini | rutina semanal | doc-maintenance, release-announcement |
| DevOps/SRE (opcional) | claude_local | Sonnet | rutina + webhook de Sentry | prepare-mcp-integration |

- Rutinas: triage diario del backlog (issue-triage), "daily brief" a las 07:00 (status card), limpieza de PRs viejos, sync semanal de ingeniería (viene en el pod product-engineering).
- Watchdogs de tareas activados para detectar runs estancados; `exit 143` = timeout/OOM → partir el issue.
- Skills propias de la empresa: crea `skills/<slug>/SKILL.md` con tu stack (comandos, convenciones, ADRs) e impórtalo con `POST /api/companies/$ID/skills/import`. Se inyecta en cada run.

**Done**: el equipo cierra un sprint completo de issues S/M sin intervención más allá de revisión de PRs y aprobaciones.

---

## Fase 5 · Integraciones GitHub + Jira + Slack (1 semana)

- **GitHub**: pasa de PAT a **GitHub App** (tokens de instalación cortos por heartbeat) o conexión gestionada (webhooks de `pull_request` mueven issues a `done` automáticamente). Protege `main`; los agentes tienen prohibido merge.
- **Jira** (conector MCP de Atlassian Cloud): los agentes leen/comentan/transicionan. Patrón realista hoy: **Jira es la fuente de verdad del negocio**; una rutina "importadora" (agente CTO) lee el sprint activo y crea issues espejo en Paperclip con el link a la clave Jira; al pasar a `done`, el agente comenta y transiciona en Jira ("Ask first" para escrituras hasta que confíes). El sync bidireccional nativo está en el roadmap, no lo esperes.
- **Slack**: (0) opción rápida: plugin comunitario `mvanhorn/paperclip-plugin-slack` (notificaciones, botones Approve/Reject, alertas de presupuesto 80/90/100 %, digest diario, comandos `/clip`); revísalo antes de instalarlo, es código de terceros con acceso a tu instancia. (a) notificador nativo = rutina cada minuto (`concurrencyPolicy: skip_if_active`) + agente barato que consulta `/approvals?status=pending`, `/issues?status=blocked&priority=critical,high` y `/dashboard`, y publica Block Kit a un webhook guardado como secret. (b) Canal de chat experimental: menciones a `@paperclip` en `#dev` se vuelven tareas (requiere HTTPS público). El tool MCP de Slack para agentes está "no completamente funcional" en 2026.916 → no dependas de él.
- **Sentry/PostHog** (opcional): webhook → rutina → issue para el SRE agent.

**Done**: aprobaciones y bloqueos llegan a Slack en < 2 min; cada issue de Paperclip tiene su clave Jira; nadie entra a Paperclip para saber "qué pasa".

---

## Fase 6 · Operación, escala y gobierno (continuo)

- **Ritual semanal (30 min)**: Costs → Providers (qué modelo gasta), rondas de revisión, PRs rechazados, issues `blocked`. Ajusta prompts/skills, no agentes.
- **Multi-cliente / multi-equipo**: una **organización por cliente** (aislamiento real de datos, presupuesto y credenciales), mismo servidor. Export/import zip para clonar la plantilla de org.
- **Runner separado**: cuando tengas "un puñado de agentes ocupados", mueve el runner a otro contenedor compartiendo `DATABASE_URL` + `PAPERCLIP_API_URL`.
- **Actualizaciones**: fija versión (`:2026.916.1`), prueba `beta` en staging una semana antes; el proyecto rompe cosas entre releases (500 commits por release).
- **Observabilidad**: `OTEL_EXPORTER_OTLP_ENDPOINT` a tu Grafana/Tempo, `SENTRY_DSN`.
- **Kill switch**: pausar la empresa (pausa ignora todos los wakes) y `hardStopEnabled` en todos los presupuestos.
- **Evolución del roadmap del producto a vigilar**: memoria/knowledge, work queues, bring-your-own-ticket-system (Jira/Linear sync), Paperclip Cloud, desktop app.

**Done (el "100")**: cada sprint, ≥ 30–50 % de los issues S/M los cierran agentes con PRs fusionados por humanos, costo por PR estable y predecible, ningún incidente de seguridad, y el equipo humano dedica su tiempo a diseño, revisión y clientes.
