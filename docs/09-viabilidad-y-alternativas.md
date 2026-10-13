# 09 · ¿Es viable? Veredicto y alternativas

## Veredicto en una frase

**Sí es viable como capa de gobierno y coordinación sobre los agentes que ya usas (Claude Code, Codex, Cursor), en un piloto aislado y con presupuesto duro; no es viable todavía como columna vertebral de la entrega a clientes.** Reevalúa en 6 meses.

## Scorecard

| Dimensión | Nota (1–5) | Evidencia |
|---|---|---|
| Funcionalidad de gobierno (presupuestos, aprobaciones, auditoría, org) | ★★★★★ | Nadie más combina todo esto en un producto MIT self-hosted; la propia "guía del comprador" del sector lo confirma (Tasklet es el único otro con topes por agente) |
| Integración con tu stack (Claude Code + Codex + Cursor + Gemini + OpenClaw) | ★★★★★ | Adapters nativos para todos; suscripciones y API keys |
| GitHub | ★★★★☆ | Worktrees, GitHub App, skill de PR, webhooks, Code Review Bot (experimental); worktrees aún tras flag experimental con bugs abiertos |
| Jira | ★★☆☆☆ | Sólo acceso MCP (leer/comentar/transicionar); sin sync bidireccional; ⚪ en roadmap sin fecha (Linear igual, aunque su conector ya no exige app OAuth propia) |
| Slack | ★★★☆☆ | Reconstruido en oct (chat bidireccional, herramientas de Slack para agentes) pero sigue experimental y marcado "no completamente funcional"; plugin comunitario cubre notificaciones/aprobaciones |
| Madurez / estabilidad | ★★☆☆☆ | 7 meses de vida; tres releases con breaking changes en tres semanas (full auto por defecto, menciones ya no despiertan, locks nuevos que exigen drenar procesos); ~3k issues abiertas; bugs de concurrencia y presupuesto. A favor: mejor manejo de loops y runs silenciosos en 1005/1012 |
| Seguridad | ★★☆☆☆ | 3 RCE críticos en 2026 (corregidos, ninguno nuevo desde julio); **full auto por defecto** desde 1001.0; agentes sin sandbox por defecto; secretos filtrados en UI (#7601 abierta). A favor: grants aplicados literalmente y gateways MCP que rechazan tokens ajenos (1012.0) |
| Sostenibilidad del proyecto | ★★☆☆☆ | Un mantenedor con el 61 % de los commits (dos personas firman 95 de los últimos 100), 797 issues abiertas vs 95 cerradas en 30 días, financiación no divulgada, Cloud en waitlist, el único hosting gestionado (Paperclip.inc) cerró el 2 de octubre |
| Costo total | ★★★★☆ | Software gratis; infra US$20–40/mes; modelos US$100–500/mes según disciplina; cero lock-in (export a markdown) |
| Comunidad y documentación | ★★★★☆ | Docs extensas y actualizadas, blog oficial con guías de operación, 8k en Discord, 16 plantillas oficiales de empresas, decenas de plugins nuevos (watchdog, approvals Slack/Telegram, usage, Gitea, codegraph), 3.9M vistas en YouTube; pero mucho ruido y colisión de nombres |

**Promedio ponderado para tu caso: 3.3/5 → "piloto sí, apuesta única no".**

## Riesgos y cómo los neutralizas

| Riesgo | Probabilidad | Mitigación |
|---|---|---|
| El proyecto se estanca o el mantenedor se quema | Media | Export a markdown = cero lock-in; tu inversión real está en AGENTS.md/skills, que son portables a Multica, Symphony o scripts propios |
| Breaking change rompe producción | Alta (3 releases con breaking changes en 3 semanas) | Versión fijada, staging con `beta`, `paperclipai update` con backup automático, export antes de subir, leer release notes (1012.0 exige drenar procesos) |
| Runaway spend | Media (alta si mal configurado) | Hard stop en 3 niveles + `maxConcurrentRuns: 1` + límites en consola del proveedor + ICs on-demand |
| Incidente de seguridad | Media | ≥ 2026.1012.0, Tailscale, strict mode, permisos restrictivos explícitos (full auto es el default), sandbox para input hostil, credenciales de prod fuera de los agentes |
| PRs mediocres consumen tiempo senior | Alta al inicio | Issues `agent-ready` acotados, review agent de otra familia, métricas de rechazo, retro quincenal |
| Fuga entre clientes (agencia) | Baja si se aísla | Una org por cliente, secrets por org, sandbox/contenedor por cliente |

## Alternativas: mapa del mercado (sept. 2026)

### A. "Control plane" / equipo de agentes (competidores directos)

| Producto | Qué es | Licencia / precio | Madurez | Vs Paperclip |
|---|---|---|---|---|
| **Multica** — multica.ai | Tablero humanos + agentes, autodetecta 11 CLIs (Claude Code, Codex, Cursor, Copilot CLI, Gemini, Hermes, Kimi, Kiro, OpenCode, OpenClaw, Pi), squads, skills | Source-available (Apache 2.0 + condiciones), gratis self-host; telemetría anónima desde v0.4.44 | 52.6k ★; v0.5.0 → v0.6.1 entre 18 sep y 1 oct: **protección contra runaway y check-ins** (wakeup v2), cierre automático de issues al fusionar el PR, fuentes Gitea | El competidor más cercano y más simple; sin la metáfora de "empresa"; presupuestos y aprobaciones más débiles; fuerte foco en Lark/WeCom/DingTalk |
| **Zeron** — github.com/zeronsh/zeron | Control plane nativo en Rust para Claude Code, Codex, Cursor y Devin | OSS | 3.2k ★, nuevo, el de crecimiento más rápido en octubre | Vigilar; aún sin gobierno comparable |
| **OpenAI Symphony** — github.com/openai/symphony | Spec + referencia en Elixir: Linear → un workspace Codex por issue, reinicia estancados, aterriza PRs tras CI/review | Apache-2.0, gratis + plan Codex | 27.6k ★, "engineering preview", sin releases desde el 15 sep (v0.0.3); OpenAI empuja "Dots" en ChatGPT (agentes siempre activos que delegan a Codex) | Sin UI, org ni presupuestos; forks añaden Claude Code y Jira. Ideal si tu backlog está en Linear y usas Codex |
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
| **Claude Code** (CLI, web, agent teams, subagents, routines) | GitHub Actions + Agent HQ; Slack; Linear vía Cyrus (OSS); lee `AGENTS.md` si no hay `CLAUDE.md` (2.1.277) | Pro US$20, Max US$100/200, Team US$20/100 por asiento; desde el 7 oct Max/Team incluyen créditos mensuales de API | Complemento: es el adapter principal; agent teams siguen experimentales |
| **Claude Managed Agents** | Vía tu código; workflows dinámicos (beta 9 oct, hasta 1,000 agentes por run) | tokens + US$0.08/hora activa (beta) | Runtime alternativo vía adapter HTTP; absorbe el patrón "coordinador" en el vendor |
| **OpenAI Codex** (cloud + CLI) | Linear assignable agent, Agent HQ, Symphony; entornos cloud reutilizables; Codex Security (preview) escanea repos y propone PRs de parche | Plus US$20, Pro US$100/200/500 (repricing 29 sep; GPT-5.5 se retira de Codex el 14 oct) | Complemento (adapter) |
| **GitHub Agent HQ / Copilot coding agent** | Asignar un issue a Copilot, Claude o Codex; agente cloud y Chat unificados (28 sep); code review por API como check requerido (2 oct); sandboxing local GA; Universe el 28–29 oct traerá novedades | Business US$19, Enterprise US$39 + créditos (1 crédito = US$0.01; facturación por adelantado desde el 1 oct) | **Reemplazo parcial** para equipos GitHub-céntricos sin necesidad de presupuestos/org |
| **Linear Agents** | Asignar issues a Cursor, Codex, Devin…; el propio Linear Agent ya escribe código (Coding Sessions, con routing adaptativo de modelos y secretos de entorno desde el 24 sep) | Incluido en Linear | Front door alternativo; Paperclip tiene conector Linear |
| **Atlassian Rovo Dev** | Agente nativo de Jira; sesiones visibles en el work item, agentes asignables a columnas del tablero, MCP gobernado por Data Security Policy (sep) | US$20/dev/mes con 2,000 créditos | Si Jira es tu centro, es el camino "cero infra" |
| **Devin** (Cognition) | Linear, Jira, Slack, GitHub, Sentry | Core US$20 + US$2.25/ACU; Team ≈ US$500/mes | "Contratar un ingeniero IA" cerrado; caro a escala |
| **Factory Droids** | CLI/CI, enterprise | US$20–200 + enterprise | Complemento/reemplazo enterprise |
| **Cursor Cloud agents / Cursor Projects** | Asignar issue de Linear a @cursor; triggers Slack/Teams; **Projects** (10 sep): coordinador que planifica y delega a subagentes cloud, versión vendor del patrón "CEO" | Pro US$20, Teams US$40/usuario | Complemento (adapter `cursor`) o reemplazo del coordinador para equipos Cursor |
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
Por qué no otros: Symphony es Codex/Linear-first y está quieto; Multica ya tiene protección contra runaway pero no presupuestos ni aprobaciones comparables; Managed Agents está en beta y añade costo de hosting; Cursor Projects sólo si todo el equipo vive en Cursor. Si en 3 meses el piloto muestra que sólo usas "issue → PR", baja a Agent HQ y apaga Paperclip.

**B · Solo founder / 1–3 personas**
Paperclip es la mejor opción de su categoría: el modelo CEO → equipo, presupuestos y aprobaciones es justo lo que necesitas cuando tú eres el único humano. Corre sobre suscripciones Max/Pro, 3–4 agentes, CEO 1×/día. Alternativa más simple si sólo quieres PRs: Codex cloud + Linear Agents o Symphony.

**C · Agencia multi-cliente**
Paperclip para clientes que aceptan self-hosting y tu proceso: una org por cliente, export/import como plantilla, costos atribuidos por org para facturar. Para clientes con **cumplimiento estricto** (SOC2, SSO, responsabilidad de proveedor) NO lo uses como herramienta primaria: GitHub Copilot Enterprise + Agent HQ, Devin Team o Factory, Rovo Dev si Jira, y Paperclip sólo interno detrás de Tailscale. Sus funciones de RBAC/SSO son nuevas y no auditadas.

**El punto de partida más simple para cualquiera de los tres**: GitHub Agent HQ + asignación de issues en Jira/Linear (cero infra). Añade Paperclip cuando choques con los problemas que su README enumera: 20 sesiones sin rastrear, gasto descontrolado, sin trazabilidad.

## Criterios para reevaluar en marzo de 2027

- ¿Qué anunció GitHub en Universe (28–29 oct) sobre Agent HQ? ¿Cubre presupuestos y aprobaciones multi-vendor?
- ¿Paperclip Cloud salió de waitlist con precio y SLA?
- ¿Se divulgó equipo/financiación o creció el núcleo de mantenedores (hoy 1 con el 61 % de los commits)?
- ¿Tasa de cierre de issues > tasa de apertura? (hoy 95 cerradas vs 797 abiertas en 30 días)
- ¿Se resolvió la expiración de suscripciones a las 8 h (#14182) y el fallo ACP de Claude Code (#14093)?
- ¿Sync bidireccional Jira/Linear en core? ¿Workspaces aislados fuera de experimental?
- ¿Nuevos CVEs críticos?
- ¿Tus métricas de piloto (costo por PR fusionado, % de issues cerrados por agentes, minutos humanos por PR) justifican el costo de operación?
