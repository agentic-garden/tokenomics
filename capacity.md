# Capacity, context, and local-memory policy

Load only for quota, rate-limit, context, or local-memory questions.

Subscription quotas are weighted capacity, not token grants. Capture CLI/account status before and after meaningful work; use capacity deltas and accepted outcomes where token counts are hidden.

Observed Codex fact for this account on 2026-09-27: a 95%-used five-hour window corresponded to 86% weekly remaining, about 14 percentage points of weekly capacity. Seven equivalent windows consume about 98%; a rolling five-hour reset enables a burst, not unlimited daily capacity. Re-measure after a material plan/model or usage-mode change.

## Context admission

    P90 demand = CLI/tool overhead + instructions + brief + selected evidence
               + expected tool output + reserved generation + safety margin

Record model maximum context, client limit, configured/resident context, reserved output, and observed compaction. Prefer the smallest safe context; smaller is not cheaper if it forces retrieval/retry/escalation.

## Context-cost admission

Context size becomes a separate economic route only where the provider changes
the bill. The current practical split is:

| Route | Capacity fact | Cost action |
|---|---|---|
| OpenAI GPT-6 Astra / Sol / Luna API | 1,050K maximum context; Astra maximum output 128K | At more than 272K input tokens, route as `long`: 2× input/cache and 1.5× output for the whole request. Otherwise use `short`. |
| Gemini 3.8 Flash API | 1M context, 64K maximum output | No published long-context surcharge. Minimize context for TTFT, failure blast radius and quota—not an invented higher token rate. |
| Claude 5 family API | 1M context / up to 128K output where offered | No current Claude-5 long-context price schedule is recorded. Do not apply older Sonnet-4 200K pricing. |
| DeepSeek V4.1 Flash API | 1M context | No documented context-rate bracket in the current evidence. |
| Kimi K3 API | up to 256K in current official help | Billing is not segmented by context length. |

Reasoning tokens also occupy model context, but OpenAI's 272K price threshold
is based on input plus cache reads/writes—not generated reasoning. A high-effort
request can still reduce available context and increase output billing without
crossing that input-price threshold. Full source links and service-tier pricing
are maintained in `economics.md` and must be refreshed within seven days.

KV cache grows with architecture, precision, resident tokens, and active sequences. Weight quantization does not guarantee KV quantization; MoE/RAM offload does not remove context pressure. Measure runtime/model/context/concurrency combinations.

On the RTX 4090, prefer fully GPU-resident interactive models with KV headroom. On the M1 Max, reserve unified memory for macOS/runtime and never treat swap as usable capacity. Batch work yields to interactive work. API project quotas do not prove native CLI subscription capacity.
