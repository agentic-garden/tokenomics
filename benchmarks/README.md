# Benchmark evidence index

This directory keeps **one auditable dossier per benchmark source**. It is the
evidence layer behind `../benchmarking.md`; it is not a universal model ranking.
Scores, costs, token counts and elapsed times are comparable only inside the
same benchmark version, harness, effort setting and grading method.

## Use this index before routing a department task

1. Classify the work by artifact and failure mode, not by the department name
   alone: code patch, factual brief, spreadsheet model, cross-app workflow,
   policy draft, creative asset, or employment decision.
2. Load only the dossier whose task environment resembles that artifact.
3. Treat the resulting models as a **candidate list**. Use the local A/B
   protocol in `../benchmarking.md` before promoting a default.
4. Keep model/provider identity, price and quota out of worker prompts. The
   department orchestrator owns route selection and the acceptance gate.

## Evidence map by department

| Department / work | Closest evidence | What it actually supports | Material gap / required guardrail |
|---|---|---|---|
| Engineering, DevOps, security | [Terminal-Bench](terminal-bench.md), [DeepSWE](deep-swe.md) | Terminal execution and verified repository patches | Local repo, CI, rollback and security checks remain decisive. |
| Finance | [APEX](apex-agents.md), [AutomationBench](automationbench.md), [Omniscience](aa-omniscience.md) | Banking-style document work, finance SaaS workflows, business/economics knowledge | No authority to book, pay, trade, file, or certify. Recalculate figures deterministically and cite primary sources. |
| Marketing | [AutomationBench](automationbench.md), [APEX](apex-agents.md), [Omniscience](aa-omniscience.md) | CRM/content-operation workflows, consulting-style synthesis, business/humanities knowledge | No direct benchmark for creative quality, brand fit, audience response, image/video generation, or campaign lift. Run local creative and conversion evaluations. |
| Operations | [AutomationBench](automationbench.md), [Terminal-Bench](terminal-bench.md) | Cross-SaaS execution with guardrails; operational terminal tasks | Sandbox side effects; require owner approval for production changes. |
| HR | [AutomationBench](automationbench.md) | Scheduling/reminder and policy-constrained application workflows | It is not evidence for hiring judgment, performance review, compensation, or legal compliance. |
| Talent management / recruiting | No qualifying benchmark among these five | At most, HR workflow assistance and factual/source drafting | Never automate candidate ranking, rejection, selection, performance judgment, or employment action. Human accountable owner and jurisdiction-specific process are mandatory. |
| Research / strategy / knowledge base | [Omniscience](aa-omniscience.md), [APEX](apex-agents.md) | Calibration-aware background knowledge and document-heavy professional work | Omniscience is not browsing. Require current primary-source retrieval, citations, and independent challenge. |

## Dossiers

| Source | Primary task family | Department relevance |
|---|---|---|
| [Terminal-Bench v4.0](terminal-bench.md) | Verified terminal agents | Engineering and technical operations |
| [APEX Agents](apex-agents.md) | Professional-services work in full applications | Finance, strategy, legal/document work |
| [AutomationBench-AA](automationbench.md) | Guardrailed cross-application automation | Finance, HR, marketing, operations, sales, support |
| [AA-Omniscience](aa-omniscience.md) | Knowledge plus calibration | Research and domain-aware synthesis |
| [DeepSWE v1.1](deep-swe.md) | Long-horizon repository change | Engineering |

Exact retrieved URL inventory: [crawl-manifest-2026-09-27.md](crawl-manifest-2026-09-27.md).

## What is deliberately absent

These five sources do not establish a winner for creative production, paid-media
optimization, video/reel creation, accounting close, payroll, talent selection,
negotiation, legal advice, or live production authority. Do not turn a nearby
score into evidence for those jobs. Add a department-owned local evaluation
packet: task contract, source data, deterministic checks where possible, human
acceptance rubric, correction time, elapsed time, and allowed side effects.

## Active shortlist — test locally, do not treat as a universal ranking

| Route role | Candidate models | Why they remain in the fleet |
|---|---|---|
| Hard technical lead | GPT-6 Astra; Claude Opus 5.5 | Current Terminal-Bench/DeepSWE and high-tier synthesis candidates. |
| High-quality independent synthesis/review | Claude Opus 5.5; Claude Fable 5.1 | APEX/Omniscience candidates for difficult evidence and long-horizon work. |
| Main substantial worker | GPT-5.6 Sol | Near-leading repository implementation candidate with a less extreme cost/step profile than the measured Opus 5 configuration. |
| Cheap bounded coding worker | Gemini 3.8 Flash | Strong DeepSWE pass signal, but constrain scope/output/tool loops. |
| Cheap verified worker | GPT-5.6 Luna | Use only where deterministic verification makes low cash more valuable than peak acceptance. |
| Sandboxed automation challenger | DeepSeek V4.1 Flash | Near the AutomationBench leaders; never grants production side-effect authority. |
| Periodic API/local challengers | GLM-5.3; Kimi K3 | Keep only if they win a representative local A/B on accepted work. |

Claude Sonnet 5 is not an economic default from the captured DeepSWE run; it
remains a project-specific challenger. See each dossier and the crawl manifest
for the source links used at the next monthly refresh.

## Refresh discipline

Snapshot each dossier on the first business day of every month, and again after
a material benchmark/model/harness change. Rankings and benchmark price cards
are watch data; verify API and plan prices separately every seven days. Each
refresh records the page URL, version/date, model identifier, effort, harness,
score, uncertainty, published token/cost/time fields, and what changed. Never
fill an unavailable per-domain result with an overall score.

Supplementary watchlist, not results blended into these five: Artificial
Analysis lists AA-Briefcase (business deliverables), GDPval (occupations),
AA-AnalystAgent (analysis) and EnterpriseOpsGym (enterprise operations) in its
[methodology](https://artificialanalysis.ai/methodology/intelligence-benchmarking).
Evaluate them as separate sources when a department needs that coverage.
