# Paperclip para equipos de desarrollo de software — Roadmap 0 → 100

> Investigación y plan de adopción de [Paperclip](https://paperclip.ing) (repo: [paperclipai/paperclip](https://github.com/paperclipai/paperclip), sitio antiguo: paperclipai.net), elaborado el 28 de septiembre de 2026 y **revisado a fondo el 12 de octubre de 2026** (releases 1001.0, 1005.0 y 1012.0, docs, blog, comunidad y alternativas) a partir de documentación oficial, Context7, GitHub, blog oficial, comunidad (X, YouTube, HN, dev.to, podcasts) y un mapa de alternativas.

**Versión resumida en una página (Artifact):** https://claude.ai/artifact/UU2gSBGNoX7p4BWbgik83t

## TL;DR

- **Qué es**: un *control plane* open source (MIT) para equipos de agentes de IA. Organiza a los agentes que ya tienes (Claude Code, Codex, Cursor, Gemini CLI, OpenClaw…) en un organigrama con metas, tareas, heartbeats, presupuestos con parada automática y aprobaciones humanas. "Si OpenClaw es un empleado, Paperclip es la empresa."
- **Tracción**: 101k ★ en 7 meses, tres releases en las últimas tres semanas (v2026.1012.0 el 12 oct), ~120k descargas/mes, 16 plantillas oficiales de empresas. Pero: un mantenedor con el 61 % de los commits, ~3k issues abiertas (797 abiertas vs 95 cerradas en 30 días), 3 RCE críticos en 2026 (corregidos, ninguno nuevo), tres releases con breaking changes en tres semanas, financiación no divulgada, Cloud en waitlist y el único hosting gestionado cerrado el 2 de octubre.
- **Veredicto**: **viable como piloto aislado y gobernado** (runner 24/7 del backlog `agent-ready`, control de gasto, auditoría) sobre Claude Code + Codex; **no** como columna vertebral de la entrega a clientes todavía. Scorecard 3.3/5. Reevaluar en marzo de 2027.
- **Alternativas**: Multica (competidor directo más simple), OpenAI Symphony (Linear + Codex), GitHub Agent HQ / Linear Agents / Rovo Dev (cero infra, "asigna issue → PR"), Devin/Factory (enterprise). Para un solo founder, Paperclip es la mejor de su categoría.
- **Tiempo al "100"**: 6–8 semanas desde la instalación hasta un equipo de agentes cerrando ≥ 30 % de los issues S/M de un sprint con PRs fusionados por humanos.

## Índice

| Doc | Contenido |
|---|---|
| [01 · ¿Qué es Paperclip?](docs/01-que-es-paperclip.md) | Conceptos, arquitectura, adapters, integraciones, qué NO es |
| [02 · Tendencias y comunidad](docs/02-tendencias-y-comunidad.md) | Qué cambió del 20 sep al 12 oct, tracción, quién está detrás, virales, críticas, issues calientes, tendencias del sector |
| [03 · Roadmap 0 → 100](docs/03-roadmap-0-a-100.md) | 7 fases con comandos, criterios de salida y tiempos: decisión → laboratorio → VPS/Docker → primer repo → equipo → Jira/Slack → operación |
| [04 · Escenarios](docs/04-escenarios.md) | Empresa 5–20 devs · Solo founder · Agencia multi-cliente: organigramas, presupuestos, gobierno |
| [05 · Casos de uso](docs/05-casos-de-uso.md) | 15 casos concretos con setup y métricas (PR nocturno, deuda técnica, QA, triage Jira/Sentry, docs, review bot, Slack, QA visual, MCP desde Claude Code, auditoría, OSS, DevOps) + qué no delegar |
| [06 · Mejores prácticas](docs/06-mejores-practicas.md) | Org, tareas, gobierno, concurrencia, costos, repo, seguridad, calidad. Checklist |
| [07 · Operación diaria](docs/07-operacion-diaria.md) | Rutina del board, costos reales, modelo de amenaza, troubleshooting, upgrades |
| [08 · Fuentes](docs/08-fuentes.md) | Todas las URLs consultadas |
| [09 · Viabilidad y alternativas](docs/09-viabilidad-y-alternativas.md) | Scorecard, riesgos, mapa de mercado, comparación, recomendación por escenario |

## Plantillas listas para usar (`templates/`)

| Archivo | Uso |
|---|---|
| `docker-compose.vps.yml`, `Caddyfile`, `.env.vps.example` | Despliegue en VPS con Postgres externo y TLS automático |
| `AGENTS.coder.md`, `HEARTBEAT.coder.md` | Rol ingeniero: reglas PR-driven, tamaño de PR, verificación |
| `AGENTS.cto.md`, `SOUL.cto.md` | Rol CTO/EM: planificación, desbloqueo, revisión |
| `agent-runtime-config.json` | `maxConcurrentRuns: 1`, timeouts, modelos, secretos por referencia |
| `execution-policy.review-gate.json` | Review por QA agent + aprobación humana por issue |
| `budget-policies.json` | Topes con parada automática por empresa y por agente |

La página resumen (Artifact) también está versionada en `artifact/paperclip-roadmap.html`.

## Los 10 mandamientos (si sólo lees esto)

1. Versión ≥ 2026.1012.0, modo `authenticated`, UI detrás de Tailscale o TLS, puerto 3100 cerrado. Los harnesses corren en full auto por defecto: restringe explícitamente a los agentes que leen input hostil.
2. Postgres 17 externo, backup diario y `secrets/master.key` fuera del servidor.
3. Empieza con 3–4 agentes desde la plantilla `superpowers` o `gstack`; añade agentes sólo donde aparezca un cuello de botella.
4. AGENTS.md tipo "job posting" (deberes, autoridad, límites, qué NO hace) con regla anti-fabricación y reglas PR-driven.
5. Workspaces aislados (worktree por issue) y `maxConcurrentRuns: 1` antes del segundo agente ingeniero.
6. Presupuesto con `hardStopEnabled` en empresa y agentes el día 1, más límites en las consolas de Anthropic/OpenAI (el tope actúa entre runs, no dentro) y una API key de respaldo para las suscripciones (expiran a las ~8 h, bug abierto).
7. Heartbeats por timer sólo en managers y en horario laboral; ingenieros sólo on-demand. Construye la org con todo pausado y activa de uno en uno.
8. Review por agente de otra familia de modelos + aprobación humana en todo issue; merge sólo por humanos con branch protection.
9. Issues `agent-ready` pequeños (≤ 400 líneas), con criterios de aceptación observables y condición de parada.
10. Mide desde el día 1: PRs fusionados, rondas de review, minutos humanos por PR, costo por PR. Nadie ha publicado estos números; los tuyos decidirán si sigues.

## Supuestos usados

- Runtimes disponibles: Claude Code (Max/Team) + OpenAI Codex + Cursor/Gemini CLI/OpenClaw.
- Integraciones: GitHub + Jira + Slack.
- Despliegue directo a VPS con Docker (con `test-drive` local de un día como mínimo).
- Se evalúan los tres escenarios a la vez; las fases 0–2 del roadmap son comunes.
