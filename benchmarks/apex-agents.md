# APEX Agents dossier

Source: [Mercor — APEX Agents Leaderboard](https://www.mercor.com/apex/apex-agents-leaderboard/)  
Companion paper: [APEX-Agents](https://arxiv.org/abs/2601.14242)  
Verified: 2026-09-27 · Rankings: refresh monthly

## What it evaluates

APEX evaluates agents completing professional work inside data-rich application
worlds. The current leaderboard describes 31 worlds and 240 tasks; its three
job roles are **investment-banking analyst, management consultant, and
corporate lawyer**. Tasks are graded against expert-authored rubrics with an LM
judge. Mean Score is the average percentage of criteria passed; Pass@1 requires
the whole rubric to pass. Neither is equivalent to a regulatory, employment,
financial-advice, or production-operational guarantee.

This is the strongest one of the five sources for document-heavy professional
synthesis. It is not a coding or terminal benchmark, and it is not a generic
"business model" leaderboard.

## Current signal captured 2026-09-27

Overall Mean Score: Claude Opus 5.5 Max **73.5 ± 4.9**; Claude Fable 5.1 Max
**68.6 ± 4.9**; Gemini 3.7 Flash High **67.8 ± 5.1**. The uncertainty ranges
matter: close rankings are challenger evidence, not automatic route changes.

The available role views are more actionable than the overall mean:

| Role view | Current leading signal | Tokenomics reading |
|---|---|---|
| Corporate lawyer | GPT-6 Astra 73.4; Fable 71.9; Opus 71.2 | Use as a drafting/synthesis challenger only; no legal advice or filing authority. |
| Management consultant | Opus 80.0; Gemini 3.8 Flash 77.5; Fable 71.6 | Flash is a serious structured-analysis challenger; high-tier lead still needs local quality/time test. |
| Investment banking | Gemini 3.7 Flash 71.3; Opus 5.5 69.3; Opus 5 66.6 | Cheap/fast routes may be viable for bounded analysis, with deterministic arithmetic and source checks. |

## Department use and limits

| Department | What APEX contributes | What it does not establish |
|---|---|---|
| Finance | Analytic narratives, diligence-style documents, banking-workflow candidate selection | Correct valuation, financial reporting, trading, payment, tax, or fiduciary decision. Use calculators/spreadsheets and human sign-off. |
| Marketing | Strategy/positioning briefs and executive synthesis proxy | Brand voice, creative quality, image/video output, audience fit, conversion or campaign lift. |
| Operations | Planning memos and cross-functional recommendations | Correct live system changes or cross-SaaS execution; use AutomationBench/local sandbox evidence. |
| HR/talent | Very little direct evidence | Hiring, ranking, promotion, performance or compensation judgment. Do not automate those outcomes. |
| Legal | Draft/research organization proxy | Legal correctness, jurisdictional compliance or authorization. |

## Tokenomics interpretation

For an executive brief, financial narrative, market map or strategy packet,
shortlist the role-view winner and a cost challenger. Run a blinded local test
with a source pack and acceptance rubric covering factual traceability,
arithmetic, decision usefulness, clarity, omissions, policy compliance, human
correction minutes and time-to-accepted. A model with lower mean score may win
your task if it reaches an accepted brief with far fewer review cycles.

Do not give a worker a prompt like “beat the Opus agent.” Give it the artifact,
sources, constraints and acceptance tests. A separate reviewer gets the same
rubric and artifact, not producer identity/cost/quota.

## Gaps and refresh fields

No task role maps directly to marketing production, people operations, talent
selection, accounting close, customer support quality or production automation.
Record `role, world/task type, model ID, effort, mean score, Pass@1, uncertainty,
tools, task completion limits, source date` on refresh. Preserve the
leaderboard's current task/world count if it changes; do not combine it with
AutomationBench or Omniscience scores.
