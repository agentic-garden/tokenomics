# AA-Omniscience dossier

Source: [Artificial Analysis — AA-Omniscience](https://artificialanalysis.ai/evaluations/omniscience)  
Methodology: [Artificial Analysis intelligence benchmarking](https://artificialanalysis.ai/methodology/intelligence-benchmarking)  
Verified: 2026-09-27 · Rankings: refresh monthly

## What it evaluates

AA-Omniscience contains 6,000 questions across 42 topics. Its top-level domains
are Business; Humanities & Social Sciences; Health; Law; Software Engineering;
and Science, Engineering & Mathematics. The calibration-aware index ranges from
-100 to 100: correct answers help, hallucinated wrong answers are penalized, and
abstention is neutral. The benchmark is therefore a useful signal for embedded
knowledge and calibrated uncertainty.

It is **not** a live-web research benchmark, a citation-quality benchmark, a
tool-use benchmark, a code-execution benchmark or an authorization test. A high
Omniscience result never replaces retrieval of current primary sources.

## Current signal captured 2026-09-27

The overall index currently lists Claude Opus 5.5 Max at **46**, GPT-6 Astra
High at **44**, and Claude Fable 5.1 Max at **43**. Fable is listed at 67%
accuracy and Opus at 66% in the current snapshot. This is calibration-aware
knowledge evidence, not an instruction to route every business task to Opus.

## How to use its domains correctly

“Business” may include economics/finance-tagged material, but it is not proof of
competence at any particular financial workflow. Do not claim a Finance, HR or
Marketing leaderboard unless the source exposes that exact domain/model/result.

| Department / project type | Relevant source domain | Valid use | Invalid leap |
|---|---|---|---|
| Finance research, market/economic brief | Business, potentially finance/economics tags | Choose a synthesis/research challenger; require source citations and deterministic math | Trading, valuation sign-off, accounting close, tax or payment authority |
| Marketing research/positioning | Business plus humanities/social sciences | Candidate for evidence organization and uncertainty-aware claim drafting | Creative quality, audience resonance, conversion, image/video/reel generation |
| Operations policy/explainer | Business, STEM, software engineering | Candidate for scoped technical/process explanation | Cross-SaaS execution, deployment or incident command |
| HR/talent policy research | Business/humanities only as a weak proxy | Source summarization with legal/policy review | Candidate scoring, employment outcome or fairness certification |
| Engineering design/research | Software engineering and STEM | Background knowledge challenger before a verified implementation route | Actual patch success, security correctness or runtime performance |

## Project evidence packet

For a department orchestrator, use the specific domain view as one field in a
project packet, never the overall index alone:

```text
artifact + required current sources + relevant Omniscience domain/topic result
+ calibration/abstention behavior + local source-traceability test
+ independent challenge + human correction/time + side-effect class
= routing evidence
```

The producing agent receives the artifact, allowed sources and acceptance
criteria; it does not receive competitor model identity, score, cost or quota.
The reviewer uses a different lens: factual/source traceability, arithmetic,
assumption/uncertainty, policy/authority, or decision usefulness.

## Department gaps worth measuring locally

There is no direct Omniscience measure for creative marketing output, campaign
performance, finance operations, HR process quality, talent evaluation,
organizational fairness, or real-world execution. Build small recurring packets
with redacted representative work and source-backed rubrics. For people-related
work, keep a human accountable owner and prohibit automated employment actions.

## Refresh fields

`benchmark date/version | model ID | effort | overall index | accuracy | relevant
domain/topic score if exposed | grading model/method | cost/token fields if exposed |
source URL | local relevance note`. Do not manufacture a missing domain score.
