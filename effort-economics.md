# Reasoning-effort economics registry

Load only when choosing a reasoning level, comparing plan/overage/API routes,
or measuring quota burn. This file prevents a UI's effort picker from becoming
an invented pricing table.

## Economic identity rule

A **route** is `provider + exact model + access lane + economic effort tier`.
An effort label is not an economic tier until its cost or quota multiplier has
been measured.

```text
same verified multiplier (for example Max = 2x and Ultra = 2x)
  -> one economic model: `<model> / elevated-2x`; retain both UI labels as aliases

different verified multipliers (for example Max = 2x, Ultra = 4x)
  -> two economic models: `<model> / max-2x`, `<model> / ultra-4x`

unknown multiplier
  -> `<model> / effort-unresolved`; never budget, price, or merge it as if known
```

This applies independently to direct API cash, Copilot overage credits, and
subscription quota. A matching API token price does **not** prove a matching
subscription quota multiplier. A plan's hidden weighted limit does **not**
prove a token or dollar multiplier.

## Current evidence inventory

The rows below are every model/effort combination currently captured in the
Tokenomics KB. `observed task cash` comes from a particular benchmark harness;
it is not an effort-multiplier experiment. Therefore every `multiplier` below
remains `unknown` unless a same-task, same-access-lane measurement says
otherwise.

| Provider / access family | Exact model | Seen effort labels | Observed task evidence | API cash multiplier | Plan/quota multiplier | Economic route today |
|---|---|---|---|---|---|---|
| OpenAI / Codex or API | GPT-6 Astra | High, Xhigh, Max; Ultra requested but not recorded | Xhigh: $4.43/task, 30k output, 29 steps; High/Max score-only | unknown | unknown | `GPT-6 Astra / effort-unresolved` |
| OpenAI / Codex or API | GPT-5.6 Sol | Max | Max: $6.46/task, 60k output, 61 steps | unknown | unknown | `GPT-5.6 Sol / effort-unresolved` |
| OpenAI / Codex or API | GPT-5.6 Luna | Max | Max: $0.61/task, 73k output, 102 steps | unknown | unknown | `GPT-5.6 Luna / effort-unresolved` |
| Anthropic / plan, Copilot or API | Claude Opus 5 | Max | Max: $11.84/task, 118k output, 99 steps | unknown | unknown | `Claude Opus 5 / effort-unresolved` |
| Anthropic / plan, Copilot or API | Claude Opus 5.5 | Max, Xhigh | performance-only records | unknown | unknown | `Claude Opus 5.5 / effort-unresolved` |
| Anthropic / plan, Copilot or API | Claude Fable 5.1 | Max | performance-only records | unknown | unknown | `Claude Fable 5.1 / effort-unresolved` |
| Anthropic / plan, Copilot or API | Claude Sonnet 5 | Max | Max: $26.40/task, 214k output, 268 steps | unknown | unknown | `Claude Sonnet 5 / effort-unresolved` |
| Google / AGY, Copilot or API | Gemini 3.8 Flash | High | High: $2.36/task, 143k output, 166 steps | unknown | unknown | `Gemini 3.8 Flash / effort-unresolved` |
| Google / AGY, Copilot or API | Gemini 3.7 Flash | High | performance-only record | unknown | unknown | `Gemini 3.7 Flash / effort-unresolved` |
| DeepSeek / API, local or rental | DeepSeek V4 Pro | Max | Max: $1.67/task, 106k output, 155 steps | unknown | unknown | `DeepSeek V4 Pro / effort-unresolved` |
| DeepSeek / API, local or rental | DeepSeek V4.1 Flash | Max | Max: $0.46/task, 108k output, 153 steps | unknown | unknown | `DeepSeek V4.1 Flash / effort-unresolved` |
| Zhipu / API, local or rental | GLM-5.3 | Max | Max: $3.99/task, 80k output, 124 steps | unknown | unknown | `GLM-5.3 / effort-unresolved` |
| Zhipu / API, local or rental | GLM-5.3 Flash | Max | Max: $0.24/task, 73k output, 123 steps | unknown | unknown | `GLM-5.3 Flash / effort-unresolved` |
| Moonshot / API, local or rental | Kimi K3 | Max | Max: $4.65/task, 81k output, 98 steps | unknown | unknown | `Kimi K3 / effort-unresolved` |

`Max` in a third-party benchmark row can describe the evaluator's selected
mode, not necessarily a provider's billable reasoning tier. Keep that
ambiguity until the source records the exact provider/model/effort setting.

## Measurement protocol: settle a multiplier

Run each selectable effort against the same model, access lane, account, task
fixture, prompt, tool allowance, context state, verifier and time window.
Do not compare a successful Max run with a failed High run and call their
token/cost difference an effort multiplier: retries and tool output must stay
in the task-tree ledger.

```text
For direct API:
  direct_multiplier(effort) = invoice_cash(effort) / invoice_cash(baseline)
  also retain input, cache-hit, cache-write, reasoning, output and tool tokens.

For Copilot overage:
  overage_multiplier(effort) = credits_delta(effort) / credits_delta(baseline)

For a subscription CLI:
  quota_multiplier(effort) = capacity_delta(effort) / capacity_delta(baseline)
```

Use at least three warm, identical repetitions per setting. Record median and
range. A multiplier is `verified` only when its observed range is stable enough
that different groups cannot overlap under the chosen tolerance:

```text
same tier      = medians within ±10% and ranges overlap
different tier = medians differ by >20% and ranges do not overlap
uncertain      = everything between; retain separate measurement records, do not merge
```

`reasoning_tokens` may make direct API cash rise even when an API's published
input/output rates do not change. On subscriptions the provider may expose no
reasoning tokens at all; then only the measured capacity delta is valid.

## Dispatch rule

1. Prefer the lowest verified economic tier that passes the task's quality and
   deadline gates.
2. Use a higher effort only with a stated reason: expected quality gain,
   shorter accepted time, or fewer retries.
3. If effort is unresolved, set a per-attempt quota/cash cap and treat it as a
   pilot—not as a cheaper or equivalent route.
4. Re-run after a provider changes the model, CLI, plan, pricing, quota UI or
   reasoning implementation.
