# Sitemap Governance

## Purpose

Keep the AI sitemap useful as a shared external state model without allowing it
to become an unbounded note dump.

## Used By

- Curator Agent
- Orchestrator Agent

## Inputs

- Current sitemap.
- New project discoveries.
- Domain-specific requirements.
- Existing schema fields.

## Procedure

1. Decide whether a discovery belongs to an existing node, a provisional node,
   or a schema extension.
2. Keep uncertain discoveries provisional until acceptance checks are clear.
3. Require a validation method for every new schema field.
4. Require a backfill rule before applying a field to existing nodes.
5. Merge or discard provisional nodes that do not become useful.

## Output

```yaml
sitemap_decision: create_node | promote_node | merge_node | discard_node | extend_schema
target: string
reason: string
review_required: boolean
```
