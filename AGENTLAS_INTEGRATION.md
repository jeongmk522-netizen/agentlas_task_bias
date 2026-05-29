# Agentlas Runtime Integration

This repo (`agentlas_task_bias`) is one of three architectures **baked into the Agentlas
desktop app and `agentlas` terminal CLI** as a built-in agent.

- **Built-in agent:** `agentlas-task-bias` (role: `governance`)
- **What ships:** an operational distillation of the Curator Agent + AI Sitemap model
  (`ARCHITECTURE.md`, `agents/10-curator-agent/agent.md`, `skills/`). Second-order control:
  it adjusts work-allocation rules and validation evidence; it does **not** mark nodes
  complete or do implementation work.
- **External state:** the AI Sitemap lives per project folder at
  `<folder>/.agentlas/sitemap.json` (nodes: status, completion_score, risk, evidence,
  provisional, dependencies — per `ARCHITECTURE.md`).
- **Where it lives in the runtime:** `electron/architecture/manifest.ts` in
  `agentlas_desktop`.

## Auto-activation

When a user works **repeatedly in a project folder**, the runtime activates the folder and
creates the AI Sitemap alongside PM Soul memory — so bias-audit + sitemap governance kick
in automatically, not just on request.

## Upgrade contract

Change the sitemap schema, priority policy, or curator boundaries → update
`agentlas_desktop/electron/architecture/manifest.ts` (Task Bias prompt) and, for new
sitemap fields, the `.agentlas/sitemap.json` skeleton in `electron/memory/project-files.ts`.
Bump `ARCHITECTURE_VERSION` and rebuild.

Full details: `agentlas_desktop/docs/ARCHITECTURE_PLAYBOOK.md`.
