# 07 · Operación diaria, costos y seguridad

## Rutina del "board" humano (15–30 min/día)

| Momento | Acción | Dónde |
|---|---|---|
| 07:00 | Leer daily brief (Slack) | rutina status-card |
| Mañana | Aprobar/rechazar approvals pendientes (estrategia, contrataciones, gasto > US$25) | Paperclip → Approvals, o link en Slack |
| Durante el día | Revisar PRs `in_review` (GitHub) y fusionar | GitHub |
| Cuando hay `blocked` | Contestar la pregunta concreta en el issue (el agente se despierta por comentario) | Paperclip issue |
| Viernes | Ritual semanal de costos y calidad | Costs → Providers / Activity CSV |

## Costos: qué esperar y cómo controlarlos

- Estimación oficial: un worker "moderadamente activo" consume 100k–500k tokens/mes ≈ **US$3–15/mes**; CEO US$30–50, manager US$20–40, worker US$10–25; empresa inicial **US$50–100/mes**. Un equipo de ingeniería que realmente cierra PRs gastará más (los coders con Opus/GPT-5.x en repos grandes pueden llegar a US$1–5 por PR).
- **Suscripciones vs API**: Paperclip muestra ventanas de cuota (5 h / diaria / semanal) de Claude Pro/Max y ChatGPT. Los coders de alto volumen sobre suscripción reducen el costo marginal a ~0 hasta el límite; ten API keys de respaldo con budget duro.
- Tres capas de tope: company (mes calendario UTC), agente (mes), proyecto (lifetime). Aviso 80 %, pausa 100 %, incidentes de presupuesto con `raise_budget_and_resume` o `keep_paused`.
- **El hard stop actúa entre runs, no dentro de uno** (issue #11276: una política de US$0.50 dejó pasar un run de US$5.11). Exposición real = tope + (runs concurrentes × costo máximo de un run). Mitiga con `maxConcurrentRuns: 1`, `timeoutSec` y `maxTurnsPerRun` bajos, y límites mensuales en las consolas de Anthropic y OpenAI.
- Sin política de presupuesto **no hay tope**: un usuario reportó 1,293 heartbeats por timer en 10 días con `budgetMonthlyCents=0` (issue #4027). Crea la política el día 1.
- Casos reales de gasto: US$236 → US$11/mes moviendo roles de Opus a modelos baratos (Moe Lueker); US$130 perdidos por una empresa mal configurada con "souls" de un párrafo (Aron Prins); cientos de miles de tokens por noche de 22 agentes **sin tareas** (issue #373); "1 % de la cuota Max por minuto" (issue #3154). Regla de la comunidad: heartbeats de 4–8 h o sólo on-demand, y modelos Sonnet/Haiku hasta demostrar valor.
- Modelos por rol: grande para planificar/revisar (pocos tokens, alto valor), mediano para codificar S/M, mini para docs/notificaciones/triage.
- Heartbeats por timer sólo en managers y sólo en horario laboral (rutina cron). Coders: **wake on demand**. Un CEO cada hora 24/7 es la forma más rápida de quemar dinero creando tareas.
- `maxTurnsPerRun` (Claude, default 300) y `timeoutSec` acotan un run; `outputInactivityTimeoutMs` (Codex) detecta cuelgues.

## Seguridad: modelo de amenaza realista

| Riesgo | Mitigación en Paperclip / entorno |
|---|---|
| Agente con shell borra/expone datos | Workspaces aislados por proyecto; contenedor sin montar `/`; sandboxes (Daytona/e2b/Modal) para clientes sensibles; usuario sin sudo en el contenedor |
| Fuga de secretos | Secrets cifrados con `master.key`; `secret_ref` en env; agentes piden secretos "cuando los necesitan" (feature 2026.8); nunca en AGENTS.md; PATs fine-grained por repo con expiración |
| Prompt injection desde issues/PRs/webs | Escrituras a Jira/Slack en "ask first"; merges prohibidos a agentes; branch protection; review humano obligatorio; egress limitado por tarea (task-scoped grants) |
| Runaway spend | hardStop en los 3 niveles; heartbeats on-demand; watchdogs; `maxReviewRounds` |
| Exposición de la UI (3 RCE críticos en 2026: CVE-2026-41679 CVSS 10, CVE-2026-77087 DNS rebinding) | Versión ≥ 2026.916.1; `authenticated` + Caddy/Nginx TLS; puerto 3100 cerrado; `TRUST_PROXY` correcto; `PAPERCLIP_ALLOWED_HOSTNAMES`; idealmente `private` vía Tailscale para el equipo; nunca `local_trusted` en una máquina con navegador que visite sitios desconocidos |
| Agente que lee input hostil (PRs externos, tickets de clientes, diffs de dependencias) | Preset `low_trust_review`: exige sandbox + workspace aislado, rechaza secretos inline, no puede comentar en hilos padre (vía de escalada de prompt injection) |
| Supply chain (npm install script) | Verifica `install.sh.sha256`; fija versión; construye la imagen tú mismo con `--target production` |
| Pérdida de datos | `db:backup` diario + export nocturno de empresa + copia offsite de `master.key` |
| Cumplimiento con clientes | Revisa ZDR/DPA de Anthropic/OpenAI; Bedrock/Vertex si exigen región; una org por cliente |

## Troubleshooting rápido

- Heartbeat "no hace nada" y sale: bandeja vacía + timer → apaga el timer, usa wake por asignación.
- `409 Conflict` en checkout: otro run tiene el issue; es normal, no es bug.
- Exit 143: timeout u OOM → parte el issue, sube `timeoutSec`, revisa RAM del VPS.
- `issue_rewake_throttled`: dos runs sin progreso → cooldown 2→30 min; revisa el transcript y el AGENTS.md.
- `push_write_credential_missing`: falta `GH_TOKEN`/`GITHUB_TOKEN` ligado al agente.
- `auth.publicBaseUrl is required`: falta `PAPERCLIP_AUTH_PUBLIC_BASE_URL`.
- Guía oficial: https://docs.paperclip.ing/how-to/debug-stuck-heartbeat/ y https://github.com/paperclipai/paperclip/blob/main/docs/guides/troubleshooting.md

## Actualizaciones

- Versionado `YYYY.MDD.P`; canales canary/nightly/beta/stable; releases estables cada 1–2 semanas con cientos de commits: **hay cambios que rompen** (p. ej. Node 22→24 en 2026.831, "cheap model profiles" eliminados en 2026.916).
- Práctica: fija la versión en producción, prueba `beta` en un contenedor de staging, lee las release notes antes de subir, y exporta la empresa antes de cada upgrade.
