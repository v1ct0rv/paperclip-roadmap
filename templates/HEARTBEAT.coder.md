# HEARTBEAT.md — checklist por heartbeat (coder)

1. Identidad y contexto: lee `PAPERCLIP_AGENT_ID`, `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`, `PAPERCLIP_WORKSPACE_CWD`, `PAPERCLIP_WORKSPACE_BRANCH`.
2. `GET /api/agents/me` → confirma presupuesto restante; si estás por encima del 90 %, termina sólo con un comentario de estado.
3. Si `PAPERCLIP_WAKE_REASON=issue_commented|issue_comment_mentioned`: lee el comentario primero; si es feedback de review, responde punto por punto.
4. `GET /api/companies/{companyId}/issues?assigneeAgentId=me&status=todo,in_progress,in_review,blocked` → toma el de mayor prioridad. Máximo 1 issue por heartbeat.
5. `POST /api/issues/{id}/checkout` con `X-Paperclip-Run-Id`. Si responde 409, otro run lo tiene: no reintentes, sal.
6. Lee `GET /api/issues/{id}/heartbeat-context`: criterios de aceptación, comentarios, PR existente.
7. Trabaja en el worktree: implementa → tests → lint/typecheck.
8. Commit → push → PR (`gh pr create --fill --base main`) o actualiza el PR existente.
9. `PATCH /api/issues/{id}` con el estado (`in_review` si CI verde, `blocked` si falta info) y un comentario obligatorio: link del PR, resumen, comandos ejecutados, riesgos.
10. Sal limpiamente. No dejes procesos en background ni cambios sin commit.
