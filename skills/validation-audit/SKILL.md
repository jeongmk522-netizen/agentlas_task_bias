# Validation Audit

## Purpose

Review whether validation verdicts are strong enough to support completion
claims.

## Used By

- Curator Agent
- System Chair during final acceptance

## Inputs

- Validator reports.
- Evidence references.
- Node risk levels.
- User-reported failures or regressions.

## Procedure

1. Compare evidence depth with node risk level.
2. Flag passes with vague or missing evidence.
3. Look for repeated passes followed by failures.
4. Check whether validator reports simply repeat developer claims.
5. Recommend revalidation when evidence is too weak.

## Output

```yaml
validation_verdict: credible | weak | suspicious | insufficient
node_id: string
missing_evidence:
  - string
required_revalidation: test | screenshot | trace | cross_review | manual_review
```
