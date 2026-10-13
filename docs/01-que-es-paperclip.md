# 01 · ¿Qué es Paperclip?

> Fuente principal: [README oficial](https://github.com/paperclipai/paperclip), [docs.paperclip.ing](https://docs.paperclip.ing/), [paperclip.ing](https://paperclip.ing/). Datos verificados el 28 de septiembre de 2026.

## En una frase

Paperclip es una **capa de gestión (control plane) open source para equipos de agentes de IA**. No es un agente ni un modelo: es la "empresa" que organiza a los agentes que ya tienes (Claude Code, Codex, Gemini CLI, Cursor, OpenClaw, etc.) en un **organigrama con jefes, metas, tareas, presupuestos y aprobaciones**.

Su propio lema lo resume: *"Si OpenClaw es un empleado, Paperclip es la empresa"* y *"Gestiona objetivos de negocio, no pull requests"*.

## Datos duros (sept. 2026)

| Dato | Valor |
|---|---|
| Fabricante | Paperclip Labs, Inc. (equipo pequeño, EE.UU.) |
| Licencia | MIT, 100 % self-hosted, sin cuenta obligatoria |
| GitHub | ~92.6k estrellas · 15.9k forks · 4,620 commits · ~2.5k issues abiertas |
| Versión estable | `2026.916.1` (21 sep 2026); canales canary → nightly → beta → stable |
| Cadencia | Release estable cada 1–2 semanas (releases de 175–500 commits) |
| Stack | Node.js 24.11+, React, Express 5, Drizzle, PostgreSQL (embebido o externo), pnpm monorepo |
| Instalación | `npx paperclipai onboard --yes` → `http://localhost:3100` |
| Precio | Gratis. Tú pagas directamente a Anthropic/OpenAI/etc. Hay "Paperclip Cloud" en waitlist, sin precio publicado |

## Los 7 conceptos que hay que entender

1. **Company / Organización**: contenedor aislado con una meta, un equipo de agentes, un tablero de tareas y un presupuesto. Una instancia puede tener varias.
2. **Agente**: "empleado de IA" = rol + jefe + presupuesto mensual + adapter (el runtime real que ejecuta el trabajo). Cada agente tiene **exactamente un jefe**; el CEO reporta a la "junta" (tú).
3. **CEO agent**: el primer agente que creas. En su primer heartbeat propone una estrategia, tú la apruebas, y él crea tareas, delega y puede pedir contratar reportes.
4. **Goals (metas)**: jerarquía Misión → Meta de proyecto → Meta de agente → Tarea. Toda tarea arrastra su "ancestría" de metas.
5. **Issues / Tareas**: tickets con ciclo `backlog → todo → in_progress → in_review → done` (+ `blocked`, `cancelled`). Los agentes hacen *checkout* atómico de una tarea (lock de ejecución) para que dos agentes no pisen lo mismo.
6. **Heartbeat**: ventana de ejecución programada. El agente "despierta" (por timer, asignación, mención `@`, aprobación resuelta o rutina), revisa su bandeja, trabaja y deja transcript + costo.
7. **Budgets / Approvals**: tope mensual por empresa y por agente (aviso al 80 %, pausa automática al 100 %). Gates de gobierno: aprobación de estrategia, de contratación, de gasto, de planes, revisiones.

## Cómo fluye el trabajo (ingeniería)

```
Tú (junta) ──aprueba estrategia──▶ CEO agent
                                     │ crea metas + tareas
                                     ▼
                                CTO agent ── descompone, asigna por especialidad
                                     │
             ┌───────────────────────┼───────────────────────┐
             ▼                       ▼                       ▼
     Coder (Codex/Claude)      QA (Claude)             Designer (Hermes)
     worktree aislado          criterios de aceptación  wireframes
     branch + commit + PR      screenshots/tests
             │
             ▼
     GitHub PR ── CI ── revisión HUMANA ── merge ── issue → done
```

Cada ejecución queda registrada con transcript, costo por proveedor/modelo, y actividad auditable. Watchdogs detectan runs estancados; los *review gates* retienen el merge hasta que un humano (o agente revisor) aprueba.

## Qué NO es (según sus propios docs)

- **No es un chatbot** ni una herramienta de un solo agente ("si tienes un agente, probablemente no necesitas Paperclip").
- **No es una herramienta de code review**: "trae tu propio proceso de revisión".
- **No corre modelos**: no hay inferencia propia; es *bring-your-own-agent*.
- **No sincroniza tickets bidireccionalmente** con Jira/Linear todavía (está en roadmap). Hoy los conectores de Jira/Slack son acceso vía MCP para los agentes.

## Adapters soportados (runtimes)

| Tipo | Runtime | Estado |
|---|---|---|
| `claude_local` | Claude Code (ACP o CLI) | Recomendado, estable |
| `codex_local` | OpenAI Codex CLI | Recomendado, estable |
| `gemini_local` | Gemini CLI | Estable |
| `cursor` | Cursor Agent CLI / Cursor Cloud | Estable |
| `opencode_local`, `pi_local`, `hermes_local`, `grok_local`, `kimi_local` | Otros CLIs | Estable |
| `openclaw_gateway` | OpenClaw vía WebSocket | Funciona, sólo por API (dropdown dice "coming soon") |
| `process` / `http` | Script propio / webhook | Funciona, sólo por API |
| Sandboxes | e2b, Daytona, Modal, Cloudflare, Novita, Kubernetes | Plugins |

Autenticación: acepta **API keys** o **suscripciones** (login de Claude Pro/Max y ChatGPT/Codex) vía "Connections" desde la 2026.916.0. Esto importa mucho para el costo: puedes correr agentes contra tu suscripción Max en lugar de pagar por token.

## Integraciones relevantes para un equipo de software

- **GitHub**: PAT fine-grained, GitHub App, o conexión gestionada (webhooks de PR sincronizan estado). Worktrees por issue, branch por issue, skill `github-pr-workflow`.
- **Jira**: MCP remoto de Atlassian Cloud (buscar, leer, comentar, transicionar). Sin sync bidireccional; escrituras en modo "ask first".
- **Slack**: dos conexiones: (a) tool MCP para agentes (marcada como "no completamente funcional" en 2026.916.0), (b) canal de chat experimental donde menciones/DMs se convierten en tareas. Notificaciones salientes se hacen con una **rutina + agente notificador** (no hay webhooks salientes nativos).
- **Otros**: Linear, Sentry, PostHog, Supabase, Cloudflare, Netlify, Stripe, Notion, Google Workspace, Zapier, MCP Tool Gateway gobernado (50 conectores en catálogo).
