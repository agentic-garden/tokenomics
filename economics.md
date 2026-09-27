# Economics and budget policy

Load only for purchases, renewals, overage/API/local/rental comparisons, or a task with a cash budget.

## Cost model

    marginal cost = API + overage + cloud/rental + local electricity for every task attempt
    allocated subscription cost = stated optional share of a plan; never pretend it is tokens
    effective cost = marginal cost + allocated subscription cost + human time

Retries, child work, review, escalation, and abandoned branches belong to the task tree once only.

## Decision rules

1. Existing subscription capacity has low marginal cash cost but is scarce; spend it on work that avoids paid overflow or delay.
2. Do not upgrade for one burst. Compare the renewal-period cost against bounded approved API/overage/rental use and parking.
3. API is for explicitly authorized automation/CI/overflow with project caps; consumer subscriptions do not fund it.
4. Local wins only after representative quality/latency benchmarks; owned hardware still costs power, attention, and workstation capacity.
5. Rental wins only when the queued batch's all-in provisioning, load, runtime, storage, egress, and shutdown cost beats the deadline alternative.

## Portfolio ledger

    provider, account scope, currency/amount, renewal, cancellation deadline,
    shared allowance, expiry, utilization, accepted parent tasks, displaced overflow,
    source, verified date

Before renewal compare retain, downgrade, remove, upgrade, or replace using actual displaced work, not token face value.

## Price fact ledger

    provider | plan/model | input/cached/cache-write/output price per 1M or plan entitlement
    | source URL | verified_at | expires_at | geography/account scope | notes

Official provider documents first; logged-in dashboard second; CLI status for actual quota/reset; community reports only as leads.

## Budget admission

Set hard cash cap, subscription-capacity share, maximum children/tool calls, deadline, and one recovery attempt. Reserve capacity to finish an already-started lead task and its required review.
