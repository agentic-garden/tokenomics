# Benchmark crawl manifest — 2026-09-27

This is the exact public-source crawl used for the benchmark dossiers. Every
URL below returned HTTP 200 and its response body was retrieved. It is a
**selected source lineage crawl**, not a claim that every link in the global
navigation of each website was read.

Important: URL fragments (`#section`) are client-side anchors. They identify
tables/sections inspected within the same underlying page; they are not separate
HTTP documents. Artificial Analysis `?knowledge=` URLs are rendered filtered
views of its evaluations application. They are recorded to make the scope
auditable, not counted as independent benchmarks.

## 1. Terminal-Bench v4.0

### Artificial Analysis evaluation and inspected sections

- https://artificialanalysis.ai/evaluations/terminalbench-4-0
- https://artificialanalysis.ai/evaluations/terminalbench-4-0#results
- https://artificialanalysis.ai/evaluations/terminalbench-4-0#cost
- https://artificialanalysis.ai/evaluations/terminalbench-4-0#token-usage
- https://artificialanalysis.ai/evaluations/terminalbench-4-0#speed
- https://artificialanalysis.ai/evaluations/terminalbench-4-0#example-tasks

### Linked benchmark/methodology lineage

- https://www.tbench.ai/leaderboard/terminal-bench/4.0
- https://github.com/laude-institute/terminal-bench
- https://github.com/harbor-framework/terminal-bench-1
- https://hub.harborframework.com/datasets/terminal-bench/terminal-bench/4?tab=tasks
- https://arxiv.org/abs/2601.11868

## 2. APEX Agents

### Mercor leaderboard and role views

- https://www.mercor.com/apex/apex-agents-leaderboard/
- https://www.mercor.com/apex/apex-agents-leaderboard/?pass=mean-score
- https://www.mercor.com/apex/apex-agents-leaderboard/?pass=pass-1
- https://www.mercor.com/apex/apex-agents-leaderboard/investment-banking-analyst-agent/
- https://www.mercor.com/apex/apex-agents-leaderboard/management-consultant-agent/
- https://www.mercor.com/apex/apex-agents-leaderboard/corporate-lawyer-agent/

### Closely related Mercor pages and public artifacts

- https://www.mercor.com/apex/apex-accounting-leaderboard/
- https://www.mercor.com/apex/apex-swe-leaderboard/
- https://www.mercor.com/blog/introducing-apex-agents-1-1
- https://huggingface.co/datasets/mercor/apex-agents-v1.1
- https://github.com/Mercor-Intelligence/apex_loop_truncated_tools_agent
- https://arxiv.org/abs/2601.14242

The Accounting and SWE pages are recorded as adjacent evidence, not blended into
the APEX Agents professional-services score.

## 3. AutomationBench-AA

### Artificial Analysis evaluation and inspected sections

- https://artificialanalysis.ai/evaluations/automationbench-aa
- https://artificialanalysis.ai/evaluations/automationbench-aa#score
- https://artificialanalysis.ai/evaluations/automationbench-aa#domain-breakdown
- https://artificialanalysis.ai/evaluations/automationbench-aa#app-breakdown
- https://artificialanalysis.ai/evaluations/automationbench-aa#task-breakdown
- https://artificialanalysis.ai/evaluations/automationbench-aa#objectives-completion
- https://artificialanalysis.ai/evaluations/automationbench-aa#violations
- https://artificialanalysis.ai/evaluations/automationbench-aa#cost
- https://artificialanalysis.ai/evaluations/automationbench-aa#token-usage

### Linked benchmark/methodology lineage

- https://zapier.com/benchmarks
- https://arxiv.org/abs/2604.18934
- https://github.com/zapier/AutomationBench
- https://github.com/zapier/AutomationBench/blob/main/README.md
- https://github.com/zapier/AutomationBench/tree/main/automationbench
- https://github.com/zapier/AutomationBench/tree/main/visualizer

## 4. AA-Omniscience

### Artificial Analysis evaluation and inspected sections

- https://artificialanalysis.ai/evaluations/omniscience
- https://artificialanalysis.ai/evaluations/omniscience#omniscience-index
- https://artificialanalysis.ai/evaluations/omniscience#detailed-domain-results
- https://artificialanalysis.ai/evaluations/omniscience#aa-omniscience-accuracy
- https://artificialanalysis.ai/evaluations/omniscience#aa-omniscience-hallucination-rate
- https://artificialanalysis.ai/evaluations/omniscience#software-engineering-deep-dive
- https://artificialanalysis.ai/evaluations/omniscience#cost
- https://artificialanalysis.ai/evaluations/omniscience#token-usage

### Filter views and methodology/data lineage

- https://artificialanalysis.ai/evaluations?knowledge=business
- https://artificialanalysis.ai/evaluations?knowledge=finance
- https://artificialanalysis.ai/evaluations?knowledge=humanities
- https://artificialanalysis.ai/evaluations?knowledge=legal
- https://artificialanalysis.ai/evaluations?knowledge=medical
- https://artificialanalysis.ai/evaluations?knowledge=science
- https://artificialanalysis.ai/methodology/intelligence-benchmarking
- https://huggingface.co/datasets/ArtificialAnalysis/AA-Omniscience-Public
- https://arxiv.org/abs/2511.13029

## 5. DeepSWE v1.1

### DataCurve site

- https://deepswe.datacurve.ai/blog/deepswe-v1-1
- https://deepswe.datacurve.ai/

### Repository and execution-environment lineage

- https://github.com/datacurve-ai/deep-swe
- https://github.com/datacurve-ai/deep-swe/blob/main/README.md
- https://github.com/datacurve-ai/deep-swe/blob/main/PROVENANCE.md
- https://github.com/datacurve-ai/deep-swe/tree/main/tasks
- https://github.com/datacurve-ai/pier

## Evidence rule after this crawl

Only use a number where its source view actually supplies that number. The
manifest does not turn an inspected overall leaderboard into a verified
department/model result. Department routing remains subject to the task-specific
local A/B and acceptance protocol in `../benchmarking.md`.
