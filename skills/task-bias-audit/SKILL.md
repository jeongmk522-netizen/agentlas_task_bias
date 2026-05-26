# Task Bias Audit

## Purpose

Detect whether agent effort is concentrating on a small subset of the project
while other nodes remain stale or uninspected.

## Used By

- Curator Agent
- System Chair during final review

## Inputs

- Sitemap node list.
- Recent task history.
- Recent validation history.
- Current priority policy.

## Procedure

1. Count unique nodes touched in the recent work window.
2. Compare repeated edits against stale nodes.
3. Identify high-risk nodes without recent validation.
4. Check whether completion claims include unvisited nodes.
5. Classify the coverage verdict.

## Output

```yaml
coverage_verdict: balanced | concentrated | stale-risk | unknown
signals:
  - node_id: string
    issue: string
recommended_action: tune_policy | revalidate | inspect_stale_node | no_change
```
