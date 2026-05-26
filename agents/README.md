# Agent Hierarchy

The architecture uses a visible hierarchy by default. This is not decoration.
It is part of the control model.

Flat agent networks make it easy for work to bounce sideways: one agent asks
another agent for more analysis, a reviewer reopens work without a budget, or a
developer chooses the next task because it is already in context. This repo
uses numbered folders so that authority, escalation, and stop conditions are
visible before implementation begins.

## Roles

| Folder | Role | Authority |
|---|---|---|
| `00-system-chair/` | System Chair | Owns mission boundary, final acceptance, and high-impact review. |
| `10-curator-agent/` | Curator Agent | Adjusts meta-rules that govern work allocation and evidence quality. |
| `20-orchestrator-agent/` | Orchestrator Agent | Selects bounded work from the sitemap. |
| `30-developer-agent/` | Developer Agent | Executes assigned implementation work. |
| `40-validator-agent/` | Validator Agent | Checks output and writes evidence-backed verdicts. |

## Rule

Workers report upward. Lateral handoffs require explicit approval from the
nearest responsible owner. No role should both perform a task and certify its
own completion.
