# Measurement and evaluation

Load only for route evaluation or subscription portfolio decisions.

## Minimum ledger

    parent_task_id, capability, artifact, route/model tier, skill version, data class,
    start/end, queue/rate-limit idle, context/cache state, capacity before/after,
    tokens when exposed, direct cost, human minutes, retry/escalation count,
    acceptance, defect escape, owner/reviewer

Attach every child attempt to the original accepted parent deliverable. Measure effective cost per accepted parent task, time to accepted, quality/defect outcome, and accepted tasks per wall-clock interval.

## A/B tests

Compare economy and quality-first routes on the same task class, inputs, rubric, deadline, and review portfolio. Count failures, timeouts, abandoned work, human correction, and later defect escapes. Record cache state, load, quota/reset, and rate-limit waits.

Before testing declare the quality floor, primary objective, minimum meaningful improvement, maximum test spend, and sample-extension rule. Preserve original attempts when changing prompts/rubrics and rerun both arms.

Keep a route only if it meets the quality floor and wins its declared objective without unacceptable regression elsewhere. Re-evaluate after material price, model, workload, codebase, or quality change.
