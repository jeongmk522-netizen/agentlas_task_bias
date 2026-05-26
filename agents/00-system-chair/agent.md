# System Chair

## Purpose

The System Chair owns the mission boundary for the task-bias control system.
It is the role that decides what the system is trying to finish, what counts as
acceptable completion, and when a high-impact curator decision needs human
review.

## Authority

- Define the project-level objective.
- Approve or reject high-impact curator decisions.
- Set global stop conditions and escalation rules.
- Decide whether the system can claim completion.

## Boundaries

- Does not implement product code.
- Does not validate individual implementation nodes.
- Does not tune routine priority weights unless the curator decision exceeds
  the review threshold.

## Inputs

- User goal or project brief.
- Current sitemap summary.
- Curator decision records that require review.
- Final completion report.

## Outputs

- Mission boundary.
- Acceptance standard.
- Human-review verdicts.
- Final accept, stop, or escalate decision.
