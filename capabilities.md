# Capability, skill, and artifact awareness

Load only when mapping a team, agent, or skill to a route. This is a generic registry, not an organization chart.

## Capability record

    capability, subtask class, input provenance, artifact/version, required skill/tool,
    data class, allowed locations, side effects, quality floor, owner, reviewer trigger,
    default tier, escalation tier, budget class, measurement pack

Managers map their local team/agent names to these records; Tokenomics never preloads a permanent department list.

## Reusable classes

| Class | Artifact / acceptance |
|---|---|
| Extract | Source packet with version/path/checksum |
| Transform | Reproducible output and mechanical checks |
| Analyze | Evidence, assumptions, limitations, reviewer gate |
| Decide | Decision record and named owner approval |
| Generate | Native editable asset and render/interaction check |
| Publish/act | Explicit authority, dry run where possible, action record |

## Department adaptations

- Engineering/DevOps/QA: build, targeted tests, staging/dry run, rollback proof.
- Security/Privacy/Trust: target scope, permitted validation, evidence handling, policy traceability.
- Research/Market/Pricing: claim-source map, units/population/method/date; market evidence is not vendor marketing.
- Data/Analytics: lineage, population, metric, missingness, split strategy, uncertainty.
- Product/Design/Operations: editable artifact, audience/accessibility, render/interaction check, owner approval.
- Finance/Legal/People/Compliance: currency/period/tolerance; jurisdiction/effective date; permitted attributes/purpose; control/evidence period. Analysis cannot pay, sign, file, certify, or make personnel decisions.

## Short-form social

    strategy → story/script → storyboard → asset generation → assembly/captions
    → brand/rights review → authorized publish

Each stage is an independent capability route with its own artifact, budget, and authority.
