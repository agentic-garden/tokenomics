# Handover — Tokenomics monitoring service

Created: 2026-09-27  
Scope of next session: design a local-first monitoring service for native CLI
subscriptions, sessions and subagents. Do **not** redesign the agentic harness
or Tokenomics routing policy; produce an implementable telemetry architecture.

## Tokenomics goal

Choose the cheapest route that produces an accepted result by its deadline,
subject to data, authority, quality and side-effect constraints. The route
decision uses three independent measures:

```text
Q = accepted / attempted
C = all task-tree cash / accepted parent task
T = queue + generation + tool + review + human correction to acceptance
```

Each task chooses two to optimize and constrains the third:

```text
quality_budget   -> Q↑, C↓; T <= deadline
quality_deadline -> Q↑, T↓; C <= cap
batch_verified   -> C↓, T↓; Q >= verifier floor
```

The repo is `/Users/piotrkundu/tokenomics`, public at
`https://github.com/agentic-garden/tokenomics`. Current pushed HEAD:
`60e234f Encode benchmark cost time performance routing`.

Useful existing files:

- `benchmarking.md` — Q/C/T decision contract.
- `economics.md` — native plan/API/overage/local/rental accounting rules.
- `measurement.md` — minimum local measurement ledger.
- `capacity.md` — context/quota/local-memory constraints.
- `benchmarks/snapshot-2026-09-27.yaml` — external benchmark prior data.

## Real Copilot session observation — required design input

Copilot session `/status` report, supplied 2026-09-27:

```text
Context before switch:
  kimi-k3 high: 529k / 1049k tokens (50%)
  System Prompt: 7.7k
  System Tools: 8.8k
  MCP Tools: 1.5k
  Messages: 511.2k
  Free Space: 342.3k
  Buffer: 176.9k

Then session model changed:
  kimi-k3 high -> gpt-6-luna max (1M context)

180-day session/activity aggregate:
  messages: 1018
  changes: +51,361 / -1,062
  AI credits: 30,994.8 AIC; displayed duration: 620h 26m 4s
  token aggregate: input 469.8m; cached input 449.9m; written 11.5m;
                   output 2.2m; reasoning 712.2k
  model rows:
    claude-sonnet-5: input 259.5m; cached 252.6m; written 7.0m;
                      output 1.6m; reasoning 591.5k; 8421.96 AIC
    kimi-k3: input 168.5m; cached 160.1m; output 188.5k;
             reasoning 28.6k; 7596.58 AIC
    claude-opus-5: input 41.6m; cached 37.1m; written 4.5m;
                    output 404.3k; reasoning 91.3k; 5648.61 AIC
    gpt-5.6-sol: input 232.2k; cached 177.8k; written 54.3k;
                 output 10.5k; reasoning 877; 27.66 AIC
  plan: 20,000 / 20,000 AIC, 100% used
```

Important integrity rules:

- The listed model AIC rows total about 21,695 AIC, not 30,995 AIC. Preserve
  the remainder as `unattributed_aic`; never force reconciliation.
- `written` may mean cache-write tokens, not model output. Keep it separate.
- The report is an aggregate over 180 days, not necessarily one session.
- A session model switch means context, credits and token rows must have time
  ranges and model-attribution boundaries; never label all session tokens as
  the final selected model.
- A context bar is capacity telemetry, not billed-token telemetry.

## Monitoring service requirements

### Sources / adapters

Native, plan-authenticated CLIs first: Codex, GitHub Copilot CLI, AGY and
Claude Code. API adapters and local/rental adapters may be added separately.
Do not require API keys to monitor native subscription use. Prefer existing
status commands, session exports, JSON output/logs and filesystem-visible
session state. Use a capability matrix: `available | unavailable | estimated |
unknown` per provider/CLI/version rather than pretending every source exposes
the same fields.

### Core data model

```text
workspace -> task(parent) -> session -> model_segment -> agent/subagent_attempt
                                  -> tool_event / context_snapshot / quota_snapshot
```

Required correlation fields:

```text
event_id, timestamp_utc, machine_id, user_id, workspace_id, task_id,
parent_task_id, session_id, parent_session_id, agent_id, parent_agent_id,
cli, cli_version, provider, model, effort, context_tier, access_lane,
source_type, confidence, raw_source_ref
```

Token fields must remain separate:

```text
input_tokens, cached_input_tokens, cache_write_tokens, reasoning_tokens,
output_tokens, tool_tokens, unknown_tokens
```

Cost/capacity/time fields:

```text
direct_api_cash, overage_cash, ai_credits, unattributed_ai_credits,
plan_window_before_after, weekly_before_after, monthly_before_after,
context_used, context_limit, context_free, context_buffer,
queue_ms, generation_ms, tool_ms, waiting_ms, human_interaction_ms,
start_at, end_at, wall_ms, accepted, verifier_result, rework_count
```

### Required views

1. **Live status line:** active model/effort, context used/limit/free/buffer,
   session AIC, plan window/weekly/monthly remaining and reset time, children
   active/blocked, task budget/deadline. Display `unknown`, never a fake zero.
2. **Session view:** timeline of model switches, context growth, tool waiting,
   interruptions, child agents, quota snapshots and raw source links.
3. **Task-tree roll-up:** all children/reviews/retries count once under the
   accepted parent artifact; split attributed vs unattributed use.
4. **Route comparison:** Q/C/T per task family, access lane and model segment.
   Never compare scores across benchmark families.
5. **Capacity planner:** predicts only from observed local history; gives a
   range/confidence, not an assumed token allowance for opaque subscriptions.

### Event handling and control

- Interrupt a stuck agent, preserving partial work and logs.
- Detect no-progress loops: repeated tool command/signature, no file/diff/test
  state change, rising context/AIC, or waiting longer than task policy permits.
- Emit state transitions (`queued`, `running`, `waiting_external`, `blocked`,
  `needs_human`, `interrupted`, `finished`, `verified`, `accepted`).
- External events (CI completion/failure/webhook) resume a parked task rather
  than a model sleeping in a blocking tool call.
- Monitoring is read-first. A kill/interrupt action requires an explicit local
  control event and audit record.

### Privacy / multi-user / routing constraints

- Local-first SQLite/event-log design; export opt-in; no central server needed
  for first release.
- Multi-machine/user sync must expose only authorized workspace/task metadata.
- Orchestrator can see route, provider, cost and quota. Workers/reviewers get
  task/artifact/acceptance information, not competing-model identities/prices.
- Do not pool or rotate personal provider credentials. Support legitimate task
  handoff between separate users/accounts by transferring task state/artifacts,
  not credentials or quota.

## Deliverables requested from next session

1. A concise technical design: processes, adapters, event schema, local store,
   transport, TUI/statusline and failure recovery.
2. A feasibility table for each native CLI: exact telemetry obtainable now,
   likely source/location/command, unavailable fields, and parsing risk.
3. An MVP sequence that produces value without modifying third-party CLIs.
4. A test plan using recorded/redacted fixture outputs, including the Copilot
   report above and model-switch/subagent/unattributed-credit cases.
5. Explicit non-goals and a boundary with the separate public agent-harness
   idea.

Do not implement or modify repository files until the design is reviewed.
