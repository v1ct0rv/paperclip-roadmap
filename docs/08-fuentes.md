# 08 · Fuentes consultadas

Investigación realizada el 28 de septiembre de 2026 con: documentación oficial (sitio, GitHub, docs.paperclip.ing), Context7 (`/paperclipai/paperclip-docs`), blog oficial, ecosistema comunitario y búsqueda web de opiniones y alternativas.

## Oficiales
- Repo: https://github.com/paperclipai/paperclip (README, ROADMAP.md, SECURITY.md, CONTRIBUTING.md, doc/INSTALLING.md, releases)
- Sitio: https://paperclip.ing/ · https://paperclip.ing/solutions/engineering/ · https://paperclip.ing/product/bring-your-own-agent/ · https://paperclip.ing/product/extensions/ · https://paperclip.ing/about/
- Sitio antiguo: https://paperclipai.net/
- Docs: https://docs.paperclip.ing/
  - Conceptos: /guides/welcome/key-concepts/ · /guides/org/org-structure/ · /guides/org/agents/ · /guides/org/team-catalog/
  - Getting started: /guides/getting-started/installation/ · /your-first-company/ · /your-first-agent/
  - Despliegue: /reference/deploy/docker/ · /how-to/deploy-to-vps-or-fly/ · /reference/deploy/deployment-modes/ · /reference/deploy/environment-variables/
  - GitHub/Jira/Slack: /how-to/connect-agent-to-github/ · /connectors/jira/ · /connectors/slack/ · /how-to/wire-slack-discord-notifications/
  - Gobierno y costos: /guides/power/execution-policy/ · /guides/projects-workflow/task-watchdogs/ · /how-to/set-monthly-budget/ · /how-to/require-board-approval-before-spend/ · /guides/day-to-day/costs/
  - Heartbeats: /guides/projects-workflow/routines/ · /how-to/debug-stuck-heartbeat/
  - Skills: /how-to/write-a-company-skill/ · /reference/skills/bundled/ · /reference/skills/bundled/software-development/github-pr-workflow/
  - Adapters: /reference/adapters/overview/ · /reference/adapters/claude-code/ · /reference/adapters/codex/
  - Backup: /how-to/back-up-and-restore-a-company/
- npm: https://registry.npmjs.org/paperclipai
- Blog: https://paperclip.ing/blog/ai-agent-governance-platform-buyers-guide/ · https://paperclip.ing/blog/who-is-using-paperclip-on-github/ · https://paperclip.ing/blog/manage-multiple-claude-code-codex-sessions/ · https://paperclip.ing/blog/when-your-agent-team-outruns-your-attention/ · https://paperclip.ing/blog/what-is-an-ai-agent-control-plane/

## Context7
- `/paperclipai/paperclip-docs` (4,823 snippets) — consultas: despliegue VPS/Docker, GitHub/Jira/Slack, organigrama de ingeniería.
- También disponibles: `/paperclipai/paperclip`, `/websites/paperclip_ing`, `/paperclipai/docs`.

## Comunidad
- https://github.com/gsxdsm/awesome-paperclip
- Plugins: GitHub Issues sync (mvanhorn/paperclip-plugin-github-issues), Linear (Oldharlem/paperclip-linear-plugin), Slack (mvanhorn/paperclip-plugin-slack), memoria Hindsight (vectorize-io/hindsight), paperclip-mcp (wizarck/paperclip-mcp)
- Libro "Headcount Zero": https://github.com/AnthonyDavidAdams/zero-employee-company-book

## Comunidad, opiniones y casos (usados en 02, 05, 06 y 09)
- Star history: https://www.star-history.com/paperclipai/paperclip · Contribuidores: https://github.com/paperclipai/paperclip/graphs/contributors · Advisories: https://github.com/paperclipai/paperclip/security/advisories
- Seguridad: https://thehackernews.com/2026/08/paperclip-ai-flaws-let-attackers-run.html · https://www.oasis.security/blog/paperclip-agent-vulnerabilities
- Hacker News: https://news.ycombinator.com/item?id=47277096 · https://news.ycombinator.com/item?id=48615312 · https://news.ycombinator.com/item?id=47795259
- X: https://x.com/dotta/status/2029239759428780116 · https://x.com/NickSpisak_/status/2033518072724705437 · https://x.com/NickSpisak_/article/2034635430700679445
- YouTube: Greg Isenberg https://www.youtube.com/watch?v=C3-4llQYT8o · Tech With Tim https://www.youtube.com/watch?v=RHZLJhp-JMM · Nick Saraev (crítica) https://www.youtube.com/watch?v=QufcrM79snw · Nick Puru https://www.youtube.com/watch?v=Yp5KiHBSpZo · Aron Prins https://www.youtube.com/watch?v=FitHZ3XlpRM · dotta en AI Engineer https://www.youtube.com/watch?v=h403btjldDQ · Ferdy Korpershoek https://www.youtube.com/watch?v=Bj4mNOYjOek · En español: UDIA https://www.youtube.com/watch?v=WQ83xC77i8M · jonyl https://www.youtube.com/watch?v=Mn6ixpNl2Fg
- Podcast: https://ossstartuppodcast.substack.com/p/episode-201-building-your-agent-army
- Reseñas: https://www.eesel.ai/blog/paperclip-ai-review · https://www.stork.ai/blog/ai-wont-run-your-company-heres-why · https://www.toolcenter.ai/en/articles/paperclip-review-2026 · https://vibecoding.app/blog/paperclip-review · https://rywalker.com/research/paperclip · https://app.dealroom.co/news/note/paperclip-the-open-source-framework-turning-ai-agents-into-companies
- Casos: https://dev.to/vivek_shetye/i-built-an-ai-company-that-shipped-an-app-from-scratch-using-paperclip-ai-3a5k · https://dev.to/jangwook_kim_e31e7291ad98/how-we-built-a-company-powered-by-14-ai-agents-using-paperclip-4bg6 · https://moelueker.com/blog/paperclip-agent-setup-ai-team-for-7-dollars-month
- Tutoriales técnicos: https://dev.to/truongpx396/paperclip-deep-dive-a-build-guide-for-an-ai-company-control-plane-dda · https://apidog.com/blog/paperclip-ai-agent-company/ · https://contabo.com/blog/how-to-self-host-paperclip-on-contabo-vps/ · https://www.deployhq.com/blog/self-host-paperclip-vps-docker-deployhq · Helm chart: https://gist.github.com/sionsmith/ee02d4091f8059aec8325c12611a6a4c
- Issues clave: #373 tokens ociosos · #390 circuit breaker · #3154 cuota Max · #3335 cwd compartido · #4027 sin tope · #6955 cola de runs · #7641 OOM · #7601 secreto en UI · #11276 hard stop entre runs · #13616 worktrees · #187 Ollama (https://github.com/paperclipai/paperclip/issues/<n>)
- Plantillas oficiales de empresas: https://github.com/paperclipai/companies (gstack, superpowers, clawteam-engineering, agentsys-engineering) · Spec: https://github.com/paperclipai/paperclip/blob/master/docs/companies/companies-spec.md
- Docs de ingeniería en el repo: https://github.com/paperclipai/paperclip/blob/master/doc/LOW-TRUST-PRESETS.md · doc/TASK-WATCHDOG.md · doc/experimental/issue-worktree-support.md · docs/deploy/secrets.md · docs/deploy/aws-ecs.md
- Plugins: https://github.com/mvanhorn/paperclip-plugin-slack · https://github.com/yesterday-ai/paperclip-plugin-company-wizard
- Prácticas del sector: https://asdlc.io/practices/agents-md-spec/ · https://www.re-entry.ai/blog/pull-request-size-policy-ai-generated-code · https://dev.to/prokopsimek/ai-agents-open-pull-requests-faster-than-we-review-them-a-setup-that-keeps-the-queue-moving-3427

## Alternativas (usadas en 09)
- https://github.com/multica-ai/multica · https://github.com/openai/symphony · https://github.com/Untrivial-ai/agent-orchestrator · https://github.com/gastownhall/gastown · https://github.com/openclaw/openclaw · https://github.com/crshdn/mission-control
- https://github.blog/changelog/2026-02-04-claude-and-codex-are-now-available-in-public-preview-on-github/ · https://linear.app/integrations/agents · https://www.atlassian.com/software/rovo-dev/pricing · https://devin.ai/pricing · https://factory.ai · https://cursor.com/pricing · https://claude.com/product/claude-code · https://platform.claude.com/docs/en/managed-agents/overview · https://developers.openai.com/codex/pricing
- https://conductor.build · https://superset.sh · https://github.com/BloopAI/vibe-kanban · https://github.com/OpenHands/OpenHands · https://github.com/crewAIInc/crewAI · https://github.com/microsoft/agent-framework · https://github.com/langchain-ai/langgraph
- Guía del comprador (escrita por Paperclip): https://paperclip.ing/blog/ai-agent-governance-platform-buyers-guide/
