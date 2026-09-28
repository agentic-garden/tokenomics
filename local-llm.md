# Local LLM: parsing, speed, and hardware

Load for local-model selection, local-vs-cloud routing, parsing throughput, or
local hardware decisions. It is a **measurement policy**, not a promise that an
open-weight model equals a cloud frontier model.

## Decision now

The current local parsing default is the RTX 4090 machine, not the M1 Max:

| Asset | Known configuration | Default role | Do not use as |
|---|---|---|---|
| MacBook Pro M1 Max | 64 GB unified memory; Bielik-11B observed at 13–27 decode tok/s in Ollama | portable/private fallback; Apple-runtime test host; batch work that may take longer | primary low-latency parser |
| Windows PC | RTX 4090 24 GB, 64 GB RAM, i9-13900K, water-cooled | primary interactive local inference and bounded parsing | host for models that spill weights/KV cache to system RAM |

`13–27 tok/s` describes **decode**, not transcript/log ingestion. It may be
fine for a short JSON answer but is not evidence that a route can ingest a long
SRT cheaply or quickly. Record prefill/input throughput independently.

No hardware purchase is justified before the 4090 has been benchmarked with a
fully GPU-resident small multilingual model and a suitable runtime. An 11B
Polish model on a Mac is an accuracy challenger, not the speed baseline.

## Canonical parsing task

### Contract

The canonical semantic task is deliberately narrow:

```text
input:  SRT transcript, log, source text, or code; every source line has a stable ID
policy: eight externally supplied categories and definitions
output: strict JSON: { category_id: [line_id, ...] }
empty:  category omitted or [] exactly as the policy specifies
```

The policy owner supplies the eight category definitions, language examples,
and gold labels. Do **not** invent threat/abuse definitions or use the model as
an autonomous moderation authority. False accusations cost more than missed
incidents, so precision and citation validity are promotion gates.

### Pipeline

```text
SRT/log/code
  -> deterministic parse, normalize, line-ID, language detect, chunk
  -> local semantic classifier selected by language/task
  -> JSON/schema + line-existence validator + dedupe
  -> accepted local result | bounded evidence packet to Gemini Flash/cloud fallback
```

Deterministic work never needs an LLM: SRT timestamps, line numbering, JSON,
JUnit/compiler output, regex signatures, file paths, and stack traces should be
extracted locally first. The LLM only judges semantic categories in bounded
chunks. Raw artifacts stay on disk; no full transcript or unbounded command
output is inserted into a costly agent context.

Chunking rule: preserve source line IDs; send overlapping semantic chunks only
when a category can cross a boundary; deduplicate at the validator. The model
returns line IDs, never copied transcript text, unless an explicitly approved
review packet requires a short quote.

## Routing policy

| Input/task | First route | Escalate when | Cloud route receives |
|---|---|---|---|
| Compiler/test/log diagnostics | deterministic parser, then local 3–8B classifier only for ambiguity | parser cannot classify, invalid citation, or low confidence | error clusters and requested log ranges only |
| Swedish/English/mixed SRT | fastest locally validated multilingual model | precision floor fails, uncertain language, or deadline fails | chunk IDs and only the selected chunks |
| Polish SRT | test Bielik 7B/11B against multilingual challengers; retain only if it wins precision or task time | same gate | selected Polish chunks plus local evidence |
| Code/source classification | code-capable small local challenger | output alters architecture or confidence/citation gate fails | exact files/ranges, not repository dump |
| Large/urgent backlog | local concurrent batch if quality floor is met | queue makes deadline fail | Gemini Flash/API batch lane with explicit spend cap |

The orchestrator knows provider/model/cost. Worker prompts see only their role,
the schema, permitted evidence, and stop condition; they do not know competing
models or routing economics.

## Measurements

Do not call all of these “tokens per second.” Persist one row per
`hardware + runtime + model + quant + context + concurrency + fixture`.

```text
prefill_input_tps = input_tokens / prefill_seconds
decode_output_tps = output_tokens / decode_seconds
TTFT              = request sent -> first streamed output
TPOT              = (end_to_end_seconds - TTFT) / (output_tokens - 1)
E2E               = request sent -> validated JSON result
accepted_docs_h   = accepted_documents / elapsed_hours
precision         = true_positive_flags / all_model_flags
invalid_citation  = returned_line_ids_not_in_input / all_returned_line_ids
```

Also capture: p50/p95/p99 values, cold versus warm model load, raw input bytes,
tokenizer used, output tokens, context configured/resident, KV-cache use,
CPU/GPU RAM, GPU power/energy if exposed, concurrent requests, retries, and
human correction time. `input_tokens` and `output_tokens` remain `unknown` if
the local runtime cannot expose them; do not estimate them from bytes.

For this task, rank in this order:

```text
1. precision and invalid-citation rate
2. validated documents/hour / p95 E2E
3. prefill input tok/s or MB/s
4. decode tok/s
5. local cash/energy and operator attention
```

Fast decode alone is a weak proxy because the desired answer is small JSON.
Prefill throughput and E2E time dominate large transcripts/logs.

## Promotion rule and A/B protocol

Use a language-balanced, human-labelled gold set. It must include Swedish,
English, Polish, code, logs, and code-switched inputs where they occur. It is
not a “public/private” question; source material may be any safely handled
representative fixture. Store only permissible fixtures and hashes/metadata for
restricted ones.

1. Freeze the eight-category policy, prompt/schema, chunking, model version,
   quantization, runtime version, context, and concurrency.
2. Warm the model once; then run at least five measured repetitions per fixture.
   Record cold start separately.
3. Run the same fixture through local candidates and Gemini 3.8 Flash. Measure
   direct endpoint behavior and the full extraction/validation workflow.
4. Review all local positives and a random sample of local negatives. Attribute
   every false positive, miss, invalid line ID, timeout, and retry.
5. Promote a local route only when it has **zero invalid citations**, meets the
   policy precision floor, and wins the declared time or cost objective without
   an unacceptable p95 regression. Otherwise retain it as `test` or demote it.

Until the policy owner chooses a numeric precision floor, use `0.95` macro
precision as a conservative provisional gate. This is a temporary evaluation
default, not a safety guarantee.

### Gemini comparison

Use both comparisons; they answer different questions.

| Comparison | Answers | Does not answer |
|---|---|---|
| Direct endpoint | TTFT, prefill/decode behavior, response quality, cash per request | agent/tool-loop overhead |
| Full workflow | end-to-end throughput, local offload value, queue/validation cost | intrinsic model decode speed alone |

Native AGY plan quota is weighted capacity. Record plan-capacity delta before
and after a run, but never convert its percentage into invented input/output
tokens. If a direct Gemini endpoint is unavailable, retain client-observed E2E
time and capacity delta; mark token-level cloud fields unknown.

## Candidate families and runtime matrix

Use current releases only after checking each model card, language fit, licence,
available quantization, and runtime support. The candidate list is a test queue,
not a recommendation to download every model.

| Family/vendor | Why test it | Initial size band | Special rule |
|---|---|---:|---|
| Bielik / SpeakLeash | Polish and European-language baseline; Bielik v3 has 7B and 11B variants | 7B, 11B | retain only if Polish precision compensates for speed/memory |
| Qwen | multilingual/code generalist challenger | 4B–8B, then 14B only if needed | first general multilingual candidate |
| Gemma | compact general-language challenger | 4B–12B | validate licence and structured-JSON reliability |
| Llama | broad ecosystem/runtime availability | 3B–8B | use only current instruct variant with needed language quality |
| Mistral | European-language/general challenger | small/medium current variant | test as a language-quality challenger, not by brand |

Do not test MoE, 30B+, or RAM/SSD-offloaded models for the fast parser lane
unless a smaller candidate failed the accuracy gate. Larger models can be
separate quality/offline routes; they are not evidence of fast local parsing.

| Host | First runtime | Challengers | Purpose |
|---|---|---|---|
| M1 Max | current Ollama GGUF baseline | MLX-native server/runtime, llama.cpp Metal | determine whether engine/quant—not hardware alone—causes 13–27 tok/s |
| RTX 4090 | llama.cpp CUDA for reproducible single-request tests | Ollama, vLLM/SGLang for serving/concurrency | establish fully GPU-resident parsing baseline |

`llama-bench` is the common synthetic benchmark because it separately measures
prompt processing (`pp`) and text generation (`tg`), repeats tests, emits JSON,
and can prefill a specified context depth. Its values exclude tokenization and
sampling, therefore they are a runtime metric—not E2E application latency.

Example baseline shape, with actual model path/version substituted and results
saved outside the repository unless intentionally curated:

```bash
llama-bench -m /models/MODEL.gguf -p 2048,8192,32768 -n 128 -r 5 -o jsonl
```

Run prompt-processing-only and generation-only cases where the installed
version supports them; retain command output, engine commit, driver, model
hash, and complete invocation alongside the result. Do not compare a short,
warm single-stream `llama-bench` decode result to a cold, long-context cloud
agent session.

For a multi-request 4090 service, vLLM reports TTFT, inter-token latency,
prefill/decode time, token counters, KV-cache use, and request counts via its
metrics endpoint. Benchmark with concurrency `1`, then the real expected batch
size; higher throughput is not a win if interactive p95 TTFT becomes unusable.

## Hardware decision ladder

Hardware prices and availability are volatile; verify Swedish retail price,
warranty, drivers, chassis/PSU, and actual local benchmark evidence before a
purchase. Compare total cost of ownership, not headline VRAM.

| Tier | Candidate decision | What it solves | Do not buy when |
|---|---|---|---|
| 0 — use owned equipment | make the 4090 the parser server; run small fully resident models; retain M1 for portable/offline/background work | immediate 3–12B inference with 24GB VRAM and no new capital cost | the 4090 baseline already meets quality and p95 deadline |
| 1 — focused 32GB upgrade | evaluate RTX 5090 32GB and AMD Radeon AI PRO R9700 32GB after local tests | extra context/model headroom beyond 24GB | the workload is bounded parsing; 8GB extra VRAM alone rarely creates a proportional parser-speed gain |
| 2 — high-VRAM workstation | RTX PRO 6000 Blackwell 96GB-class system, proper workstation cooling/PSU/PCIe | genuinely large local model, long context, or sustained multi-user serving | purchased merely to replace Gemini Flash for small structured extraction |
| 3 — multi-GPU or rented overflow | dedicated multi-GPU/Threadripper-class server, or rented high-VRAM GPU for bursts | concurrent users or a model that cannot fit one GPU | expected utilisation is intermittent or a single-stream latency target is the only need |

Facts to carry into price/performance calculations:

- RTX 5090 has 32GB GDDR7; it is a capacity increase over the 4090, not a
  reason to discard a working 4090 before testing.
- Radeon AI PRO R9700 has 32GB GDDR6, 640 GB/s bandwidth, and a 300W board
  power specification. Its Linux/ROCm support is a prerequisite to validate on
  the intended runtime; do not assume Windows/Ollama parity.
- RTX PRO 6000 Blackwell workstation hardware has 96GB ECC GDDR7 and vendor
  specified bandwidth up to 1.8 TB/s. It is a large-model/concurrency purchase,
  not a cost-efficient answer to SRT line classification.

Calculate before purchase:

```text
local_TCO_month = hardware_purchase / amortization_months
                + kWh_month * actual_SEK_per_kWh
                + storage + maintenance + operator_time

monthly_saving  = cloud_cost_avoided - local_TCO_month
break_even_months = hardware_purchase / max(monthly_saving, 0)
```

No zero/negative-saving purchase is a Tokenomics win. Rented GPU remains the
overflow route when a large model is needed only occasionally. Multi-GPU model
parallelism is not assumed to improve one user’s token latency; benchmark it
separately from concurrency throughput.

## Evidence sources and refresh discipline

Use first-party specifications and reproducible local runs for decisions.
Crowdsourced leaderboards identify candidates; they do not promote a route.

| Source | Use | URL |
|---|---|---|
| llama.cpp `llama-bench` | reproducible pp/tg benchmark methodology and JSON output | https://github.com/ggml-org/llama.cpp/tree/master/tools/llama-bench |
| vLLM metrics/bench | serving TTFT, TPOT/ITL, KV-cache and concurrency measurement | https://docs.vllm.ai/en/latest/benchmarking/cli/ |
| Local AI Arena | cross-hardware candidate discovery with prefill/decode separation | https://localaiarena.com/ |
| ComputeArena | community run discovery by model, hardware, runtime and context | https://computearena.ai/leaderboard |
| MLX inference bench | Apple-runtime challenger methodology | https://github.com/odysa/mlx-inference-bench |
| Bielik model card | exact Polish baseline version/license/model details | https://huggingface.co/speakleash/Bielik-11B-v3.0-Instruct |
| NVIDIA/AMD product pages | VRAM, power, bandwidth and platform prerequisites | https://www.nvidia.com/en-us/geforce/graphics-cards/50-series/rtx-5090/ ; https://www.amd.com/en/products/graphics/workstations/radeon-ai-pro/ai-9000-series/amd-radeon-ai-pro-r9700.html ; https://www.nvidia.com/en-us/products/workstations/professional-desktop-gpus/rtx-pro-6000-family/ |

Refresh candidate models/runtimes monthly. Refresh hardware and cloud/rental
prices weekly. A changed model, quant, runtime, driver, operating system,
context, or concurrency invalidates previous speed comparisons until rerun.

## Status vocabulary

```text
watch    external result only
test     locally installed, not yet proven on the gold set
promote  meets precision/citation/time criteria for an explicit lane
retain   still passing a current local regression run
demote   regression, deadline failure, or invalid-citation failure
retire   unavailable, unsupported, obsolete, or no longer economical
```

