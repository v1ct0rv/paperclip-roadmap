# 02 · Tendencias, tracción y lo que dice la comunidad

> Escaneo del 28 de septiembre de 2026 sobre X, YouTube, GitHub, Hacker News, blogs y podcasts, **actualizado el 12 de octubre de 2026**. Reddit bloquea el acceso programático (403 "whoa there, pardner" sin sesión u OAuth), así que los hilos se leyeron a través del archivo público pullpush.io; sus puntajes pueden estar desactualizados.

## Qué cambió entre el 20 de septiembre y el 12 de octubre

| Fecha | Release / hecho | Por qué importa para un equipo de software |
|---|---|---|
| 1 oct | **v2026.1001.0** (77 commits) | Harnesses en **full auto por defecto**; broker Composio retirado (sin migración automática); Slack reconstruido con wizard; GitHub MCP con toolset de Actions y setup guiado de review bot; Railway, You.com, agregadores MCP (Zapier/Arcade/Composio Connect/Executor) |
| 5 oct | **v2026.1005.0** (179 commits) | **Una mención `@` ya no inicia un run**; Slack bidireccional (respuestas del board al hilo, herramientas de Slack para agentes); GitHub Code Review Bot como conector propio; Browser Use Cloud; 5 conectores de memoria experimentales; skills sincronizados desde repos GitHub; AGENTS.md persistente entre tareas; fix de crash-loop en runs rechazados |
| 7 oct | Blog: *Harness vs Framework vs Runtime vs Control Plane* y *Self-Hosted AI Agent Stacks* | Paperclip explica por escrito que **no puede bloquear el shell del runtime** y que todas las llamadas a modelos salen a Anthropic/OpenAI (no probaron un setup 100 % on-prem) |
| 12 oct | **v2026.1012.0** (130 commits) | **Assistant Connection (MCP)**: Claude Code/Codex operan Paperclip como usuario; Sonnet 5.5 y GPT-6.1 Sol; "Check usage" de cuotas; runs silenciosos marcados a 5/15 min; instrucciones por defecto del CEO reducidas a 20 palabras; nuevo protocolo de locks (hay que drenar procesos antes de subir); grants de herramientas aplicados literalmente; Neon, Asana |
| 12 oct | Blog: *Running AI Agents Overnight* | Modelo oficial de **tres carriles** para trabajo desatendido y doctrina de presupuestos: "paran el siguiente run, no la llamada actual" |

Lo que **no** cambió: Jira/Linear siguen sin sync bidireccional (⚪ en roadmap), workspaces aislados siguen experimentales, Ollama/modelos locales sin adapter (#187 abierta), Paperclip Cloud sigue en waitlist sin precio (`/pricing` da 404), sin advisories de seguridad nuevos desde julio.

## Señales principales

### 🔥 1. La tracción es real y sigue creciendo, pero vive en X, YouTube y Discord, no en Hacker News
101,000 estrellas y 17,000 forks en poco más de 7 meses (repo creado el 2 de marzo de 2026; 92.6k el 28 de septiembre → 101k el 12 de octubre), ~120k descargas mensuales del CLI en npm, 8,005 miembros en Discord, commits diarios y tres releases en las últimas tres semanas. Trayectoria: 26k estrellas a mediados de marzo → 50k el 15 de abril → 70k en junio → 101k hoy. En cambio, ninguna de las 7 publicaciones en Hacker News pasó de 6 puntos: la comunidad "builder" clásica lo ha ignorado o lo mira con escepticismo ("el costo creciente de que la IA cuide a la IA").
Fuentes: https://github.com/paperclipai/paperclip · https://www.star-history.com/paperclipai/paperclip · https://news.ycombinator.com/item?id=47277096

### 🔥 2. Seguridad: el mayor golpe reputacional del año
12 advisories en 2026, incluidos **CVE-2026-41679** (RCE sin autenticación, CVSS 10.0, módulo de Metasploit publicado por Rapid7) y **CVE-2026-77087** (RCE drive-by por DNS rebinding contra instalaciones locales por defecto, CVSS 9.6, 22 de julio). Cubierto por The Hacker News, Infosecurity Magazine, Oasis Security y SentinelOne. Corregidos desde v2026.416.0. La crítica estructural de Oasis: "la configuración del agente es código", y el producto ejecuta CLIs sin sandbox en el host.
Fuentes: https://github.com/paperclipai/paperclip/security/advisories · https://thehackernews.com/2026/08/paperclip-ai-flaws-let-attackers-run.html · https://www.oasis.security/blog/paperclip-agent-vulnerabilities

### 🔥 3. El backlog de mantenimiento crece más rápido que el equipo
2,970 issues abiertas y 3,031 PRs abiertos al 12 de octubre. En los últimos 30 días se abrieron **797 issues y se cerraron 95** (tasa de cierre ~12 %, el backlog crece ~700/mes); se abrieron 1,940 PRs y se fusionaron 730. La concentración es extrema: el fundador seudónimo "dotta" (`cryppadotta`) firma el 61 % de todos los commits, los tres primeros contribuidores el 81 %, y de los últimos 100 commits 95 son de dos personas. El propio fundador habló en el Open Source Startup Podcast (3 ago) de "la realidad de mantener una tonelada de PRs de la comunidad".
Fuentes: https://github.com/paperclipai/paperclip/graphs/contributors · https://ossstartuppodcast.substack.com/p/episode-201-building-your-agent-army

## Quién está detrás

- **Paperclip Labs, Inc.** Fundador: "@dotta" (Nate, seudónimo; bio: "CEO Paperclip Labs. Forgotten Runes Wizards Cult. Crypto-quant"). Cofundadores reportados: Devin Foley (ex Slack/Figma) y Scott Tong (ex Pinterest), según un resumen de eWeek que no pudimos confirmar en el texto original.
- **Financiación: no divulgada** en ninguna fuente (Ry Walker Research, junio: "still no disclosed team, company, or funding").
- **Modelo de negocio**: MIT + "Paperclip Cloud" en waitlist + funciones enterprise en camino (secretos por usuario, Connections, SSO) + alianza con Ramp for Agents (tarjeta de gasto para agentes, julio).
- **Señal de alerta**: el único hosting gestionado que existía, el revendedor estonio no afiliado Paperclip.inc (€10/mes), **cierra el 2 de octubre de 2026**. Contabo y Hostinger venden imágenes VPS de un clic.
- **Reposicionamiento**: el eslogan pasó de "orquestación para empresas sin humanos" (marzo) a "la app para gestionar agentes de IA en el trabajo" (junio). La narrativa "zero-human" fue la que generó más rechazo.

## Momentos virales y cobertura

| Fecha | Qué | Alcance | Señal |
|---|---|---|---|
| 4 mar | Tweet de lanzamiento de @dotta | 2.4M vistas reportadas | 🔥 |
| 16 mar | Nick Spisak: "empresa 100 % agentes en 5 minutos" | 2.7M vistas | 🔥 |
| 26 mar | Greg Isenberg, demo en vivo con dotta (YouTube) | 385k vistas | 🔥 |
| 29 mar | Nick Saraev: "Paperclip Sucks, Actually" ("hype city") | 109k vistas | 🔥 crítica |
| 4 abr | Tech With Tim: tutorial completo | 125k vistas | 🔥 |
| 6 abr | Nick Puru: "The Truth About Zero Human Companies" | 58k | 📈 crítica |
| 15 abr | Charla de dotta en AI Engineer Europe | 50k | 📈 |
| 3 ago | Open Source Startup Podcast ep. 201 | — | 📈 |
| 24 sep | **NetworkChuck: "you need to try Paperclip RIGHT NOW!"** (departamento de IT con Claude Code, Codex, Hermes y OpenClaw; repo guía probado en 2026.916.1) | **1.34M vistas** al 12 oct; mayor impulsor de adopción del periodo | 🔥 |
| 25–27 sep | Tutoriales (Full Stack, Ferdy Korpershoek "Paperclip AI Is Insane") | 10–29k c/u | 👀 |
| 6 oct | Cuenta oficial: "100+ creadores han hecho videos, 3.9M vistas en YouTube" + programa de creadores fundadores | — | 📈 |
| 6–10 oct | AICodeKing (Claude Code + Codex), Julian Goldie (Paperclip + Hermes) | 17k · 7k | 👀 |
| 12 oct | 100k estrellas en GitHub; anuncio de v2026.1012.0 | 49 likes en X | 📈 |

Enlaces: https://www.youtube.com/watch?v=C3-4llQYT8o · https://www.youtube.com/watch?v=RHZLJhp-JMM · https://www.youtube.com/watch?v=QufcrM79snw · https://www.youtube.com/watch?v=h403btjldDQ · https://x.com/dotta/status/2029239759428780116 · https://www.youtube.com/watch?v=7RVf25Rg0Mc · https://github.com/theNetworkChuck/paperclip-guide · https://x.com/papercliping/status/2107535700463964251

Efecto secundario del video de NetworkChuck: la discusión #13999 "3M tokens gone to runaway agents" (25 sep) la abrió un espectador que siguió el video, tuvo 15–25 sesiones concurrentes **durante el onboarding**, antes de cualquier trabajo real, por cadenas de delegación; la solución fue pausar agentes, apagar heartbeats y fijar `maxTurnsPerRun` por agente. https://github.com/paperclipai/paperclip/discussions/13999

## Lo que dicen quienes lo usaron de verdad

**A favor**
- eesel (jul): "uno de los proyectos open source más impresionantes del año… un control plane, no un agente"; elogia gobierno, agnosticismo de agentes y self-hosting; critica la carga DIY de infraestructura. https://www.eesel.ai/blog/paperclip-ai-review
- Vivek Shetye (jul): org CEO/CTO/Engineer/QA/Security (Hermes para gestión, Codex para ingeniería) entregó un MVP de CRM (auth, import CSV, pipeline, métricas). Advierte: "no construye un SaaS de producción desde un prompt"; la calidad depende de AGENTS.md/SOUL.md y de las dependencias entre tareas. https://dev.to/vivek_shetye/i-built-an-ai-company-that-shipped-an-app-from-scratch-using-paperclip-ai-3a5k
- Effloow (abr): 14 agentes en 5 divisiones; un agente construyó y mantiene un sitio Laravel con infra < US$20/mes. Incidente día 1: los agentes **inventaron métricas y testimonios**; la solución fue una regla anti-fabricación en todos los prompts. Ingresos: US$0 al momento de escribir. https://dev.to/jangwook_kim_e31e7291ad98/how-we-built-a-company-powered-by-14-ai-agents-using-paperclip-4bg6
- Moe Lueker (may): "equipo de IA por US$7/mes"; bajó la factura de Opus de US$236 a US$11 moviendo roles a modelos baratos. https://moelueker.com/blog/paperclip-agent-setup-ai-team-for-7-dollars-month
- ToolCenter (2 semanas de prueba): un agente entró en loop con un ticket ambiguo, quemó hasta el 80 % del tope y avisó; "reescribí el ticket". Un secreto mal configurado costó medio día de fallos silenciosos. https://www.toolcenter.ai/en/articles/paperclip-review-2026
- Felipe Fontoura (25 sep, act. 6 oct): 13 agentes sobre Hermes, seis semanas de fallos documentados; un `AGENTS.md` perdido en la carpeta de la app se inyectaba en cada wake; aislar los directorios de trabajo redujo los tokens de entrada un 22 %. https://felipefontoura.com/articles/paperclip-ai-agent-company/
- Preuve AI (4 oct, sobre 2026.1001.0): leyó los 12 advisories; estima US$20–100/mes en modelos según los docs; señala que no hay plan hosteado y que el adapter de Codex trae el bypass en `true`; lo recomienda sólo a devs que ya corren varios agentes. https://preuve.ai/blog/paperclip-ai-review
- Alex Aslam (dev.to, 25 sep): un enjambre plano de 18 agentes se degradó; propone 3 niveles con el CEO delegando a 2–4 managers. https://dev.to/alex_aslam/your-agent-org-chart-is-flat-and-thats-why-it-fails-at-scale-the-3-tier-pattern-that-handles-43nh

**En contra**
- Stork.AI: cuatro fallas: la IA no necesita un CEO, "teatro de productividad", propagación de errores tipo teléfono roto, "montarse en un cohete v0.3". Conclusión: "una herramienta potente envuelta en un mito", útil para delegar, no para crear. https://www.stork.ai/blog/ai-wont-run-your-company-heres-why
- Aron Prins: "gasté US$130 y nada funcionó" (CSS roto, él terminó siendo el departamento de QA); causa raíz: "souls" de un párrafo vs. tres páginas. Luego se volvió contribuidor. https://www.youtube.com/watch?v=FitHZ3XlpRM
- Issues más comentadas: #373 agentes ociosos consumieron cientos de miles de tokens en una noche (abierta); #390 circuit breaker para loops (abierta desde marzo); #3154 "1 % de Claude Max por minuto"; #7601 un agente imprimió un secreto en texto plano en la UI (abierta); #2301 rant "otro desastre de IA" por fallos de instalación.
- Features más pedidas y no entregadas: LLMs locales vía Ollama (#187, 66 votos, abierta desde marzo) y vía LiteLLM (#14104); onboarding sin Codex/Claude Code (discusión #14385); adapter para Copilot CLI (#2092); chat con agentes (#49, ahora 🟡 en roadmap); OpenRouter (#1068).
- Issues calientes de las últimas 3 semanas: #14093 todos los agentes Claude Code fallan con "ACP agent reported a terminal access failure" (26 sep, 16 comentarios); #14182/#13725/#13805 **las conexiones por suscripción de Anthropic expiran a las ~8 h** porque sólo se guarda el access token (tres duplicados abiertos); #13889 el Claude Code empaquetado no corre modelos nuevos; #13949/#15161 no se puede editar el rol de un agente tras crearlo; #15053 onboarding con suscripción OpenAI falla con gpt-6.1-sol; #14600/#14537/#15483 deadlocks de recuperación que dejan issues congelados; **#15027 agentes sin run token actúan como board en modo `local_trusted`** (3 oct); #15531 el proxy de red del sandbox tumba el servidor (8 oct).

## Hacker News y Reddit, leídos directamente (12 oct)

**Hacker News** (API de Algolia): 11 publicaciones sobre Paperclip entre marzo y septiembre de 2026 y ninguna superó los 6 puntos ni tuvo más de un comentario. Las últimas dos (24 sep, 3 y 4 puntos, 0 comentarios): https://news.ycombinator.com/item?id=49831929 · https://news.ycombinator.com/item?id=49830940. Hay un "Show HN" de un competidor con la misma idea (9 sep): https://news.ycombinator.com/item?id=49633106. En los 717 comentarios de 2026 que mencionan "paperclip", ninguno discute el producto en profundidad. Conclusión: para la audiencia técnica clásica de HN, Paperclip no existe; su comunidad está en YouTube, X, Discord y Reddit.

**Reddit** (vía pullpush.io; existe r/PaperClip_AI): el tema dominante, con diferencia, es el **consumo de tokens**:
- r/openclaw, "Spent more on Paperclip heartbeats than on rent" (abr): "el heartbeat dispara cada pocos segundos y se come tus tokens"; un comentarista vio que el 50 % de su uso era Paperclip. Otro responde que el default es 30 min y que corre la plantilla *Agency Agents* (160+ agentes) con todos los heartbeats apagados salvo CEO y liderazgo, usando ~40 % de su suscripción ChatGPT Pro en 4 días. https://www.reddit.com/r/openclaw/comments/1seolaj/
- r/PaperClip_AI, "How do I minimize token use?" (abr): 2–5 millones de tokens por issue con 4 agentes; "dos semanas instalando y aún no está bien"; un usuario que revisó el código: "arquitectónicamente está lejos de ser eficiente… la UX es de primera, por dentro es cinta adhesiva". https://www.reddit.com/r/PaperClip_AI/comments/1st5wid/
- r/openclaw, "Has anyone used paperclip to connect their openclaws" (jul): "extremadamente ineficiente en tokens", "quemó tokens y no estaba seguro de que hiciera algo", "las actualizaciones rompían cosas"; a favor, un usuario que coordina 8 repos con 2 agentes como lead y revisor y él como board, tick cada 15 min y 1 run concurrente. https://www.reddit.com/r/openclaw/comments/1v57r2n/
- r/AI_Agents, "From $5.86 to $1.21 a day" (6 oct): diario de costos de 6 agentes en Claude; pasar de timers a **wake on demand** bajó el gasto diario un 79 % en dos pasos (−57 % con menos despertares, −52 % más al eliminar los timers), porque cada despertar recarga todo el contexto aunque no haya trabajo. https://www.reddit.com/r/AI_Agents/comments/1wz0jqb/
- r/PaperClip_AI, "how do you prevent agents from getting stuck in endless reviews" (28 sep): loops de revisión entre agentes; respuestas: el tope de rondas por defecto y una rutina que los "desatasca". https://www.reddit.com/r/PaperClip_AI/comments/1ws7f1s/
- r/ClaudeCode, "Work = Managing a bunch of agents" (may): un usuario con bloqueos de aprobación diseñados en cada agente; otro: "es una caja negra, no queda claro cuándo un issue necesita a un humano". https://www.reddit.com/r/ClaudeCode/comments/1tdp8xh/
- r/indiehackers, "Running a complete AI agent team. Is it real or not?" (abr, 64 comentarios): consenso: funciona para flujos repetibles y acotados, el costo con Opus se dispara con 3+ agentes y reintentos, "la economía sólo cierra si eres honesto sobre qué reemplazan". https://www.reddit.com/r/indiehackers/comments/1sk46gr/
- Otros: r/OpenaiCodex "quema 7× menos tokens que Paperclip" (competidor WUPHF, abr); r/UnaAI "glitchy, temprano, la estrategia venía llena de estadísticas alucinadas" (may); r/hermesagent compara Paperclip con el kanban nativo de Hermes (sep); r/developersPak "lo probé hoy y me gustó, ¿alguien lo usa?" (25 sep); varios posts de *Papercliped*, un MCP comunitario para operar Paperclip desde Claude/ChatGPT (9–10 oct), una semana antes de que el 1012.0 trajera lo mismo de forma oficial.

Lectura: Reddit confirma con datos lo que los issues ya decían: el producto castiga a quien deja timers encendidos y premia a quien lo usa event-driven con pocos agentes. Ninguna de las quejas de tokens viene de alguien con `maxConcurrentRuns: 1`, ICs on-demand y presupuestos duros.

## Datos de uso real

- Estudio oficial (26 mar): 14,267 commits públicos atribuidos a Paperclip en 884 repos y 562 owners. Mediana de 3 commits por repo; el top 100 concentra el 70 %. Perfil: founders técnicos y equipos muy pequeños, TypeScript/Next.js. Nadie ha publicado un P&L mensual de un equipo de desarrollo real.
- Costos reportados: de US$11 a US$236/mes en suscripciones; Claude Max US$200 se considera "prácticamente obligatorio" (issue #498); Preuve estima US$20–100/mes en modelos; un post-mortem comunitario registró **miles de millones de tokens de escritura de caché en un mes** por agentes ociosos que revisaban su bandeja cada 30 min en Azure AI Foundry. La regla comunitaria ya no es "heartbeats cada 4–8 h" sino **wake por asignación y sin timers en ICs**.
- Sin novedades: no apareció ningún caso de estudio de equipo de software con PRs/día o costo por feature entre el 20 de septiembre y el 12 de octubre.

## Tendencias del ecosistema que enmarcan a Paperclip (sept. 2026)

1. **"Control plane" como categoría**: Paperclip (101k ★), Multica (52.6k ★, v0.6.1 con protección contra runaway y cierre automático de issues al fusionar el PR), Zeron (nuevo, 3.2k ★, control plane nativo en Rust para Claude Code/Codex/Cursor/Devin), Agent Orchestrator, Gas Town, 5dive, LangSmith Fleet, Tasklet. Paperclip publicó su "guía del comprador" (18 sep) y una comparación de stacks self-hosted (7 oct). Los vendors mueven el "coordinador" a su runtime: Cursor Projects (coordinador que delega a subagentes cloud), Claude Managed Agents con workflows dinámicos (beta 9 oct, hasta 1,000 agentes por run), ChatGPT "Dots" (agentes siempre activos que delegan a Codex).
2. **Los vendors absorben el caso base**: GitHub Agent HQ (asignar issues a Claude/Codex/Copilot), Linear Agents, Atlassian Rovo Dev (US$20/dev), Cursor Cloud, Devin. Para "asigna un ticket, recibe un PR" ya no hace falta un orquestador propio.
3. **OpenAI Symphony** (abr, Apache-2.0, 27k ★): orquestación Linear → Codex por issue, sin UI ni presupuestos, "engineering preview". Confirma el patrón pero no compite en gobierno.
4. **Backlash al "zero-human" y giro a "router delgado"**: la crítica dominante es que la jerarquía añade tokens y pérdida de contexto ("teléfono roto") sin mejor output. Paperclip respondió reposicionándose a "gestiona agentes en el trabajo", recortando las instrucciones por defecto del CEO a 20 palabras (1012.0) y, con Assistant Connection, permitiendo que el dev delegue desde Claude Code sin pasar por un "CEO". La dirección del mercado es un coordinador delgado que enruta, no un "CEO que piensa".
5. **Seguridad de agentes con shell** es el tema caliente: los CVEs de OpenClaw y Paperclip empujan hacia sandboxes (Daytona, e2b, Modal) y presets de baja confianza.
6. **Colisión de nombres**: existe otro "Paperclip AI" (paperclip.ai, de Ayush Pathak, con un CEO llamado "Zeus", no-code hosteado). Varias reseñas virales hablan de ese producto. El tuyo, paperclipai.net, es el sitio antiguo del proyecto real (paperclipai/paperclip → hoy paperclip.ing).

## Y entonces, ¿qué?

Paperclip ganó la carrera de la categoría en visibilidad (100k estrellas, 3.9M vistas en YouTube) y en funcionalidad de gobierno (presupuestos, aprobaciones, auditoría, y ahora revisión de PRs, Slack bidireccional y MCP para operar desde el terminal), pero la deuda de mantenimiento (797 issues abiertas vs 95 cerradas en 30 días), la concentración en un mantenedor, tres RCEs críticos, tres releases con breaking changes en tres semanas y la falta de financiación divulgada lo mantienen en "producción temprana para entusiastas". Para un equipo de desarrollo la pregunta correcta no es "¿reemplaza a mis devs?" sino "¿me da control de gasto y trazabilidad sobre los agentes que ya uso?". Ahí sí tiene sentido, como piloto acotado y aislado. Ver [09-viabilidad-y-alternativas.md](09-viabilidad-y-alternativas.md).
