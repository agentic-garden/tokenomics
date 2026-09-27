# AutomationBench-AA dossier

Source: [Artificial Analysis — AutomationBench-AA](https://artificialanalysis.ai/evaluations/automationbench-aa)  
Verified: 2026-09-27 · Rankings: refresh monthly · Side effects: always local-policy gated

## What it evaluates

AutomationBench-AA has 657 tasks across **Finance, HR, Marketing, Operations,
Sales and Support**. Agents use REST APIs for applications including Gmail,
Google Sheets, Slack, Salesforce, Zendesk, Jira and HubSpot. Tasks deliberately
include cross-app discovery, layered policies and misleading records.

AA's key score is the average share of objectives completed **without a
guardrail violation**; a task with a guardrail break scores zero. This makes it
the direct evidence source among the five for department automation—but not a
license to perform side effects in our accounts.

## Current overall signal captured 2026-09-27

| Configuration | AA score | Objective completion | Decision use |
|---|---:|---:|---|
| Claude Opus 5.5 Max | 69.5% | 90% | High-tier planning/review candidate for consequential workflows |
| DeepSeek V4.1 Flash Max | 68.9% | Not carried as a general claim | Cheap sandbox automation challenger |
| GPT-6 Astra Max | 68.5% | 89% | High-tier alternative challenger |

The near tie makes this a clear local A/B situation. Do not declare a
per-department winner from the combined score: the accessible source snapshot
establishes task coverage by domain but does not provide a reliable numeric
model winner for every domain/app combination here.

## Department interpretation

| Department | Representative benchmark behavior | Default route implication | Non-negotiable gate |
|---|---|---|---|
| Finance | Sheets/Drive/Gmail tasks such as grant-expense allocation, exact-value carryover and overspend flags | Cheap model can be a sandbox executor if deterministic reconciliation catches failures; high-tier model can plan/review exceptions | No payment, trade, ledger posting, filing or approval without accountable human authority. |
| HR | Workflow tasks such as interview reminders constrained by candidate opt-in and notes | Use for drafts, scheduling and reminders with explicit data minimization | Do not use for screening, ranking, rejection, selection, performance, compensation or other employment decisions. |
| Marketing | CRM, messaging and cross-app operations | Use to test campaign ops/scheduled drafts and source-to-CRM workflows | Brand/claims review; no publishing or paid-spend action without owner approval. |
| Operations | Jira/Slack/Gmail/Sheets operational coordination | Strong source for sandboxed, permission-scoped execution candidates | Dry run, idempotency, rollback and change-owner checkpoint. |
| Sales/support | CRM/ticket workflow | Use as a candidate pre-filter for triage/drafts | Customer-impacting messages and data changes require approval policy. |

## Tokenomics routing pattern

1. Classify action as `read`, `draft`, `propose`, `execute sandbox`, or
   `execute production`. Only the first four belong in a model test.
2. Give a budget challenger a narrow task, allowlisted tools, structured schema,
   maximum calls, idempotency key and deterministic end-state verifier.
3. Give an independent high-tier reviewer a different lens: permission/policy,
   missing-data, reconciliation/rollback, or user impact. It does not receive
   the executor model identity.
4. Compare accepted workflow outcomes—not request cost—across cash, scarce plan
   capacity, retries, policy violations, review minutes and elapsed time.
5. Production remains an explicit owner action even after a route wins a test.

## Important limits

A low violation rate can result from an agent taking few actions. It is not
enough; inspect completed objectives and required action coverage together.
AutomationBench does not evaluate local credentials, organization-specific RBAC,
privacy law, retention, union/works-council obligations, bias, brand risk, or
real customer consequences.

## Refresh fields

`benchmark version | task count | domain/app slice | model/effort | AA score |
objective completion | violation definition | cost/task | input/cache/reasoning/output tokens |
time if published | tool permissions | source URL | notes`
