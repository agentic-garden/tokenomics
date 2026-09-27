# Terminal-Bench v4.0 dossier

Source: [Artificial Analysis — Terminal-Bench v4.0](https://artificialanalysis.ai/evaluations/terminalbench-4-0)  
Verified: 2026-09-27 · Benchmark facts/rankings: refresh monthly · Pricing: not a provider quote

## What it evaluates

Terminal-Bench v4.0 contains 66 verified tasks spanning software engineering,
machine learning, science, operations, security, hardware and media. An agent
works through a terminal; every task has its own tests and a run passes only if
all tests pass. Artificial Analysis reports a pass@1 average over three repeats
per task using its `mini-swe-agent` harness. Version 4.0 recalibrated compute
and time allowances and removed eight problem tasks.

The page publishes benchmark-specific score, cost per task, separated token
categories and weighted decode time. Its time measure excludes time-to-first-
token and other overhead. That makes it unusually useful for identifying a
terminal-agent candidate, but it remains a measurement of AA's harness—not a
promise for Codex, Copilot, Claude Code, AGY, a subscription plan, or our repo.

## Current signal captured 2026-09-27

Claude Opus 5.5 Max/Xhigh and GPT-6 Astra Xhigh are tied at **59.6%**. This is
a terminal-task candidate signal only. Capture the source card's exact effort,
cost/task, token breakdown and weighted decode time at every refresh rather
than carrying a free-floating "best model" claim into another task family.

## Where this helps departments

| Work pattern | Evidence strength | Routing use |
|---|---|---|
| Repository/CLI debugging, build/test repair, deployment runbooks | High candidate signal | Select a high-tier terminal lead, then validate against local CI and rollback rules. |
| Technical operations, migration rehearsal, security investigation | Medium | The suite includes production-like operations/security tasks; use it to select challengers, not grant production authority. |
| Finance/marketing/HR/talent | None or incidental | Do not use a terminal score to choose a document, people, or creative model. |

Examples such as a zero-downtime MySQL-to-Postgres migration, photonic routing,
and a FreeCAD parametric task explain the breadth. They do **not** make this a
business-process or human-judgment benchmark.

## Tokenomics interpretation

Use Terminal-Bench to shortlist a lead for hard terminal work where an
independent verifier exists. A high score with large context/tool output may be
inferior in our environment if it consumes a scarce subscription window or
misses a real deadline due to queue/tool time. For a local comparison record:

```text
same repo snapshot + same task contract + same permissions + same context cap
+ same CI/verifier + same timeout + randomized order
= acceptance, plan-capacity delta, token fields (if exposed), elapsed time,
  human correction and rollback readiness
```

Do not use a Flash model as an unbounded terminal lead just because it is cheap;
do use lower-cost routes for bounded inventory, log classification and
deterministically verified transformations.

## Gaps and safeguards

- The pass@1 outcome is not an approval to run destructive commands, deploy,
  change infrastructure or handle secrets.
- The evaluation is not a measure of coding style, maintainability, team fit,
  product discovery, research freshness or regulatory compliance.
- Benchmark cost is observed API evaluation cost, not Copilot overage, Codex
  subscription value, or a token allocation on a personal plan.
- Keep task/model identities blind for reviewers. Assign separate review lenses:
  correctness/tests, data/API/rollback, architecture/concurrency, and security.

## Refresh fields

`version | page date | model ID | effort | harness | pass@1 | repeats | cost/task |
input/cache/reasoning/output/tool tokens | weighted decode time | source URL | notes`
