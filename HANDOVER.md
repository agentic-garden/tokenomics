# Tokenomics handover

Use this on any machine before changing routing, costs, local hardware, or the
monitoring design. Start with `README.md`; load only the task-specific module.

## Objective

Tokenomics is an orchestration reference for choosing the route that minimizes
accepted-task cost and time while holding a required quality/authority floor.
It supports subscription-first native CLIs, then bounded API/local/rental
overflow. It is not a provider marketing comparison and never fabricates a
subscription's hidden token allowance.

```text
Q = accepted / attempted
C = all task-tree cash / accepted parent task
T = queue + generation + tools + review + human correction
```

Choose two of Q/C/T as objectives; constrain the third. Every retry, child,
review and failed branch belongs to the accepted parent task once.

## Non-negotiable operating rules

- Route/model/provider/cost/quota data belongs to the orchestrator, not worker
  agents. Workers receive task, authority profile and acceptance criteria only.
- Use specialist, independent review roles; never rely on same-model
  self-review as the default.
- Raw command output, audio, transcripts, compiler logs and tool dumps remain
  artifacts. Pass a bounded evidence packet upward, never an unbounded dump.
- Subscription quotas are weighted capacity, not a token wallet. Record native
  CLI status before/after work; hidden capacity must not be converted into API
  tokens or dollars.
- A plan/API price, model catalog, quota, benchmark, or promotion fact expires
  seven days after verification. Keep source URL, verification date, geography
  and account scope.
- If a request says “use only Tokenomics,” do not browse or make claims beyond
  these committed files.

## Default routing

| Tier | Work | Default | Escalation |
|---|---|---|---|
| 0 | deterministic parsing/conversion | deterministic tool, qualified local model, or Codex Luna | never final authority |
| 1 | routine scripts, tests, bounded edits | Luna/local with deterministic checks | Sol if behaviour is ambiguous |
| 2 | multi-file features, plans, implementation | Codex Sol | Astra/Opus/Fable only for known hard cases |
| 3 | architecture, concurrency, conflict resolution | Codex Astra or Copilot Opus/Fable | human owner for missing authority/evidence |
| scout | file maps, bounded logs/evidence | AGY Flash | not unbounded implementation or final review |

For a Python audio-to-text/transcription feature: Sol Standard/Medium is the
implementation lead; Luna is for bounded fixtures/scripts; Astra Xhigh is the
hard-blocker/review route, not the default implementer.

## Verified effort and context facts (2026-09-29)

- API reasoning effort normally changes consumed reasoning/output tokens, not
  the published per-token rate. Do not invent `Max = N×` or `Ultra = N×`.
- OpenAI GPT-6 Astra API lists `low, medium, high, xhigh, max`; Codex may show
  `ultra`, but no separate API rate tier is published for it. Reasoning tokens
  bill as output tokens. OpenAI Pro mode performs more work at normal token
  rates and is a distinct higher-usage dispatch mode.
- GPT-6 Astra/Sol/Luna API has a genuine context-price split: above 272K input
  tokens (including cache reads/writes), the whole request is 2× input/cache
  and 1.5× output. Treat `short≤272K` and `long>272K` as separate routes.
- Gemini 3.8 Flash exposes low/medium/high and 1M context/64K output. Thinking
  tokens are billed as output; no long-context surcharge is published.
- Current Claude 5 material: effort changes token use, not published unit
  pricing; do not carry older Sonnet-4 long-context pricing into Claude 5.
- Consumer plans/Copilot do not publish a reliable effort-to-credit/token
  formula. Measure capacity deltas instead.

Full citations and service-tier rules live in `economics.md` and `capacity.md`.

## Local inference state

- Owned Windows host: RTX 4090 24GB, 64GB RAM, i9-13900K. First-party
  Bielik-11B observation at configured 32K: ~96.15 decode tok/s across 1,734
  tokens. It is the primary small-model local parser baseline.
- Owned M1 Max 64GB: Bielik-11B/Ollama observed 13–27 decode tok/s; portable
  fallback and Apple-runtime test host, not the primary low-latency parser.
- A local parser must be judged on precision/citation validity, p95 E2E,
  prefill throughput and accepted documents/hour—not decode TPS alone.
- Mac Studio M5 Max/Ultra is a credible capacity and local-serving contender;
  test the exact model, runtime, context and SRT fixture before buying. DGX
  Spark/Strix Halo are capacity-first, not automatic 4090 parser upgrades.
- `local-llm.md` is the detailed local-model, runtime, hardware and benchmark
  policy. Do not buy hardware without the prescribed local A/B evidence.

## Current repository state

Committed core work ends at `f36bb5d` (published effort/context rules). Recent
local-inference commits are `f390db9`, `ed6ae91`, `f5a7e9f`, `13a9e1d`,
`878ca39`, and `d95af5f`.

At handover creation, `README.md` has an uncommitted monitoring-module link and
`monitoring.md` is an untracked, user/other-agent authored monitoring policy.
Do not overwrite, stage, or discard either without reviewing its owner intent.
The branch was nine commits ahead of `origin/main` before this handover commit;
confirm push status before assuming another machine can see the work.

## Files to load by task

| Need | File |
|---|---|
| entrypoint and module loading | `README.md` |
| task/model routing | `routing.md`, then `benchmarking.md` when changing a route |
| API/plan/overage/local/rental economics | `economics.md` |
| context, quota, memory | `capacity.md` |
| task measurement / A-B | `measurement.md` |
| local models and hardware | `local-llm.md` |
| capability/department adaptation | `capabilities.md` |
| native CLI monitoring/authority (pending commit) | `monitoring.md`, `HANDOVER-monitoring-service.md` |

## Immediate next actions

1. Review the pending `README.md` and `monitoring.md`, then commit them only
   with their author's approval.
2. Push the committed branch after checking the remote and intentional scope.
3. Run representative A/B fixtures before promoting any model, reasoning
   effort, local runtime, or hardware purchase.
4. Refresh all pricing/model/benchmark facts once their seven-day expiry is
   reached; do not silently retain expired numbers.
