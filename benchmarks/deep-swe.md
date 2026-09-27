# DeepSWE v1.1 dossier

Source: [DeepSWE v1.1 release](https://deepswe.datacurve.ai/blog/deepswe-v1-1)  
Repository/method: [datacurve-ai/deep-swe](https://github.com/datacurve-ai/deep-swe)  
Verified: 2026-09-27 · Rankings/prices: refresh monthly

## What it evaluates

DeepSWE v1.1 evaluates long-horizon repository change: 113 tasks from active
open-source projects across TypeScript, Go, Python, JavaScript and Rust. A task
contains an instruction, isolated environment, hidden behavioral tests and a
held-out reference solution. The agent produces a patch; the patch is applied
to a pristine container and evaluated. The suite reports structured reward,
test results and execution logs.

It is strong evidence for engineering implementation/repair economics because
it exposes pass rate, average cost, output tokens and steps. It intentionally
does **not** report wall-clock time: host/provider load makes that comparison
inconsistent. Do not invent a speed ranking from its step count.

## Current v1.1 evidence captured 2026-09-27

| Configuration | Pass | Eval cost/task | Output tokens/task | Steps/task |
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

The defensible comparison is Astra vs the measured Opus 5 configuration: same
74% pass rate and about **2.7x lower** listed evaluation cost ($4.43 vs $11.84).
There is no Opus 5.5 row here; do not extend that claim to Opus 5.5. Gemini
Flash's equal pass rate comes with 4.8x Astra's output and 5.7x steps. That is
a reason to bound it as a scout, not evidence that it is a bad or slow model in
every environment. Luna/GLM are valuable low-cash challengers only when a
verifier catches failure cheaply.

## Department interpretation

| Work | Relevance |
|---|---|
| Product/platform engineering | Direct candidate evidence for implementation and repair in the covered languages. |
| Data/technical operations | Indirect: use only where the artifact is a tested repository patch. |
| Finance, marketing, HR, talent | No direct model-selection evidence. Their tools may contain code, but use the department's own task evidence for the resulting business artifact. |

## Tokenomics routing and A/B

For a real code task, select one high acceptance candidate and one economical
challenger. Match repository snapshot, compiler/tool access, context cap,
timeout, effort and independent tests. Record patch acceptance, test outcome,
security/rollback findings, plan-capacity delta or actual API cash, separated
tokens if exposed, retries, human correction and end-to-end elapsed time.

The benchmark's cost/task is an API/harness observation. It does not tell us
whether a Codex/Copilot/Claude subscription has remaining capacity or what an
overage costs. Use `../economics.md` and `../capacity.md` for that separate
decision.

## Refresh fields

`version/date | repository/language slice | model/effort | pass | cost/task |
output tokens | steps | pricing snapshot/corrections | verifier/container policy |
source URL | local comparison status`.
