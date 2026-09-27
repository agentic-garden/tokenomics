# Measurement and evaluation

Load only for route evaluation or subscription portfolio decisions.

## Minimum ledger

    parent_task_id, capability, artifact, benchmark_family, route/model/access/effort,
    skill_version, data_class, mode(quality_budget|quality_deadline|batch_verified),
    quality_floor, cash_cap, deadline, start/end, queue/rate-limit idle,
    generation/tool/review/human seconds, context/cache state, capacity before/after,
    input/cache-hit/cache-write/reasoning/output/tool tokens when exposed,
    api/overage/rental/local/tool/review cash, human minutes, retry/escalation count,
    acceptance, defect escape, owner/reviewer

Attach every child attempt to the original accepted parent deliverable. Derive
`Q=accepted/attempted`, `C=all-cash/accepted-parent-task`, and
`T=queue+generation+tool+review+human-correction`. Keep unknown token/plan fields
null; never substitute list price or a made-up token equivalent.

## A/B tests

Compare economy and quality-first routes on the same task class, inputs, rubric, deadline, and review portfolio. Count failures, timeouts, abandoned work, human correction, and later defect escapes. Record cache state, load, quota/reset, and rate-limit waits.

Before testing declare the quality floor, primary objective, minimum meaningful improvement, maximum test spend, and sample-extension rule. Preserve original attempts when changing prompts/rubrics and rerun both arms.

Keep a route only if it meets the quality floor and wins its declared objective without unacceptable regression elsewhere. Re-evaluate after material price, model, workload, codebase, or quality change.

## Monthly benchmark decision record

    refresh_date, source/version, model/version, effort, harness,
    score/uncertainty, observed cost/task, observed token fields, observed time,
    comparable task family, candidate status (retain|watch|test|promote|demote|retire),
    local evidence links, decision, rollback route, owner, expires_at

An external leaderboard entry becomes `watch`, never `promote`, until a local A/B
test provides representative accepted-task evidence. See `benchmarking.md`.
