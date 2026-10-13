# 04 · Tres escenarios de adopción

Los tres comparten las fases 0–2 del [roadmap](03-roadmap-0-a-100.md). Difieren en organigrama, presupuesto, gobierno y riesgo.

| | A · Empresa de software (5–20 devs) | B · Solo founder / 1–3 personas | C · Agencia multi-cliente |
|---|---|---|---|
| Objetivo | Aumentar throughput del equipo humano sin bajar calidad | Operar una "empresa" casi autónoma con supervisión mínima | Escalar entregas paralelas por cliente con aislamiento |
| Board (humanos) | CTO/Tech Lead + 1 senior por squad | Tú | PM/Delivery lead + tech lead por cuenta |
| Nº de agentes (arranque → régimen) | 3 → 6–8 | 2 → 4–5 | 3 por cliente → 4–6 por cliente |
| Presupuesto mensual sugerido | US$300–800 + suscripciones | US$100–400 (suscripción Max cubre mucho) | US$200–500 por cliente, facturable |
| Modo de despliegue | VPS compartido, `authenticated/private` (Tailscale) o `public` | VPS pequeño o Mac siempre encendida | VPS + **una organización por cliente**; sandboxes para datos sensibles |
| Gobierno | Review QA agent + approval humano en TODO | Review QA agent; approval humano sólo en PRs L o infra | Review + approval humano; secrets y repos por org; export/import de plantilla |
| Riesgo principal | Ruido y PRs mediocres que consumen tiempo senior | Runaway spend y decisiones de "CEO" sin criterio de negocio | Fuga de contexto entre clientes; costos no atribuidos |
| Métrica de éxito (3 meses) | ≥ 30 % de issues S/M cerrados por agentes; tiempo de revisión < 15 min/PR | 1 producto pequeño con releases semanales; < US$400/mes | Costo por PR fusionado por cliente; margen por cuenta |

---

## A · Empresa de software (5–20 devs)

**Organigrama (régimen)**

```
Board: Tech Lead humano
└── CTO agent (Claude, Opus) — planifica, tría, revisa
    ├── Backend coder (Codex)          on-demand
    ├── Frontend coder (Claude Sonnet) on-demand
    ├── QA agent (Claude Sonnet)       review stage
    ├── Reviewer senior (Claude Opus)  segunda opinión en PRs M/L
    └── Doc & Release (Codex mini)     rutina semanal
```

Sin CEO agent: el "CEO" es el negocio real. El CTO agent recibe metas de proyecto que escribe el Tech Lead.

**Cómo encaja con el equipo humano**
- Los devs siguen trabajando igual; los agentes toman los issues etiquetados `agent-ready` en Jira/GitHub (bugs claros, tests, chores, endpoints simples).
- Cada dev senior revisa los PRs de agentes de su área; el tiempo de revisión es la métrica que más hay que cuidar.
- Retro quincenal: qué issues fueron buenos para agentes, cuáles no. Ajusta la etiqueta `agent-ready` con esa evidencia.

**Casos de uso concretos**
1. Deuda técnica planificada: migrar 40 componentes a la nueva librería de UI, un PR por componente.
2. Cobertura de tests: subir cobertura de un módulo del 40 % al 80 % con PRs de ≤ 300 líneas.
3. Bug triage nocturno: rutina de 22:00 que reproduce bugs reportados en Sentry, escribe el test que falla y abre PR con fix propuesto.
4. Mantenimiento de docs: el agente de docs actualiza README/ADRs después de cada merge a `main` (webhook `pull_request.closed`).
5. Dependabot con criterio: los PRs de dependencias los evalúa un agente que corre los tests y resume el changelog en el PR.

## B · Solo founder / 1–3 personas

**Organigrama**

```
Board: tú
└── CEO agent (Claude Opus, 1 heartbeat/día 07:00)
    ├── Coder full-stack (Codex, suscripción ChatGPT)     on-demand
    ├── QA (Claude Sonnet)                                  review stage
    └── Growth/Content (Claude Sonnet, opcional)            rutina 3×/semana
```

- Aquí sí tiene sentido el CEO agent: tú das la misión y métrica; él propone estrategia, planifica sprints y te reporta. **Tú apruebas estrategia y contrataciones**.
- Corre coders con suscripción (Max/Pro) para controlar el costo; API keys con budget duro para CEO/QA.
- El día típico: mañana lees el brief del CEO en Slack, apruebas 2–3 decisiones, revisas 3–5 PRs; el resto es autónomo.
- Guardarraíl imprescindible: `hardStopEnabled` en company y agentes; `maxReviewRounds: 3`; heartbeat de CEO **una vez al día**, no por hora (la mayor causa de gasto inútil en single-founder es un CEO hiperactivo creando tareas).

**Casos de uso**: MVP de SaaS pequeño (landing + auth + Stripe + 1 feature), micro-productos internos, automatizaciones de marketing con Growth agent, mantenimiento de un producto existente mientras tú vendes.

## C · Agencia multi-cliente / multi-repo

**Estructura**

```
Instancia Paperclip (VPS)
├── Org "Cliente A"  → CTO-A + Coder-A + QA-A   (secrets A, PAT repo A, budget A)
├── Org "Cliente B"  → CTO-B + Coder-B + QA-B
└── Org "Interna"    → agentes de la agencia (propuestas, docs, tooling)
```

- **Una organización por cliente** es la unidad de aislamiento nativa: datos, presupuesto, credenciales, costos atribuidos por proveedor/modelo (para facturar).
- Plantilla: configura la org del primer cliente, expórtala (`paperclipai company export`) y clona para cada cuenta nueva (`import` → `new_company`). Los agentes importados arrancan con heartbeat apagado.
- Datos sensibles / NDA estrictos: sandbox provider (Daytona, e2b, Modal) o un contenedor Paperclip por cliente. Nunca un workspace compartido.
- Jira: cada org apunta al proyecto Jira del cliente; el conector es por cuenta de Atlassian, así que usa una cuenta de servicio por cliente si la política lo permite.
- Facturación: exporta costos mensuales por org (Costs → Finance) y súmalos a la factura como "AI compute" o absórbelos en margen.
- Contrato: revisa con tus clientes qué proveedor de modelo ve su código (Anthropic/OpenAI ZDR, Bedrock/Vertex si exigen región) antes de conectar el repo.

**Casos de uso**: mantenimiento evolutivo de N sitios/apps de clientes con un humano por cuenta, migraciones repetitivas (framework, Node, PHP) entre clientes reutilizando skills, QA de regresión por release, respuesta de primer nivel a bugs reportados en Jira fuera de horario.
