# 01 · ¿Qué es Paperclip?

> Fuente principal: [README oficial](https://github.com/paperclipai/paperclip), [docs.paperclip.ing](https://docs.paperclip.ing/), [paperclip.ing](https://paperclip.ing/). Datos verificados el 12 de octubre de 2026.

## En una frase

Paperclip es una **capa de gestión (control plane) open source para equipos de agentes de IA**. No es un agente ni un modelo: es la "empresa" que organiza a los agentes que ya tienes (Claude Code, Codex, Gemini CLI, Cursor, OpenClaw, etc.) en un **organigrama con jefes, metas, tareas, presupuestos y aprobaciones**.

Su propio lema lo resume: *"Si OpenClaw es un empleado, Paperclip es la empresa"* y *"Gestiona objetivos de negocio, no pull requests"*.

## Datos duros (12 oct. 2026)

| Dato | Valor |
|---|---|
| Fabricante | Paperclip Labs, Inc. (equipo pequeño, EE.UU.) |
| Licencia | MIT, 100 % self-hosted, sin cuenta obligatoria |
| GitHub | ~101k estrellas · 17k forks · 5,059 commits · ~3k issues abiertas · ~3k PRs abiertos |
| Versión estable | `2026.1012.0` (12 oct 2026); tres releases en tres semanas (1001.0, 1005.0, 1012.0 = 386 commits); canales canary → nightly → beta → stable |
| Cadencia | Release estable cada 1–2 semanas (releases de 175–500 commits) |
| Stack | Node.js 24.11+, React, Express 5, Drizzle, PostgreSQL (embebido o externo), pnpm monorepo |
| Instalación | `npx paperclipai@latest onboard --yes` → `http://localhost:3100` |
| Precio | Gratis. Tú pagas directamente a Anthropic/OpenAI/etc. Hay "Paperclip Cloud" en waitlist, sin precio publicado |

## Los 7 conceptos que hay que entender

1. **Company / Organización**: contenedor aislado con una meta, un equipo de agentes, un tablero de tareas y un presupuesto. Una instancia puede tener varias.
2. **Agente**: "empleado de IA" = rol + jefe + presupuesto mensual + adapter (el runtime real que ejecuta el trabajo). Cada agente tiene **exactamente un jefe**; el CEO reporta a la "junta" (tú).
3. **CEO agent**: el primer agente que creas. En su primer heartbeat propone una estrategia, tú la apruebas, y él crea tareas, delega y puede pedir contratar reportes.
4. **Goals (metas)**: jerarquía Misión → Meta de proyecto → Meta de agente → Tarea. Toda tarea arrastra su "ancestría" de metas.
5. **Issues / Tareas**: tickets con ciclo `backlog → todo → in_progress → in_review → done` (+ `blocked`, `cancelled`). Los agentes hacen *checkout* atómico de una tarea (lock de ejecución) para que dos agentes no pisen lo mismo.
6. **Heartbeat**: ventana de ejecución programada. El agente "despierta" (por timer, asignación, comentario en su tarea, aprobación resuelta o rutina), revisa su bandeja, trabaja y deja transcript + costo. Desde 2026.1005.0 una mención `@` **ya no** inicia un run.
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
- **No sincroniza tickets bidireccionalmente** con Jira/Linear todavía (sigue en roadmap como "bring-your-own-ticket-system"). Hoy los conectores de Jira/Linear son acceso vía MCP para los agentes.
- **No bloquea las herramientas nativas del runtime** (shell, archivos, red dentro de Claude Code/Codex). Desde 2026.1001.0 los harnesses corren en **full auto por defecto** (`--dangerously-skip-permissions` en Claude Code; Codex en ACP conserva su sandbox de workspace, en `engine: cli` lo salta). La política de ejecución gobierna transiciones de tareas y el tool gateway sólo las herramientas conectadas (MCP/HTTP).

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

Autenticación: acepta **API keys** o **suscripciones** (login de Claude Pro/Max y ChatGPT/Codex) vía "Connections" desde la 2026.916.0; desde 2026.1012.0 la acción "Check usage" lee los límites de cuota de Codex, Claude, Grok y OpenRouter. Esto importa mucho para el costo: puedes correr agentes contra tu suscripción Max en lugar de pagar por token. Modelos soportados al 12 oct: Claude Sonnet 5.5 / Opus / Fable 5.1 (Claude Code 2.1.286), GPT-6.1 Sol y GPT-5.6 (Codex 0.160), Bedrock.

**Assistant Connection (MCP)** (2026.1012.0, experimental `enablePublicMcp`): tu Claude Code o Codex se conecta a Paperclip **como tú** (`claude mcp add --transport http paperclip https://<host>/mcp/paperclip`) y puede listar agentes, buscar/crear tareas, comentar, leer entregables y ver aprobaciones pendientes. Es la forma de operar Paperclip sin salir del terminal.

## Integraciones relevantes para un equipo de software

- **GitHub**: PAT fine-grained, GitHub App, o conexión gestionada (webhooks de PR sincronizan estado). Worktrees por issue, branch por issue, skill `github-pr-workflow`.
- **Jira**: MCP remoto de Atlassian Cloud (buscar, leer, comentar, transicionar; puede exponer Confluence). Sin sync bidireccional; escrituras en modo "ask first". **Linear**: conector MCP con scopes read/write, ya no requiere registrar tu propia app OAuth.
- **Slack**: reconstruido en 2026.1001.0/1005.0. Canal de chat bidireccional (flag experimental *Chat connectors* + HTTPS público): una mención en un hilo = una tarea, las respuestas del board vuelven al hilo y los agentes tienen herramientas de Slack en tareas y rutinas. La página oficial aún avisa "no completamente funcional". Detalle de configuración en la fase 5 del [roadmap](03-roadmap-0-a-100.md).
- **GitHub Code Review Bot** (experimental, parte de Chat connectors): GitHub App que revisa PRs por mención o automáticamente, publica un comentario con puntaje (1–5), hallazgos inline y un *check* "Paperclip Review" que puedes exigir en un ruleset; `APPROVE`/`REQUEST_CHANGES` apagados por defecto.
- **Otros**: Sentry, PostHog, Supabase, Neon, Railway (deploy/rollback), Browser Use Cloud (navegador en vivo dentro de la tarea), Cloudflare, Netlify, Stripe, Notion, Fireflies, AgentMail, agregadores MCP (Zapier, Arcade, Composio Connect, Executor, activos por defecto desde 1005.0), conectores de memoria experimentales (Mem0, Zep, Supermemory, Cognee, Honcho), MCP Tool Gateway gobernado (61 entradas en catálogo). Google Workspace está oculto temporalmente pendiente de verificación OAuth.
