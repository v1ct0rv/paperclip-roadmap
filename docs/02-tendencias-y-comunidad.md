# 02 · Tendencias, tracción y lo que dice la comunidad

> Escaneo del 28 de septiembre de 2026 sobre X, YouTube, GitHub, Hacker News, blogs y podcasts. Reddit no fue accesible directamente; lo que se cita de Reddit es de segunda mano.

## Señales principales

### 🔥 1. La tracción es real y sigue creciendo, pero vive en X, YouTube y Discord, no en Hacker News
92,650 estrellas y 15,873 forks en menos de 7 meses (repo creado el 2 de marzo de 2026), ~120k descargas mensuales del CLI en npm, 8,005 miembros en Discord, commits diarios y releases cada 1–2 semanas. Trayectoria: 26k estrellas a mediados de marzo → 50k el 15 de abril → 70k en junio → 92.6k hoy. En cambio, ninguna de las 7 publicaciones en Hacker News pasó de 6 puntos: la comunidad "builder" clásica lo ha ignorado o lo mira con escepticismo ("el costo creciente de que la IA cuide a la IA").
Fuentes: https://github.com/paperclipai/paperclip · https://www.star-history.com/paperclipai/paperclip · https://news.ycombinator.com/item?id=47277096

### 🔥 2. Seguridad: el mayor golpe reputacional del año
12 advisories en 2026, incluidos **CVE-2026-41679** (RCE sin autenticación, CVSS 10.0, módulo de Metasploit publicado por Rapid7) y **CVE-2026-77087** (RCE drive-by por DNS rebinding contra instalaciones locales por defecto, CVSS 9.6, 22 de julio). Cubierto por The Hacker News, Infosecurity Magazine, Oasis Security y SentinelOne. Corregidos desde v2026.416.0. La crítica estructural de Oasis: "la configuración del agente es código", y el producto ejecuta CLIs sin sandbox en el host.
Fuentes: https://github.com/paperclipai/paperclip/security/advisories · https://thehackernews.com/2026/08/paperclip-ai-flaws-let-attackers-run.html · https://www.oasis.security/blog/paperclip-agent-vulnerabilities

### 🔥 3. El backlog de mantenimiento crece más rápido que el equipo
2,542 issues abiertas vs 913 cerradas; 3,444 PRs abiertos; en los últimos 30 días se abrieron 466 issues y se cerraron 111. Un solo mantenedor (el fundador seudónimo "dotta", GitHub `cryppadotta`) tiene 2,865 commits; el siguiente, 566. El propio fundador habló en el Open Source Startup Podcast (3 ago) de "la realidad de mantener una tonelada de PRs de la comunidad".
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
| 25–27 sep | Tutoriales recientes (Full Stack, Ferdy Korpershoek) | 10–13k c/u | 👀 |

Enlaces: https://www.youtube.com/watch?v=C3-4llQYT8o · https://www.youtube.com/watch?v=RHZLJhp-JMM · https://www.youtube.com/watch?v=QufcrM79snw · https://www.youtube.com/watch?v=h403btjldDQ · https://x.com/dotta/status/2029239759428780116

## Lo que dicen quienes lo usaron de verdad

**A favor**
- eesel (jul): "uno de los proyectos open source más impresionantes del año… un control plane, no un agente"; elogia gobierno, agnosticismo de agentes y self-hosting; critica la carga DIY de infraestructura. https://www.eesel.ai/blog/paperclip-ai-review
- Vivek Shetye (jul): org CEO/CTO/Engineer/QA/Security (Hermes para gestión, Codex para ingeniería) entregó un MVP de CRM (auth, import CSV, pipeline, métricas). Advierte: "no construye un SaaS de producción desde un prompt"; la calidad depende de AGENTS.md/SOUL.md y de las dependencias entre tareas. https://dev.to/vivek_shetye/i-built-an-ai-company-that-shipped-an-app-from-scratch-using-paperclip-ai-3a5k
- Effloow (abr): 14 agentes en 5 divisiones; un agente construyó y mantiene un sitio Laravel con infra < US$20/mes. Incidente día 1: los agentes **inventaron métricas y testimonios**; la solución fue una regla anti-fabricación en todos los prompts. Ingresos: US$0 al momento de escribir. https://dev.to/jangwook_kim_e31e7291ad98/how-we-built-a-company-powered-by-14-ai-agents-using-paperclip-4bg6
- Moe Lueker (may): "equipo de IA por US$7/mes"; bajó la factura de Opus de US$236 a US$11 moviendo roles a modelos baratos. https://moelueker.com/blog/paperclip-agent-setup-ai-team-for-7-dollars-month
- ToolCenter (2 semanas de prueba): un agente entró en loop con un ticket ambiguo, quemó hasta el 80 % del tope y avisó; "reescribí el ticket". Un secreto mal configurado costó medio día de fallos silenciosos. https://www.toolcenter.ai/en/articles/paperclip-review-2026

**En contra**
- Stork.AI: cuatro fallas: la IA no necesita un CEO, "teatro de productividad", propagación de errores tipo teléfono roto, "montarse en un cohete v0.3". Conclusión: "una herramienta potente envuelta en un mito", útil para delegar, no para crear. https://www.stork.ai/blog/ai-wont-run-your-company-heres-why
- Aron Prins: "gasté US$130 y nada funcionó" (CSS roto, él terminó siendo el departamento de QA); causa raíz: "souls" de un párrafo vs. tres páginas. Luego se volvió contribuidor. https://www.youtube.com/watch?v=FitHZ3XlpRM
- Issues más comentadas: #373 agentes ociosos consumieron cientos de miles de tokens en una noche (abierta); #390 circuit breaker para loops (abierta desde marzo); #3154 "1 % de Claude Max por minuto"; #7601 un agente imprimió un secreto en texto plano en la UI (abierta); #2301 rant "otro desastre de IA" por fallos de instalación.
- Features más pedidas y no entregadas: LLMs locales vía Ollama (#187, 66 votos, abierta desde marzo), adapter para Copilot CLI (#2092), chat con agentes (#49), OpenRouter (#1068).

## Datos de uso real

- Estudio oficial (26 mar): 14,267 commits públicos atribuidos a Paperclip en 884 repos y 562 owners. Mediana de 3 commits por repo; el top 100 concentra el 70 %. Perfil: founders técnicos y equipos muy pequeños, TypeScript/Next.js. Nadie ha publicado un P&L mensual de un equipo de desarrollo real.
- Costos reportados: de US$11 a US$236/mes en suscripciones; Claude Max US$200 se considera "prácticamente obligatorio" (issue #498). Regla comunitaria: heartbeats de 4–8 h y modelos Sonnet/Haiku hasta demostrar valor.

## Tendencias del ecosistema que enmarcan a Paperclip (sept. 2026)

1. **"Control plane" como categoría**: Paperclip, Multica (51k ★), Agent Orchestrator, Gas Town (Steve Yegge), LangSmith Fleet, Tasklet: todos compiten por ser "la capa de gestión" sobre Claude Code/Codex. Paperclip publicó su propia "guía del comprador" el 18 de septiembre.
2. **Los vendors absorben el caso base**: GitHub Agent HQ (asignar issues a Claude/Codex/Copilot), Linear Agents, Atlassian Rovo Dev (US$20/dev), Cursor Cloud, Devin. Para "asigna un ticket, recibe un PR" ya no hace falta un orquestador propio.
3. **OpenAI Symphony** (abr, Apache-2.0, 27k ★): orquestación Linear → Codex por issue, sin UI ni presupuestos, "engineering preview". Confirma el patrón pero no compite en gobierno.
4. **Backlash al "zero-human"**: la crítica dominante es que la jerarquía añade tokens y pérdida de contexto ("teléfono roto") sin mejor output. Paperclip respondió reposicionándose a "gestiona agentes en el trabajo" y a "humanos en la junta".
5. **Seguridad de agentes con shell** es el tema caliente: los CVEs de OpenClaw y Paperclip empujan hacia sandboxes (Daytona, e2b, Modal) y presets de baja confianza.
6. **Colisión de nombres**: existe otro "Paperclip AI" (paperclip.ai, de Ayush Pathak, con un CEO llamado "Zeus", no-code hosteado). Varias reseñas virales hablan de ese producto. El tuyo, paperclipai.net, es el sitio antiguo del proyecto real (paperclipai/paperclip → hoy paperclip.ing).

## Y entonces, ¿qué?

Paperclip ganó la carrera de la categoría en visibilidad y en funcionalidad de gobierno (presupuestos, aprobaciones, auditoría), pero la deuda de mantenimiento, la concentración en un mantenedor, tres RCEs críticos y la falta de financiación divulgada lo mantienen en "producción temprana para entusiastas". Para un equipo de desarrollo la pregunta correcta no es "¿reemplaza a mis devs?" sino "¿me da control de gasto y trazabilidad sobre los agentes que ya uso?". Ahí sí tiene sentido, como piloto acotado y aislado. Ver [09-viabilidad-y-alternativas.md](09-viabilidad-y-alternativas.md).
