# Capacity, context, and local-memory policy

Load only for quota, rate-limit, context, or local-memory questions.

Subscription quotas are weighted capacity, not token grants. Capture CLI/account status before and after meaningful work; use capacity deltas and accepted outcomes where token counts are hidden.

Observed Codex fact for this account on 2026-09-27: a 95%-used five-hour window corresponded to 86% weekly remaining, about 14 percentage points of weekly capacity. Seven equivalent windows consume about 98%; a rolling five-hour reset enables a burst, not unlimited daily capacity. Re-measure after a material plan/model or usage-mode change.

## Context admission

    P90 demand = CLI/tool overhead + instructions + brief + selected evidence
               + expected tool output + reserved generation + safety margin

Record model maximum context, client limit, configured/resident context, reserved output, and observed compaction. Prefer the smallest safe context; smaller is not cheaper if it forces retrieval/retry/escalation.

KV cache grows with architecture, precision, resident tokens, and active sequences. Weight quantization does not guarantee KV quantization; MoE/RAM offload does not remove context pressure. Measure runtime/model/context/concurrency combinations.

On the RTX 4090, prefer fully GPU-resident interactive models with KV headroom. On the M1 Max, reserve unified memory for macOS/runtime and never treat swap as usable capacity. Batch work yields to interactive work. API project quotas do not prove native CLI subscription capacity.
