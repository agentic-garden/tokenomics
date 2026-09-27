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

## Time-of-day API pricing

**Peak/off-peak** means the provider charges different API rates according to the
time the request begins. It is not a reasoning setting, service tier, or quality
difference. Schedule only deferrable, idempotent batch work into a cheaper
window; never delay an interactive task or a recovery/verification step solely
to save tokens.

| Provider/model | Current published window | Peak price / 1M (cache-hit input; miss input; output) | Off-peak price / 1M | Routing rule |
|---|---|---:|---:|---|
| DeepSeek V4.1 Flash (`deepseek-flash`) | **Peak:** weekdays 01:00–04:00 and 06:00–10:00 UTC. Off-peak: all other times. | $0.006; $0.30; $1.20 | $0.003; $0.15; $0.60 | Queue large non-urgent extraction, test generation, and first-pass analysis off-peak. |
| DeepSeek V4 Pro (`deepseek-v4-pro`) | Same schedule | $0.044; $1.32; $3.96 | $0.022; $0.66; $1.98 | Queue bounded independent reviews off-peak; do not use this price difference to defer a decision owner needs now. |

For Stockholm, the DeepSeek peak windows are 03:00–06:00 and 08:00–12:00 during
CEST, or 02:00–05:00 and 07:00–11:00 during CET. All other local times are
off-peak. The provider can change prices or windows: verify the official source
before a scheduled workload and expire this fact after seven days.

Record `provider_time_zone`, `request_started_at_utc`, `rate_window`,
`input_tokens`, `cache_hit_tokens`, `output_tokens`, and `invoice_cost` for
every timed-price batch. A time window is provider/model-specific; do not apply
DeepSeek's schedule to Kimi, GLM, Qwen, Copilot overage, subscriptions, or local
models without a separate current primary source.

Source: https://api-docs.deepseek.com/quick_start/pricing/ (verified 2026-09-27;
refresh by 2026-10-04).

## Budget admission

Set hard cash cap, subscription-capacity share, maximum children/tool calls, deadline, and one recovery attempt. Reserve capacity to finish an already-started lead task and its required review.
