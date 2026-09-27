# Benchmark routing contract

Load only for route selection, a plan/API purchase, or a model A/B. Machine data:
[`benchmarks/snapshot-2026-09-27.yaml`](benchmarks/snapshot-2026-09-27.yaml).
Source URL inventory: [`benchmarks/crawl-manifest-2026-09-27.md`](benchmarks/crawl-manifest-2026-09-27.md).

## Non-negotiable filters

```text
eligible = data_allowed && tools_allowed && artifact_capable && authority_allowed
           && context_fits && quality_floor_possible
```

Safety/authority and quality floor are constraints, never tradeable objectives.
Benchmark scores are comparable only inside `benchmark + version + harness +
effort + task family`. `unknown` is a required value, not zero or an estimate.

## Three objectives; choose two, constrain the third

```text
Q = accepted / attempted                 # local verifier; benchmark score is only a prior
C = (api + overage + rental + tools + review + retries) / accepted_parent_task
T = queue + generation + tool + review + human_correction_to_accept
```

| Work mode | Optimize | Constrain | Typical use |
|---|---|---|---|
| `quality_budget` | Q↑, C↓ | T ≤ deadline | architecture, finance/research deliverable, normal feature |
| `quality_deadline` | Q↑, T↓ | C ≤ cap | incident, blocking CI, decision due today |
| `batch_verified` | C↓, T↓ | Q ≥ verifier floor | parsing, scripts, tests, transformations, sandbox automation |

Pareto rule: discard a route only when another eligible route has Q no lower,
C no higher, T no higher, and one strict improvement. Otherwise retain both and
run the declared mode locally. Never normalize a leaderboard score into dollars
or milliseconds.

## Required route record

```yaml
route:
  model: exact_id
  provider: exact_provider
  access: native_plan|copilot_overage|api|local|rental
  task_family: exact_benchmark_or_local_family
  effort: exact_setting
  q: {score: number|null, metric: pass_rate|objective_share|mean_score|index, source: url}
  c: {cash_per_task_usd: number|null, input: int|null, cache_hit: int|null,
      reasoning: int|null, output: int|null, tool: int|null, plan_capacity_delta: number|null}
  t: {elapsed_s: number|null, queue_s: number|null, decode_s: number|null,
      tool_s: number|null, human_s: number|null, steps: number|null}
  status: candidate|watch|test|active|demoted
```

`steps` is not time. DeepSWE deliberately does not publish wall-clock time;
keep `elapsed_s: null` until local measurement. A subscription with hidden
tokens/cash records `plan_capacity_delta`, not a fabricated API equivalent.

## Captured external performance/cost matrix

All rows are **DeepSWE v1.1 repository-change runs**. Cost is observed
evaluation API cost/task, not a personal API quote, overage, or plan value.
`T elapsed` is unknown for every DeepSWE row.

| Model / effort | Q pass % | C $/task | Output tokens | Steps | T elapsed | Candidate role |
|---|---:|---:|---:|---:|---|---|
| GPT-6 Astra Xhigh | 74 | 4.43 | 30k | 29 | unknown | hard lead |
| Gemini 3.8 Flash High | 74 | 2.36 | 143k | 166 | unknown | bounded cheap coding |
| Claude Opus 5 Max | 74 | 11.84 | 118k | 99 | unknown | independent high-quality lead |
| GPT-5.6 Sol Max | 73 | 6.46 | 60k | 61 | unknown | main substantial worker |
| GLM-5.3 Max | 69 | 3.99 | 80k | 124 | unknown | A/B challenger |
| Kimi K3 Max | 69 | 4.65 | 81k | 98 | unknown | A/B challenger |
| GPT-5.6 Luna Max | 67 | 0.61 | 73k | 102 | unknown | verified bounded worker |
| DeepSeek V4 Pro Max | 63 | 1.67 | 106k | 155 | unknown | low-cash challenger |
| GLM-5.3 Flash Max | 63 | 0.24 | 73k | 123 | unknown | verified mechanical work |
| DeepSeek V4.1 Flash Max | 53 | 0.46 | 108k | 153 | unknown | sandbox automation challenger |
| Claude Sonnet 5 Max | 54 | 26.40 | 214k | 268 | unknown | project-specific only |

Astra dominates the *measured Opus 5 run* on Q/C/steps (74%, $4.43, 29 versus
74%, $11.84, 99), but neither row proves subscription value or end-to-end
latency. Flash has equal Q and lower observed C than Astra but 4.8× output and
5.7× steps: use it with a scope, tool-call and output cap.

## Other benchmark priors — performance only unless source publishes C/T

| Family | Candidate | Q metric/value | C | T | Meaning |
|---|---|---:|---|---|---|
| Terminal-Bench 4.0 | GPT-6 Astra Xhigh | pass@1 59.6% | refresh | weighted decode: refresh | hard terminal candidate |
| Terminal-Bench 4.0 | Claude Opus 5.5 Max/Xhigh | pass@1 59.6% | refresh | weighted decode: refresh | independent terminal candidate |
| AutomationBench-AA | Claude Opus 5.5 Max | safe score 69.5%; objectives 90% | unknown | unknown | high-risk workflow plan/review |
| AutomationBench-AA | DeepSeek V4.1 Flash Max | safe score 68.9% | unknown | unknown | cheap sandbox workflow candidate |
| AutomationBench-AA | GPT-6 Astra Max | safe score 68.5%; objectives 89% | unknown | unknown | alternative workflow lead |
| APEX Agents | Claude Opus 5.5 Max | mean 73.5±4.9 | unknown | unknown | professional synthesis |
| APEX Agents | Claude Fable 5.1 Max | mean 68.6±4.9 | unknown | unknown | long-horizon synthesis |
| APEX Agents | Gemini 3.7 Flash High | mean 67.8±5.1 | unknown | unknown | structured-document challenger |
| AA-Omniscience | Claude Opus 5.5 Max | calibration index 46 | unknown | unknown | research synthesis; browse required |
| AA-Omniscience | GPT-6 Astra High | calibration index 44 | unknown | unknown | independent factual challenge |
| AA-Omniscience | Claude Fable 5.1 Max | calibration index 43 | unknown | unknown | independent factual challenge |

## Admission algorithm

```text
1. select benchmark family matching artifact; never cross-score families
2. filter eligible; load official current price and plan capacity from economics.md
3. choose {quality_budget, quality_deadline, batch_verified}
4. reject Q < floor, C > cap, or measured T > deadline
5. choose Pareto set on selected objectives; if T unknown, require local pilot
6. run >=3 representative tasks: same context/tools/verifier, randomized order
7. promote only if Q floor holds and selected-objective gain >=10%
8. retain fallback; demote after 2 clearly inferior accepted tasks
```

## Refresh

First business day monthly: refresh YAML values from the linked source views;
never infer missing C/T. Every seven days: refresh official API/overage/plan
prices in `economics.md`. Each local task writes Q/C/T fields to
`measurement.md`. Provider/model/cost/quota stay with the orchestrator, not the
workers.
