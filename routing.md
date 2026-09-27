# Routing

Load only when selecting a model, tool, skill, reviewer, or escalation route.

## Selection

Filter by data permission, tool/artifact capability, authority, quality floor, deadline, context fit, and approved spend. Then choose the best complete-task route:

    expected completion time = queue + service + expected rework + review wait
    effective cost = task-tree marginal cost + allocated plan share + human time

Quota/reset/shared-pool pressure is a scheduling constraint, not fabricated cash cost.

## Default tiers

| Tier | Use | Default | Escalate / do not use |
|---|---|---|---|
| 0 | Parsing, conversion, approved template fill | Deterministic tool; local or Codex Luna | Not final authority |
| 1 | Routine code, copy, scripts, tests, synthesis | Luna/local | Sol/Sonnet for interpretation |
| 2 | Features, plans, multi-file work, strategy | Codex Sol or Copilot Sonnet | Astra/Opus for known hard cases |
| 3 | Architecture, concurrency, conflicting evidence, hard methodology | Codex Astra or Copilot Opus/Fable | Owner, not another model, when evidence/authority is missing |
| Gemini scout | File maps, logs, bounded independent evidence | AGY Flash | Never pricing, architecture, final review |

Use specialist review roles rather than generic same-model self-review.

## Capability routes

| Capability | Default | Output / escalation |
|---|---|---|
| Supplied-source extraction | Deterministic/local/Luna | Versioned evidence packet; Sol for conflicts |
| Current pricing comparison | Sol + official dated sources | Table with units/geography; Astra + numerical audit |
| Mechanical patch/code map | Local/Luna | Patch/tests; Sol/Sonnet if behavior ambiguous |
| Feature/implementation | Sol/Sonnet | Patch/tests/rollback; Astra/Opus for concurrency/migration |
| Statistical/causal analysis | Sol | Reproducible analysis; Astra + methodology review |
| Strategy/requirements/runbook | Sol/Sonnet | Native artifact/decision log; owner acceptance |
| Visual/video asset | Specialist generation tool | Cost per accepted asset/second; brand/rights review |
| Publish/send/update | Deterministic integration | Explicit authority and action record |

## Sensitive data and authority

Any sensitive task states classification, allowed location/provider account, permitted fields, retention, disclosure authority, and sanitization requirement. Missing permission blocks transfer, not public/sanitized work. Analysis or a valid artifact is never action authority.

## Short-form reel example

| Step | Route | Acceptance |
|---|---|---|
| Goal, story, storyboard | Sol/Sonnet | Brand owner accepts |
| Caption, shot list, metadata | Luna/local | Fits approved brief |
| Asset generation | Video/image tool | Cost per accepted asset/second and rights check |
| Assemble/caption/export | Deterministic media tool | Render/aspect/caption check |
| Publish | Authorized integration | Explicit post authority |
