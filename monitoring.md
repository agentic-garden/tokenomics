# Monitoring native agent work

Load for a live task, an agent/CLI authority decision, session or quota observation, an interruption, or a post-task Q/C/T record. This is the **current native-CLI operating policy**. It does not require, or describe, a custom agent harness; the separate future harness plan is maintained outside this repository in the KBB ideas workspace.

## Purpose and boundary

Monitoring has two equally important jobs:

1. report work, capacity, cost evidence, progress and acceptance; and
2. enforce and report the authority an agent was granted.

Without a custom harness, enforcement is limited to the controls the CLI and operating system actually apply at launch. A monitor can observe an independently-started interactive session, but cannot retroactively make it read-only. Never represent a prompt, an `AGENTS.md` instruction, or an after-the-fact diff check as enforcement.

| State | Meaning |
|---|---|
| `enforced` | A CLI sandbox, OS boundary, explicit tool rule, or isolated worktree prevented an action. |
| `approved` | A human explicitly allowed a bounded exception. |
| `observed_only` | The monitor saw the action but had no technical control over it. |
| `unknown` | The source does not expose enough detail to determine the result. |

The monitor is local-first. It must not collect prompts, responses, secrets, tool arguments, source files, or provider credentials by default. It records metadata, hashes, status, and redacted raw-source references only.

## Model-neutral authority profiles

Profiles belong to Tokenomics, not to a model or provider. Routing chooses a model and effort *after* the task has selected a profile. Profile names must describe authority and complexity, never a model.

| Profile | Intended work | Source authority |
|---|---|---|
| `analyst.low` | repository orientation, log triage, simple hypothesis | read/search only; no network or writes |
| `analyst.medium` | dependency tracing, reproduction design, test plan | read-only; bounded diagnostic commands if available |
| `analyst.high` | difficult root-cause analysis and fix specification | read-only; isolated tests if available; no source changes |
| `runner.low` | one known test, linter, formatter check, or reproduction | read-only source; one allowlisted command; temporary output only |
| `runner.medium` | bounded suite, benchmark, or reproduction harness | read-only source; allowlisted command family; CPU/time/output caps |
| `implementer.low` | small scoped code change | writes only in the task worktree and assigned files; tests allowed |
| `implementer.high` | complex implementation | task worktree; diff/file budget; verifier and review required |
| `reviewer` | inspect diff, acceptance evidence, and policy compliance | read-only; may reject/request rework; no edits |
| `release` | prepare a human release decision | no merge, publish, deploy, payment, or external-write authority |

`analyst.high` returns evidence, ranked hypotheses, an implementation specification, risks, and an exact verifier command. It hands this to an implementer; it never applies the proposed fix itself. `runner.*` may write only its ephemeral test output/cache directory, never the source checkout.

Every dispatched task records an authority grant separately from routing:

```yaml
authority_grant:
  task_id: bug_184
  profile: analyst.high
  filesystem: read_only
  writable_roots: []
  network: denied
  allowed_actions: [read, search, diagnostic_test]
  denied_actions: [source_write, git_commit, git_push, dependency_install, deploy, publish, credential_access]
  max_tool_calls: 30
  max_wall_ms: 1800000
  approval_required_for: [any_side_effect]
route:                         # orchestrator-only information
  provider: exact_provider
  model: exact_id
  effort: exact_setting
```

Workers and reviewers receive the profile and task contract, but not competing model identities, prices, quota, or routing rationale.

## What to record for every attempt

Record the required ledger fields in `measurement.md`, plus these authority events where a source exposes them:

```text
authority_granted, session_started, model_segment_started,
tool_requested, policy_evaluated, tool_allowed, tool_denied,
approval_requested, approval_granted, approval_rejected,
tool_completed, policy_violation_detected, agent_interrupted,
session_finished, verifier_finished, accepted
```

An authority event includes `task_id`, `session_id`, `agent_id`, `profile`, `requested_action`, `decision`, `enforcement_state`, `policy_rule`, timestamp, and a redacted `raw_source_ref`. Keep the actual model on the route/model segment, not in the profile name.

Progress is evidence, not chat volume. Count a meaningful checkpoint only if an artifact hash changes, a test/verifier result changes, an external result arrives, or the task changes state. Repeated assistant messages and repeated commands are not progress.

## Current setup: native CLI runbook

Use a per-version capability matrix. `unknown` means no claim, not zero.

| CLI | Enforce now | Observe now | Important limits |
|---|---|---|---|
| Codex CLI | `--sandbox read-only` for analysts; `workspace-write` only for task worktrees; approval policy and command-prefix rules where configured | session lifecycle; local agent/app-server discovery after version probe; workspace/test evidence | Native token/credit/quota feed is not a documented stable source in this policy. Treat it as `unknown` until validated. |
| GitHub Copilot CLI | its permission/tool/path/URL controls when launched with a bounded session configuration; do not use broad `--allow-all`/`--yolo` for constrained profiles | local OTel JSONL: model/tool spans, durations, token fields where emitted, errors and subagent trace links; `/context`, `/usage`, `/statusline` snapshots | An existing session may have broader prior permissions. OTel is telemetry, not a sandbox. Capability and exact policy behavior must be pinned to CLI version. |
| AGY | use its sandbox option and do not use `--dangerously-skip-permissions`; isolate worktree/source externally when writes are allowed | selected model/effort, JSON/JSONL print output, configured local log file, available-agent listing | Installed CLI help does not establish quota/context/subagent telemetry. Mark those fields `unknown`; log schema is uncontracted. |
| Claude Code | discover and version-test its local permission/session controls before assigning an enforced profile | status/session export/log sources only after discovery | It is not installed on this machine at the time of this policy. All current capabilities are `unknown`. |

### Codex launches

An analyst is launched with a technical read-only boundary, not merely a prompt:

```sh
codex --model <route-selected-model> --sandbox read-only \
  --ask-for-approval on-request
```

An implementer runs from a dedicated task worktree with `--sandbox workspace-write`; it does not receive a writable main checkout. Use the narrowest writable roots and approval scope. Codex sandboxing applies to spawned commands such as Git and package managers as well as direct file operations. See the [official sandbox documentation](https://learn.chatgpt.com/docs/sandboxing).

### Copilot launches and observation

Start an opted-in local metadata telemetry file for each monitored session:

```sh
COPILOT_OTEL_FILE_EXPORTER_PATH="$TOKENOMICS_SESSION_DIR/copilot-otel.jsonl" \
copilot --model <route-selected-model>
```

Use the CLI's least-privilege permission configuration; never add `--allow-all`, `--allow-all-tools`, `--allow-all-paths`, or `--yolo` to an analyst or runner. Save `/context`, `/usage`, and `/statusline` as redacted manual snapshots when needed. OTel message-content capture remains off: metadata-only collection is the default.

### AGY and Claude Code

For AGY, set a task-scoped `--log-file`, use `--sandbox`, and record model, effort, project/conversation, start/end, workspace diff, and verifier result. Structured print-mode runs may use `--output-format json` or `stream-json`. Do not claim it enforces command allowlists or exposes credits/context unless a versioned fixture demonstrates that fact.

For Claude Code, do not create a nominal adapter now. First capture `--version`, `--help`, its permissions/status output, and one redacted session fixture; then write a version-specific capability entry.

## Local monitoring loop without a harness

The operator starts the CLI with the intended profile, records the authority grant, and attaches the task to a workspace. A local watcher then collects:

```text
CLI telemetry/log/status snapshots
workspace diff and status
verifier/test/build outcome
task state and human approvals
```

Check `git status --porcelain`, `git diff --numstat`, artifact hashes, and the declared verifier at checkpoints. A runner should operate on a read-only source copy/mount with task-scoped temporary directories for caches and test output. An implementer receives a separate worktree. Never use the main checkout as an unbounded agent scratchpad.

Alert, but do not automatically kill, when a repeated normalized tool command has no file/test/artifact state change, when an analyst attempts a side effect, or when tool/wall/credit limits are exceeded. An interrupt requires an explicit local control event and preserves the partial diff and received logs.

## Reporting and acceptance

The live view shows profile, enforcement state, task state, model segment, last meaningful evidence, children active/blocked, context/quota if actually exposed, and policy denials. Example:

```text
analyst.high · enforced read-only · 14 reads · 2 isolated test runs · 0 writes
last evidence: expiry fixture reproduces failure · handoff ready · 9m remaining
```

The task-tree roll-up charges every child, test, review, retry, and human correction once to the accepted parent artifact. Split attributed and unattributed credits; never convert hidden plan usage or context capacity into invented cash or token quantities.
