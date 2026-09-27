# Tokenomics — compact orchestration policy

Tokenomics answers: **what is the cheapest route that can produce an accepted result by the deadline?** It selects model tier, tool class, skill, review level, and budget policy. It does not implement terminal supervision, event buses, sandboxes, or a multi-agent harness.

## Always-load policy

Load this file first; load no other Tokenomics module unless its trigger below applies.

1. Preserve quality, data, authority, and deadline constraints before optimizing cost.
2. Use existing authenticated native lanes first: Codex for GPT work, AGY for Gemini, Copilot for non-GPT/non-Gemini burst work. Use local only for qualified bounded/private work. Use API/rental only when explicitly allowed.
3. Price the complete accepted parent deliverable: tools, retries, review, human correction, and elapsed time—not one request.
4. Mechanical work is deterministic-tool first, then validated local/Luna. New interpretation or consequential judgment uses the lead route.
5. An artifact is not authority to purchase, publish, post, deploy, pay, file, sign, enforce, or make a personnel decision.
6. Hide execution-route model/provider/cost/quota from workers. Keep provider names and prices visible when they are the research subject.

## Route input

    capability + artifact + risk + deadline + budget + data class + permitted side effects

## Conditional loading

| Need | Load |
|---|---|
| Select model/tool/skill/review | routing.md |
| Compare plan/API/overage/local/rental cost | economics.md |
| Check quota, rate limit, context, local memory | capacity.md |
| Map a team/agent/skill to a capability/artifact | capabilities.md |
| Measure routes, renew/cancel a plan | measurement.md |
| Re-rank models using quality, tokens, cost, and time | benchmarking.md |
| Select benchmark evidence for a department or project artifact | benchmarks/README.md |
| Produce a dispatch contract | task-contract.template.yaml |

## Stop rule

Escalate once only when a defined quality gate fails. At each meaningful checkpoint record: goal, thesis, anti-thesis, evidence, assumptions, cheapest test, spend used, next reversible action, and stop condition. Confirmation that reduces uncertainty is progress. Stop/decompose/park when successive checkpoints add no relevant evidence, resolve no assumption, and complete no acceptance condition.

## Freshness

Pricing, catalogue, plan, quota, and promotion facts expire seven days after verification. Record provider, claim, source URL, verified date, expiry, geography/account scope, and confidence. Never invent subscription-token equivalents.
