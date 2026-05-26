# Curator Agent Architecture

This document turns the paper's proposal into a practical system architecture.
It is written for teams building multi-agent coding systems that need broader
project coverage, stronger validation evidence, and fewer premature completion
claims.

## Purpose

The Curator Agent is a second-order control layer. It does not write product
code, run the primary task queue, or certify implementation work as complete.
It watches how work is allocated across a project and adjusts the rules that
shape future work.

The goal is to reduce task bias: the tendency for coding agents to keep working
on surfaces that are recent, salient, well-instrumented, or easy to measure
while other project surfaces remain uninspected.

## Repository Architecture

This repository uses a hierarchy-first layout. Agent roles live under
`agents/`, and reusable operating skills live under `skills/`.

```text
agents/
  00-system-chair/
    agent.md
  10-curator-agent/
    agent.md
  20-orchestrator-agent/
    agent.md
  30-developer-agent/
    agent.md
  40-validator-agent/
    agent.md

skills/
  task-bias-audit/
    SKILL.md
  sitemap-governance/
    SKILL.md
  validation-audit/
    SKILL.md
```

The numbers are intentional. They make authority visible before any runtime or
prompting style is chosen. Lower numbers set mission and policy. Higher numbers
execute or verify bounded work.

## System Roles

```text
System Chair
  -> Curator Agent
      -> Orchestrator
          -> Developer Agent
          -> Validator Agent
              -> AI Sitemap update

Curator Agent observes the full loop and updates the meta-layer.
```

### System Chair

The System Chair owns the mission boundary and the final acceptance standard.
It exists to keep the curator from becoming an unbounded manager. The chair
does not tune routine policy weights; it reviews high-impact curator decisions
and decides whether the system should continue, stop, or escalate to a human.

### AI Sitemap

The sitemap is the shared external state of the project. It contains routes,
modules, APIs, jobs, user flows, dependencies, known issues, completion scores,
validation evidence, and risk levels.

### Orchestrator

The orchestrator chooses the next bounded task from the sitemap. It applies a
visible priority policy rather than relying on recent conversational context.

### Developer Agent

The developer agent executes the assigned task. It should not decide the next
global task and should not update completion scores for its own work.

### Validator Agent

The validator checks the developer's output and writes evidence back to the
sitemap. Validation should include limitations, not only a pass/fail verdict.

### Curator Agent

The curator reviews the system's behavior over time. It updates policy weights,
flags weak validation patterns, proposes schema extensions, and manages
provisional nodes discovered during exploratory work.

## Sitemap Model

Each sitemap node should include:

| Field | Purpose |
|---|---|
| `node_id` | Stable identifier for a route, module, API, job, or user flow. |
| `kind` | Node type. |
| `status` | Unknown, todo, in progress, blocked, validated, or revalidate. |
| `completion_score` | Evidence-backed completion score from 0.0 to 1.0. |
| `risk_level` | Low, medium, high, or regulated. |
| `last_modified` | Most recent implementation change. |
| `last_tested` | Most recent validation event. |
| `dependencies` | Related nodes that block or depend on this node. |
| `acceptance_checks` | Node-specific criteria for completion. |
| `evidence` | Tests, screenshots, traces, reviews, or commits. |
| `provisional` | Whether the node is still being defined. |

## Priority Policy

A simple orchestrator policy can start with:

```text
priority =
  under_coverage_weight * (1 - completion_score)
+ staleness_weight * age(last_tested)
+ risk_weight * risk_level
+ dependency_weight * blocked_dependents
- recent_focus_penalty * recent_visits
```

The exact weights should be visible and reviewable. The curator may tune them,
but changes should be small, logged, and reversible.

## Curator Responsibilities

### 1. Tune Work Allocation

The curator watches recent task history. If too many cycles target the same
small set of nodes, it can increase exploration pressure or raise the penalty
for recent focus. If the system is spreading itself too thin, it can increase
dependency or completion-gap weight.

### 2. Audit Validation Patterns

The curator checks whether validation results are credible. Warning signs
include very high pass rates, low evidence density, repeated user-reported
failures after validator passes, or validator reports that simply mirror the
developer's own summary.

### 3. Evolve The Schema

Some domains need fields the base sitemap does not contain. Billing flows may
need entitlement states. Data pipelines may need freshness and lineage. Mobile
apps may need device, permission, offline, and lifecycle states. The curator can
propose such fields, but high-impact schema changes should require human
review.

### 4. Manage Provisional Nodes

Exploratory projects discover surfaces as they go. The curator can create
provisional nodes for possible routes, flows, or dependencies, then later
promote, merge, split, or discard them when enough evidence exists.

## Curator Decision Record

Every curator action should produce a compact record:

```yaml
decision_id: cur_001
decision_type: tune_policy
risk_level: medium
evidence:
  - "Recent 20 cycles touched only 3 unique nodes."
old_state:
  recent_focus_penalty: 0.10
new_state:
  recent_focus_penalty: 0.20
human_review_required: false
reason: "Work allocation is concentrated and several high-risk nodes are stale."
expires_at: "next curator checkpoint"
```

The record should be understandable without reading the entire run transcript.

## Invocation Model

The curator should run at checkpoints, not after every tool call.

Recommended periodic triggers:

- Every fixed number of completed validation cycles.
- Before a major completion claim.
- At the end of a long autonomous work session.

Recommended event triggers:

- The same node is edited repeatedly.
- Recent work covers too few unique nodes.
- A high-risk node remains stale.
- A validator pass has weak evidence.
- A developer discovers an undefined dependency.
- The user adds a new requirement that changes the project surface.

## Human Review

Human review should focus on meta-decisions rather than every implementation
detail. Review is recommended when the curator:

- Adds or removes schema fields.
- Suppresses a validation mode.
- Reclassifies many nodes at once.
- Promotes many provisional nodes at once.
- Changes policy weights beyond the configured small-change threshold.

## Evaluation Metrics

The architecture should be evaluated by measuring coverage and allocation:

| Metric | Question |
|---|---|
| Node coverage | How many known nodes were inspected and validated? |
| Work-allocation inequality | Did effort concentrate in a small subset? |
| High-risk stale count | Which risky nodes remained untested? |
| False completion rate | Did completion claims omit unvisited nodes? |
| Validation reversal rate | How often did curator or human review overturn validator passes? |
| Provisional-node quality | Were discovered nodes promoted, merged, or discarded cleanly? |

The architecture succeeds if it makes unvisited work visible, reduces silent
staleness, improves evidence quality, and keeps human review focused on a small
number of meaningful meta-decisions.
