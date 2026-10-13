# 09 · ¿Es viable? Veredicto y alternativas

## Veredicto en una frase

**Sí es viable como capa de gobierno y coordinación sobre los agentes que ya usas (Claude Code, Codex, Cursor), en un piloto aislado y con presupuesto duro; no es viable todavía como columna vertebral de la entrega a clientes.** Reevalúa en 6 meses.

## Scorecard

| Dimensión | Nota (1–5) | Evidencia |
|---|---|---|
| Funcionalidad de gobierno (presupuestos, aprobaciones, auditoría, org) | ★★★★★ | Nadie más combina todo esto en un producto MIT self-hosted; la propia "guía del comprador" del sector lo confirma (Tasklet es el único otro con topes por agente) |
| Integración con tu stack (Claude Code + Codex + Cursor + Gemini + OpenClaw) | ★★★★★ | Adapters nativos para todos; suscripciones y API keys |
| GitHub | ★★★★☆ | Worktrees, GitHub App, skill de PR, webhooks; worktrees aún tras flag experimental con bugs abiertos |
| Jira | ★★☆☆☆ | Sólo acceso MCP (leer/comentar/transicionar); sin sync bidireccional; en roadmap |
| Slack | ★★★☆☆ | Chat connector experimental; tool MCP "no completamente funcional"; plugin comunitario cubre notificaciones/aprobaciones |
| Madurez / estabilidad | ★★☆☆☆ | 6 meses de vida, releases de 500 commits con breaking changes, 2,542 issues abiertas, bugs de concurrencia y presupuesto |
| Seguridad | ★★☆☆☆ | 3 RCE críticos en 2026 (corregidos); agentes sin sandbox por defecto; secretos filtrados en UI (#7601 abierta) |
| Sostenibilidad del proyecto | ★★☆☆☆ | Un mantenedor dominante, financiación no divulgada, Cloud en waitlist, único hosting gestionado cerrando |
| Costo total | ★★★★☆ | Software gratis; infra US$20–40/mes; modelos US$100–500/mes según disciplina; cero lock-in (export a markdown) |
| Comunidad y documentación | ★★★★☆ | Docs extensas y actualizadas, 8k en Discord, plantillas oficiales de empresas, plugins; pero mucho ruido y colisión de nombres |

**Promedio ponderado para tu caso: 3.3/5 → "piloto sí, apuesta única no".**

## Riesgos y cómo los neutralizas

| Riesgo | Probabilidad | Mitigación |
|---|---|---|
| El proyecto se estanca o el mantenedor se quema | Media | Export a markdown = cero lock-in; tu inversión real está en AGENTS.md/skills, que son portables a Multica, Symphony o scripts propios |
| Breaking change rompe producción | Alta | Versión fijada, staging con `beta`, export antes de subir |
| Runaway spend | Media (alta si mal configurado) | Hard stop en 3 niveles + `maxConcurrentRuns: 1` + límites en consola del proveedor + ICs on-demand |
| Incidente de seguridad | Media | ≥ 2026.916.1, Tailscale, strict mode, sandbox para input hostil, credenciales de prod fuera de los agentes |
| PRs mediocres consumen tiempo senior | Alta al inicio | Issues `agent-ready` acotados, review agent de otra familia, métricas de rechazo, retro quincenal |
| Fuga entre clientes (agencia) | Baja si se aísla | Una org por cliente, secrets por org, sandbox/contenedor por cliente |

## Alternativas: mapa del mercado (sept. 2026)

### A. "Control plane" / equipo de agentes (competidores directos)

| Producto | Qué es | Licencia / precio | Madurez | Vs Paperclip |
|---|---|---|---|---|
| **Multica** — multica.ai | Tablero humanos + agentes, autodetecta 11 CLIs (Claude Code, Codex, Cursor, Copilot CLI, Gemini, Hermes, Kimi, Kiro, OpenCode, OpenClaw, Pi), squads, skills | Source-available (Apache 2.0 + condiciones), gratis self-host | 51.6k ★, misma edad | El competidor más cercano y más simple; sin la metáfora de "empresa"; presupuestos y aprobaciones más débiles |
| **OpenAI Symphony** — github.com/openai/symphony | Spec + referencia en Elixir: Linear → un workspace Codex por issue, reinicia estancados, aterriza PRs tras CI/review | Apache-2.0, gratis + plan Codex | 27.5k ★, "engineering preview", OpenAI no lo mantendrá como producto | Sin UI, org ni presupuestos; forks añaden Claude Code y Jira. Ideal si tu backlog está en Linear y usas Codex |
| **Agent Orchestrator** — Untrivial | Supervisar equipos de agentes de código de plan a merge, cualquier harness | Apache-2.0 | 12.5k ★ | Más ligero, sólo dev |
| **Gas Town** — Steve Yegge | Gestor de workspaces multi-agente con identidades persistentes; cloud en Kilo | MIT | 18k ★, experimental | Para tinkerers |
| **OpenClaw + Mission Control** | Asistente personal (WhatsApp/Telegram/Slack) + dashboards comunitarios | OSS, 390k ★ | Dashboard principal archivado ago 2026 | Complemento: "OpenClaw es el empleado, Paperclip la empresa" |
| **LangSmith Fleet / Tasklet / LobeHub** | Plataformas hosteadas de gestión de agentes | Propietarias | Variada | Fleet: RBAC y gateway enterprise, self-host en beta; Tasklet: topes por agente; LobeHub: controles no verificados |
| CrewAI, Microsoft Agent Framework, LangGraph, OpenAI Agents SDK, Claude Agent SDK, Google ADK | Frameworks para **construir** agentes en código | MIT/Apache | Maduros | No son un control plane; sólo si quieres construir tu propio orquestador |
| MetaGPT, ChatDev | "Empresas de software virtuales" de investigación | MIT/Apache | Estancados (MetaGPT sin push desde enero) | No para producción |
| Lindy, Relevance AI, Dust | "Empleados de IA" SaaS para ventas/soporte/ops | US$24–200/usuario | Maduros | Irrelevantes para backlog de ingeniería |

### B. Plataformas de ingeniería con agentes (lo que ya tienes o puedes comprar)

| Producto | Autonomía sobre backlog | Precio 2026 | Relación con Paperclip |
|---|---|---|---|
| **Claude Code** (CLI, web, agent teams, subagents, routines) | GitHub Actions + Agent HQ; Slack; Linear vía Cyrus (OSS) | Pro US$20, Max US$100/200, Team US$20/100 por asiento | Complemento: es el adapter principal |
| **Claude Managed Agents** | Vía tu código | tokens + US$0.08/hora activa (beta) | Runtime alternativo vía adapter HTTP |
| **OpenAI Codex** (cloud + CLI) | Linear assignable agent, Agent HQ, Symphony | Plus US$20, Pro US$100/200 | Complemento (adapter) |
| **GitHub Agent HQ / Copilot coding agent** | Asignar un issue a Copilot, Claude o Codex; "Mission Control" en GitHub | Business US$19, Enterprise US$39 + créditos | **Reemplazo parcial** para equipos GitHub-céntricos sin necesidad de presupuestos/org |
| **Linear Agents** | Asignar issues a Cursor, Codex, Devin… | Incluido en Linear | Front door alternativo; Paperclip tiene conector Linear |
| **Atlassian Rovo Dev** | Agente nativo de Jira | US$20/dev/mes con 2,000 créditos | Si Jira es tu centro, es el camino "cero infra" |
| **Devin** (Cognition) | Linear, Jira, Slack, GitHub, Sentry | Core US$20 + US$2.25/ACU; Team ≈ US$500/mes | "Contratar un ingeniero IA" cerrado; caro a escala |
| **Factory Droids** | CLI/CI, enterprise | US$20–200 + enterprise | Complemento/reemplazo enterprise |
| **Cursor Cloud agents** | Asignar issue de Linear a @cursor; triggers Slack/Teams | Pro US$20, Teams US$40/usuario | Complemento (adapter `cursor`) |
| **Sentry Seer** | Root cause + PR de fix desde errores | US$40/mes por contribuidor activo | Complemento (conector Sentry) |
| Conductor, Superset, Vibe Kanban, Warp Oz | Cockpits locales de worktrees paralelos por desarrollador | Gratis–US$50/mes (Vibe Kanban huérfano tras cierre de Bloop) | Complementan: son por persona, no por equipo |
| Google Jules, Amazon Kiro, Kilo Code, OpenHands | Agentes de código con distintos grados de autonomía | Gratis–US$200 | OpenHands (89k ★, MIT) es runtime alternativo OSS |

## Comparación directa (lo que importa para ti)

| | Paperclip | Multica | Symphony | GitHub Agent HQ | Linear Agents + Cursor/Codex | Rovo Dev (Jira) | Devin |
|---|---|---|---|---|---|---|---|
| Open source / self-host | MIT / sí | Source-avail. / sí | Apache / sí | No | No | No | No |
| Org de agentes con jefes y metas | Sí | Parcial (squads) | No | No | No | No | No |
| Presupuestos por agente con parada | Sí | Básico | No | Créditos por plan | Por asiento | Créditos | ACUs |
| Aprobaciones humanas / review gates | Sí (runtime) | Revisión de tareas | Sólo PR | Sólo PR | Sólo PR | Sólo PR | Plan approval + PR |
| GitHub / Jira / Slack | Sí / MCP / experimental | Sí / – / Sí | Linear (forks Jira) | Nativo / – / Sí | Nativo / – / Sí | – / Nativo / Sí | Sí / Sí / Sí |
| Multi-runtime (Claude+Codex+Cursor+…) | Sí (12 adapters) | Sí (11) | Codex (+forks) | Claude/Codex/Copilot | Cursor/Codex/Devin | Rovo | Devin |
| Madurez | 6 meses, churn | 6 meses | Preview | Preview público, Microsoft | GA | GA | GA, enterprise |
| Costo de infra | VPS + tu ops | VPS + tu ops | VPS + tu ops | 0 | 0 | 0 | 0 |

## Recomendación por escenario

**A · Empresa de software 5–20 devs (tu caso principal)**
Stack recomendado: **Claude Code (Team/Max) + Codex** para el trabajo interactivo; **GitHub Agent HQ** para "asigna issue → PR" sin infraestructura; **Paperclip self-hosted** como runner 24/7 del backlog `agent-ready`, guardián de presupuesto e historial auditable, con conector GitHub y agente-espejo de Jira; Slack por plugin comunitario. Añade Conductor o Superset por desarrollador para worktrees paralelos locales.
Por qué no otros: Symphony es Codex/Linear-first; Multica no tiene presupuestos ni aprobaciones; Managed Agents está en beta y añade costo de hosting.

**B · Solo founder / 1–3 personas**
Paperclip es la mejor opción de su categoría: el modelo CEO → equipo, presupuestos y aprobaciones es justo lo que necesitas cuando tú eres el único humano. Corre sobre suscripciones Max/Pro, 3–4 agentes, CEO 1×/día. Alternativa más simple si sólo quieres PRs: Codex cloud + Linear Agents o Symphony.

**C · Agencia multi-cliente**
Paperclip para clientes que aceptan self-hosting y tu proceso: una org por cliente, export/import como plantilla, costos atribuidos por org para facturar. Para clientes con **cumplimiento estricto** (SOC2, SSO, responsabilidad de proveedor) NO lo uses como herramienta primaria: GitHub Copilot Enterprise + Agent HQ, Devin Team o Factory, Rovo Dev si Jira, y Paperclip sólo interno detrás de Tailscale. Sus funciones de RBAC/SSO son nuevas y no auditadas.

**El punto de partida más simple para cualquiera de los tres**: GitHub Agent HQ + asignación de issues en Jira/Linear (cero infra). Añade Paperclip cuando choques con los problemas que su README enumera: 20 sesiones sin rastrear, gasto descontrolado, sin trazabilidad.

## Criterios para reevaluar en marzo de 2027

- ¿Paperclip Cloud salió de waitlist con precio y SLA?
- ¿Se divulgó equipo/financiación o creció el núcleo de mantenedores (hoy 1 dominante)?
- ¿Tasa de cierre de issues > tasa de apertura?
- ¿Sync bidireccional Jira/Linear en core? ¿Workspaces aislados fuera de experimental?
- ¿Nuevos CVEs críticos?
- ¿Tus métricas de piloto (costo por PR fusionado, % de issues cerrados por agentes, minutos humanos por PR) justifican el costo de operación?
