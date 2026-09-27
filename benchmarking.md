# Benchmarking and efficiency policy

Load only when selecting or re-ranking a model, changing a default route, evaluating a plan/provider, or running an A/B test. A public leaderboard is evidence for a candidate list, never an automatic routing decision.

## What efficiency means

Cheap tokens are not cheap work if a route emits excessive context/output, fails and retries, needs expensive review, or takes too long. Price the accepted parent task:

```text
expected_cash_to_accepted =
  (model + cache + tools + overage + local/rental cost across all attempts and reviews)
  / accepted_parent_deliverables

time_to_accepted = queue_wait + model/tool elapsed time + human active correction/review

token_efficiency = accepted_parent_deliverables
  / (input + cached input + reasoning + output + tool tokens)
```

Keep input, cached input, reasoning, output, and tool-token fields separate. Do
not use one blended token count. Subscription lanes that do not expose tokens
receive `tokens: unknown`; compare them by accepted task, capacity consumed,
elapsed time, and human correction instead.

## Routing scorecard

For each candidate route, store these values for the *same task family*, task
contract, harness, effort level, tools, context policy, and verifier:

| Dimension | Question | Direction |
|---|---|---|
| Acceptance | Did it meet the defined checks without unsafe/invalid changes? | Higher |
| First-pass rate | How often did one lead attempt pass? | Higher |
| Token efficiency | How many separated tokens were needed per accepted parent task? | Lower |
| Cash to accepted | What did all attempts, child work, and review cost? | Lower |
| Time to accepted | How long until a human can safely accept it? | Lower |
| Human correction | How much focused human work was required? | Lower |
| Capacity pressure | Did it consume a scarce plan window or credit pool? | Lower |
| Reliability/safety | Did it obey authority and side-effect constraints? | Higher |

There is no universal winner. A model may cost 50% more per token yet win if it
uses half the tokens, avoids one retry/reviewer, or meets a deadline that a
cheaper route misses. Conversely, a five-times-faster route is not automatically
better if it creates five times the correction burden. Declare the task's
deadline and the decision owner's value of elapsed time before weighting time.

### Dominance rule

Route A is a default candidate over Route B only if it is at least as good on
acceptance and safety, and is lower on either expected cash-to-accepted or
time-to-accepted without materially worsening the other. Otherwise retain both
as task-specific routes and run a local A/B test. Never promote a route merely
because it is ranked #1 on an unrelated benchmark.

## External benchmark pre-filter

Capture the exact date, benchmark version, model version, reasoning effort,
harness, pricing snapshot, and uncertainty. Public scores cannot be compared
across different harnesses, tool permissions, time limits, or grading methods.

The source-specific methodology, department coverage and known gaps live in
[`benchmarks/README.md`](benchmarks/README.md). Load the relevant dossier before
using a source as routing evidence. AutomationBench is direct evidence for
constrained Finance/HR/Marketing/Operations/Sales/Support SaaS workflows; APEX
is professional-services evidence; AA-Omniscience measures knowledge and
calibration, not web research; and none of these sources qualifies automation
of talent or employment decisions.

| Source | What it measures | Tokenomics use | Do not infer |
|---|---|---|---|
| [Terminal-Bench v4.0](https://artificialanalysis.ai/evaluations/terminalbench-4-0) | 66 verified terminal tasks across coding, ML, science, ops, security, hardware, and media | Primary pre-filter for native CLI coding/terminal agents. Capture pass@1, cost/task, token breakdown, and weighted decode time. | That a model is best for normal product coding or that another harness will reproduce its result. |
| [DeepSWE v1.1](https://deepswe.datacurve.ai/blog/deepswe-v1-1) | 113 repository-change tasks, isolated patch verification | Primary pre-filter for Python/Go/C++ implementation agents. Capture pass rate, cost/task, output tokens, and steps. | Wall-clock superiority: the benchmark intentionally does not report wall time. |
| [AutomationBench-AA](https://artificialanalysis.ai/evaluations/automationbench-aa) | 657 cross-application REST/SaaS workflows with guardrail violations penalized | Pre-filter for tool-using automation and future department-agent workflows. | Permission safety in our environment; our own side-effect policy still governs. |
| [APEX-Agents](https://www.mercor.com/apex/apex-agents-leaderboard/) | 240 long-horizon cross-application professional-services tasks in 31 worlds | Pre-filter for high-level orchestrators, research/planning, and document-heavy work. | Software-engineering or terminal-agent quality by itself. |
| [AA-Omniscience](https://artificialanalysis.ai/evaluations/omniscience) | Factual recall and hallucination across economically relevant domains | Add a reliability penalty for research, pricing, factual KB, and evidence tasks. | Live-web research ability or source verification; require browsing/citations separately. |

Use official provider pricing for cash calculations, not leaderboard price cards.
Benchmark price/task is a useful comparison *within that evaluation* because it
includes observed token behavior; it is not a quote for our plan, API account,
or harness.

## Monthly refresh and strategy update

Run on the first business day of every month; perform an out-of-cycle refresh
within seven days of a material model release, price change, provider limit
change, or a route repeatedly missing acceptance/time targets.

1. Snapshot each source above with date, version, current top candidates, effort/harness, score, cost/task, token fields, and time field if published.
2. Refresh official model/API pricing, cache prices, time-of-day rates, plan tiers, and observed local capacity. Every external price fact expires after seven days.
3. Build a **challenger list**, not new defaults: retain only models available through an allowed native plan, approved API, or tested local/rental route.
4. Compare candidates only inside their relevant benchmark families; flag result changes that exceed the source's stated uncertainty or a 10% practical delta.
5. Run local A/B tests before changing a default route. Update routing only with measured accepted-task evidence; otherwise note `watch` with no policy change.
6. Record retain/promote/demote/retire decision, evidence links, owner, expiry date, and rollback route in `measurement.md`'s ledger.

The refresh must explicitly state `no route change` when external rankings move
but no representative local evidence exists. This prevents leaderboard churn
from turning the fleet into an unstable model lottery.

## Local A/B protocol

Use a representative, non-public test set with at least three task families:
bounded mechanical work, normal implementation/test repair, and difficult
architecture/debugging. Add a research/evidence family when that is a real use
case. Do not leak which provider/model is being tested to workers or reviewers.

```text
same task contract + same repository snapshot + same tools/permissions
+ same maximum context policy + same independent verifier
+ randomized route order + repeated tasks where practical
= comparable evidence
```

For each route record:

```text
model, provider, CLI/harness, model effort, task family, context supplied,
cache state, input/cache-hit/reasoning/output/tool tokens when exposed,
plan capacity delta or API/local cost, attempts, child/review cost,
elapsed time, human active time, verifier outcome, defects, accepted outcome
```

Use an independent reviewer/verifier with a different role and, where possible,
a different model family. The reviewer receives task requirements and artifacts,
not the identity, price, or score of the producing model.

### Promotion guardrails

- A cheap challenger must match the incumbent's acceptance/safety gate before cost matters.
- A high-tier challenger must earn its extra cost by reducing accepted-task cost, avoiding a deadline miss, or materially reducing human correction.
- Test a small sample first; stop a route that fails a safety/authority gate or has two consecutive clearly inferior accepted-task outcomes.
- Preserve the former route as fallback until the new route has enough representative wins; do not replace all workload types from one benchmark result.

## Current evidence note — 2026-09-27

The cited sources already demonstrate why list-price ranking fails. For example,
DeepSWE v1.1 currently displays score, average cost, output tokens, and agent
steps together; it does not publish wall-clock time because host/provider load
makes it inconsistent. Terminal-Bench v4.0 provides score, cost/task, token
breakdown, and weighted decode time. These are precisely the fields the local
ledger must preserve rather than collapsing into a model name or price/M-token.

### Initial routing snapshot

This is a challenger/routing snapshot, not a universal model ranking. **Never
average, normalize, or rank values across these task families.** Values are only
comparable inside the named evaluation, using its exact harness and effort.

#### Terminal / CLI agent work — Terminal-Bench 4.0

Opus 5.5 Max/Xhigh and GPT-6 Astra Xhigh are tied at **59.6%**. They remain
high-tier terminal-coding candidates. This score says nothing about their
repository-change cost, long-horizon document work, or factual calibration.

#### Repository implementation and repair — DeepSWE v1.1

| Configuration | Pass rate | Evaluation cost/task | Output tokens/task | Steps/task |
|---|---:|---:|---:|---:|
| GPT-6 Astra Xhigh | 74% | $4.43 | 30k | 29 |
| Gemini 3.8 Flash High | 74% | $2.36 | 143k | 166 |
| Claude Opus 5 Max | 74% | $11.84 | 118k | 99 |
| GPT-5.6 Sol Max | 73% | $6.46 | 60k | 61 |
| GLM-5.3 Max | 69% | $3.99 | 80k | 124 |
| Kimi K3 Max | 69% | $4.65 | 81k | 98 |
| GPT-5.6 Luna Max | 67% | $0.61 | 73k | 102 |
| DeepSeek V4 Pro Max | 63% | $1.67 | 106k | 155 |
| GLM-5.3 Flash Max | 63% | $0.24 | 73k | 123 |
| DeepSeek V4.1 Flash Max | 53% | $0.46 | 108k | 153 |
| Claude Sonnet 5 Max | 54% | $26.40 | 214k | 268 |

**Astra is cheaper than the measured Opus configuration here**: $4.43/task
versus $11.84/task for Claude Opus 5 Max, at the same 74% pass rate—about
**2.7x lower evaluation cost**. That is the valid comparison. The evaluation
does not provide an Opus 5.5 row in this snapshot, so no cost claim about Astra
versus Opus 5.5 is made here.

Gemini 3.8 Flash's equal pass rate hides 4.8x output and 5.7x steps versus
Astra. Keep it as a bounded scout rather than an unbounded AGY lead. Luna is a
budget bounded-worker candidate; GLM/Kimi are local A/B challengers. Sonnet 5
is not an economic default for this task family based on this result.

#### Cross-application automation — AutomationBench-AA

Opus 5.5 Max scores **69.5%**, DeepSeek V4.1 Flash Max **68.9%**, and Astra Max
**68.5%**. Opus completes **90%** of objectives. DeepSeek Flash is therefore a
serious cheap automation challenger, but it still needs our permission and
side-effect gates before work leaves a draft/sandbox environment.

#### Long-horizon professional synthesis — APEX-Agents

Opus 5.5 Max scores **73.5% ±4.9**, Fable 5.1 Max **68.6% ±4.9**, and Gemini
3.7 Flash High **67.8% ±5.1**. In the management-consulting subset, Gemini 3.8
Flash is **77.5%** versus Opus 5.5 at **80.0%**. Retain Opus/Fable as high-tier
synthesis candidates and Gemini Flash as a structured-document challenger.

#### Factual recall and calibration — AA-Omniscience

Opus 5.5 Max scores **46**, Astra High **44**, and Fable 5.1 Max **43** on the
calibration-aware index. Use high-tier independent research synthesis for
factual KB work, but always require primary-source browsing and citations.

### Provisional routing by task family

These are orchestrator defaults from the snapshot above. They are not worker
instructions, and workers must not be told the producing/reviewing model's
identity, quota, or price. Native-plan routes consume plan capacity; API routes
require explicit task authorization and a hard cash cap.

| Task family | Lead route | Budget / overflow route | Do not default to | Why this is the current policy |
|---|---|---|---|---|
| Hard terminal coding, difficult debugging, systems work | GPT-6 Astra Xhigh through Codex, when available | Claude Opus 5.5 through Copilot for an independent lead/reviewer | Flash or local models as autonomous lead | Terminal-Bench ties Astra/Opus at the top; DeepSWE makes Astra materially cheaper than measured Opus 5 for repository work. |
| Normal multi-file implementation and test repair | GPT-5.6 Sol, medium/high, with a bounded task contract | GPT-5.6 Luna for contained changes; GLM-5.3 as approved API challenger | Claude Sonnet 5 as assumed economic default | Sol is near the leading DeepSWE pass rate with lower observed cost/steps than Opus. Luna/GLM trade acceptance for lower cash and require verifier coverage. |
| Architecture, long-horizon planning, difficult trade-offs | Claude Opus 5.5; Fable 5.1 for a separately bounded second opinion | GPT-6 Astra for an independent challenge/review | Flash, Luna, or local models deciding architecture | APEX and AA-Omniscience favor Opus/Fable/Astra on long-horizon and calibrated work. |
| Repository inventory, compiler-log grouping, extraction, constrained summaries | Gemini 3.8 Flash at low/medium effort, tightly scoped | Local qualified model; Luna; DeepSeek Flash API if authorized | AGY Flash long-running autonomous implementation | Gemini's DeepSWE result is capable but consumes 143k output/166 steps; bound context, tool calls, and output. |
| Mechanical edits, boilerplate, test drafts, transformations with a deterministic verifier | GPT-5.6 Luna | GLM-5.3 Flash or local qualified model; DeepSeek Flash only with API authorization | Any frontier model by default | Low cash routes win only where the verifier catches errors and failure is cheap. |
| Cross-application automation / tool execution | DeepSeek V4.1 Flash in sandbox/draft mode | Opus 5.5 or Astra for high-risk plan/review, with a human authority checkpoint | Autonomous production side effects by a cheap model | DeepSeek Flash nearly matches leaders on AutomationBench, but benchmark guardrails do not grant our real-world authority. |
| Research, current pricing, factual KB, evidence packet | Astra, Opus 5.5, or Fable 5.1 as synthesizer **with browsing** | Gemini Flash/Luna/local only for source extraction and structuring | Model-memory-only answer, regardless of benchmark score | AA-Omniscience is a calibration signal, not live factual access; primary sources remain required. |
| Independent code review | Assign review lenses: correctness/tests; API/data/rollback; architecture/concurrency; security/permissions | Use a different model family from the lead where capacity permits | Same-model self-review as the only review | Diversity is in the review question and evidence; blind the reviewer to producer identity and cost. |

### Challengers and demotions

- **Promote to active A/B:** GLM-5.3 for ordinary implementation/review, Kimi K3 for an independent long-context implementation review, DeepSeek V4.1 Flash for sandboxed automation, and Luna for bounded verified work.
- **Keep bounded:** Gemini 3.8 Flash. Its benchmark pass rate does not justify unbounded agent loops under an opaque weekly AGY quota.
- **Demote from economic default:** Claude Sonnet 5 for repository implementation. It can still be used where a local project test proves it wins, but its listed DeepSWE run has the worst cost/output/step profile of the candidates above.
- **Not benchmark-qualified yet:** local Qwen/DeepSeek distills and Gemini 3.1 Pro. Route them only to low-risk, independently checked work until they have local A/B evidence on this hardware.

Sources and snapshot date: Terminal-Bench 4.0, DeepSWE v1.1, AutomationBench-AA,
APEX-Agents, and AA-Omniscience, captured 2026-09-27. Public benchmark costs are
evaluation-specific API estimates, not plan-token equivalents or personal quotes.

The linked YouTube item is treated as a lead, not an authority: the accessible
page did not expose a verifiable transcript. The benchmark sites and official
provider documents above are the auditable sources for policy updates.
