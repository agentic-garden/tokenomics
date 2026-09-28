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

## Dedicated AI desktops and minis: the missing comparison

This table compares the machines that are actually sold for local AI work. It
does **not** turn a vendor's NPU TOPS number or memory capacity into an LLM
throughput claim. `TG` is one-stream decode; `PP` is prompt/prefill throughput.
Those phases answer different routing questions.

| System | Memory / practical advantage | Comparable measured evidence | Decision for this fleet |
|---|---|---|---|
| **Owned RTX 4090 PC** | 24 GB VRAM; conventional CUDA stack; already owned | Bielik-11B at 32K: **96.15 TG tok/s** sustained (owned measurement) | **Keep as primary small-model parser.** It is already near the 100 TG target and is the no-purchase baseline. |
| **NVIDIA DGX Spark (GB10)** | 128 GB coherent LPDDR5X; NVIDIA states support for up-to-200B-parameter local models | Official `llama.cpp` DGX benchmark: Qwen2.5-Coder 7B Q8 at 4K: **~28.6 TG / 2,100 PP tok/s**; Qwen3 30B-A3B Q8 at 4K: **~54.8 TG / 2,633 PP tok/s**. A separate Bielik NVFP4/DFlash run reported 73–78 TG. | Buy for **large-model fit, experimentation and concurrent serving**, not to accelerate this 8–11B parser. Its much larger RAM does not beat the owned 4090's observed Bielik decode result. |
| **AMD Ryzen AI Max+ 395 / Strix Halo 128 GB mini** (for example GMKtec EVO-X2 / Framework Desktop class) | 128 GB unified memory makes 27B–70B+ local models and long KV possible in a compact box | Reproducible EVO-X2, Qwen3.8 27B Q5, Vulkan, 128K: **16.45 TG / 31.44 PP tok/s**, 3.06s TTFT. Claims of 50–60+ TG for smaller MoEs exist but are runtime/quant/context-dependent and are not a purchase-grade comparison. | A **capacity-per-krona and quiet always-on lab box**, not a single-stream speed upgrade. Do not buy for log/SRT parsing while the 4090 is available. |
| **Mac Studio M5 Max** (up to 128 GB) | 614 GB/s unified-memory bandwidth; quiet, compact, MLX-native long-context or multi-model endpoint | M5 Max / MLX external run: Qwen3 4B **171 TG / 6,385 PP tok/s**, Qwen3 30B-A3B **131 TG / 5,402 PP tok/s**. Independent M5 Ultra testing also shows why active parameters matter: 14B dense **80 TG**, gpt-oss-120B MoE **120 TG**, Qwen 235B-A22B **25.7 TG**. | A real **fast local inference option**, unlike the Mac mini. It can cover the local parser lane and run models too large for 24 GB VRAM. Validate Bielik and your exact SRT fixture before buying: neither number is a direct Bielik comparison. |
| **Mac Studio M5 Ultra** (256–512 GB) | 1.2 TB/s unified-memory bandwidth; 512 GB fit for very large private models and long context; Thunderbolt 5/RDMA clustering | The M5 Ultra benchmark above used ~8–10K prompts and averaged three runs; it is evidence for large-model capability, not a guarantee at 128K context or under concurrency. | The best compact **private capacity + good decode** choice in this table. Buy it if you need 120B–235B-class local work, massive context, quiet always-on service, or a future multi-Studio cluster—not merely to replace a working 4090 for 11B parsing. |
| **RTX 5090 32 GB workstation** | +8 GB VRAM vs 4090; strongest conventional single-GPU CUDA upgrade | External Qwen3.8 27B NVFP4/vLLM at 65K: **155.2 TG tok/s**; exact Bielik result unknown. | The only plausible single-GPU upgrade if the 4090 fails a demonstrated quality/context requirement. Require a local rental/borrowed benchmark first. |
| **RTX PRO 6000 Blackwell 96 GB workstation** | 96 GB ECC VRAM; workstation deployment / large models / multi-user concurrency | External 14B Q4 at 16K: **96.9 TG tok/s**. | Purchase only for **96 GB capacity, reliability or concurrency**, never merely to make a small parser faster. |

### Consequence: capacity and speed are separate purchases

```text
Need a fast 4B–11B parser?       existing RTX 4090 first.
Need a 27B–120B private model?   DGX Spark / Strix Halo / Mac Studio compare by fit,
                                  PP, sustained TG, power and software reliability.
Need 100+ TG from a larger model? test or rent RTX 5090-class CUDA first.
Need Flash-like cloud latency?    local hardware has no credible general replacement;
                                  route urgent work to Flash and batch/private work local.
```

The DGX Spark and Strix Halo are attractive precisely because 128 GB lets a
single host retain weights and a large KV cache. That does not make either a
better *one-request decoder* than a 4090. The **M5 Mac Studio is the exception
worth testing**: its much higher unified-memory bandwidth and MLX runtime make
it a credible speed *and* capacity machine, rather than merely a quiet endpoint.

## Market snapshot: routes that can reach 100+ decode tok/s

These are **external measurements or claims**, not results from the two owned
machines. They establish which combinations deserve installation and local A/B
testing. Decode speed is shown only at the source's stated context/engine; it
must not be extrapolated to a long SRT.

| Host | Model / runtime / condition | Reported decode | Parsing implication | Evidence confidence |
|---|---|---:|---|---|
| Owned RTX 4090 24GB | Qwen3 4B, CUDA llama.cpp, TG128, short context | 150.6 tok/s | First fast multilingual-parser candidate; small enough to leave KV headroom | community run, reproducible llama.cpp details |
| Owned RTX 4090 24GB | Qwen3 8B Q4/Q8, llama.cpp, 4K–16K context | 104–141 tok/s | Main quality/speed candidate; test Swedish, English, Polish and code before promotion | multiple community/synthesis reports; context-sensitive |
| Owned RTX 4090 24GB | Qwen3 30B-A3B, heterogeneous KTransformers | 49.8 single; 98.8 aggregate 4-way | batch/concurrency experiment, **not** a low-latency single-parser default | primary project benchmark; CPU/RAM offload involved |
| Owned M1 Max 64GB | Qwen3.6 35B-A3B via experimental Splash Metal port | 144 tok/s | research lead only: reported 38K prompt TTFT is 72s, so unsuitable as a Flash-like long-SRT route until reproduced | single community experimental claim |
| RTX 5090 32GB | Qwen3.8 27B NVFP4, vLLM, 65K context | 155.2 tok/s | credible upgrade candidate if a larger local multilingual model is actually needed | benchmark-site run; must reproduce |
| RTX 5090 32GB | Qwen3.6 35B-A3B, vLLM | 437.3 tok/s | highly fast MoE lead; validate model quality, quant, and per-request vs aggregate throughput before believing/buying | single benchmark-site run |
| RTX PRO 6000 Blackwell 96GB | 14B Q4 at 16K context | 96.9 tok/s | buy for 96GB capacity, long context or concurrency—not to make small parsing magically cheap | external hardware benchmark |

### What this means for the two machines

1. **Do not buy anything to reach 100 tok/s parsing.** The RTX 4090 should do
   it for an appropriately small 4B–8B model, provided all weights and the
   working KV cache remain on the GPU.
2. **Do not use Bielik-11B/Ollama on the M1 as the speed reference.** Its
   13–27 tok/s observation is in the expected dense 11B/unified-memory class.
   On the Mac, test an MLX/Metal runtime and a small model for a compact batch
   lane; treat the 144 tok/s MoE/Splash claim as an experiment, not a purchase
   basis.
3. **For Polish:** benchmark Bielik 7B before Bielik 11B on the 4090. If its
   precision does not beat the 4B/8B multilingual winner enough to justify the
   slower E2E route, retain it only as a Polish escalation model.
4. **For raw parsing speed:** a 0.6B–4B local model or deterministic parser
   may be faster, but it is not promoted until the eight-category false-positive
   and line-citation evaluation proves it is usable.

The immediate candidate order is therefore:

```text
4090 + Qwen3 4B       -> speed floor / language-screening challenger
4090 + Qwen3 8B       -> expected primary local semantic parser
4090 + Bielik 7B      -> Polish precision challenger
M1 + small MLX model  -> portable/background batch challenger
M1 + Splash MoE       -> experimental research lane only
5090 + 27B/35B MoE    -> buy only if the 4090 fails the quality/context target
```

## Bielik-11B: machine evidence, not extrapolation

The relevant model is Bielik-11B-v3.0-Instruct. Do not silently substitute a
v2/v2.3 figure: v2 is Mistral-derived and several older community posts concern
memory fit rather than performance. The official Ollama variants show that v3
Q4_K_M is 6.7GB and Q8_0 is 12GB with a 32K advertised context, so either can
fit in a 24GB 4090 in principle; the remaining VRAM is needed for runtime and
KV cache.

| Machine / runtime | Exact Bielik configuration | Decode evidence | What is actually known |
|---|---|---:|---|
| Owned M1 Max 64GB / Ollama | user-reported Bielik-11B | 13–27 tok/s | real local observation; quant, context and cold/warm state still need recording |
| Apple Silicon / MLX-LM with cross-family speculative decoding | Bielik-11B target + Bielik 1.5B, Qwen2.5-1.5B, or Llama3.2-1B draft | no absolute t/s published in the paper abstract | structured Polish text reached up to 1.7x speedup; varied instructions failed to benefit consistently |
| NVIDIA GB10 / vLLM | Bielik-11B-v3 NVFP4 | 33.7 tok/s target only; 73–78 tok/s with DFlash speculative decode | single-stream, temp=0, 32K server configuration; useful NVFP4 proof, not a 4090/5090 result |
| RTX 3090/4090 24GB / llama.cpp | Bielik-11B-v2.3 Q8_0 | unknown | an older community inventory shows it fits with 20K configured context; no throughput was reported |
| **Owned RTX 4090 24GB / llama.cpp server** | **Bielik-11B; 32K configured context; exact quant/version unknown** | **96.15 tok/s aggregate over 1,734 output tokens; rolling 95.13–97.18** | **first-party local observation on 2026-09-28; prefill, TTFT, actual resident context, power and model hash still unknown** |
| RTX 5090 / RTX PRO 6000 | Bielik-11B-v3 | **unknown** | no credible published measurement found in this research pass |

### Bielik decision

```text
Bielik-11B is now a proven approximately-100 tok/s decoder on the owned RTX
4090 for a sustained 1,734-token generation. It remains a Polish-quality
challenger until the eight-category fixture proves its precision and E2E parser
throughput against Qwen candidates.

For speed on 4090: use this 11B result as the quality baseline; benchmark
Bielik 7B only if it delivers materially higher parsing E2E throughput without
losing the configured Polish precision floor.

For M1: do not assume speculative decoding will make it Flash-like. The only
published Apple result is a content-dependent multiplier, not an absolute speed.
```

Sources: [official Ollama v3 variants](https://ollama.com/qooba/bielik-11b-v3.0-instruct),
[GB10 NVFP4/DFlash measurement](https://huggingface.co/norecyc/Bielik-11B-v3.0-Instruct-NVFP4/commit/77dc7656fc66837481d1f6d66d1c0d76ae7f2d9b),
[Apple speculative-decoding study](https://arxiv.org/abs/2604.16368), and
[older 24GB fit inventory](https://www.reddit.com/r/LocalLLaMA/comments/1gai2ol/list_of_models_to_use_on_single_3090_or_4090/).

### Cloud comparison: Gemini 3.8 Flash High

External Artificial Analysis reporting puts Gemini 3.8 Flash High at roughly
300–305 output tok/s. Against the observed 4090 Bielik rate:

| Output-only measure | Bielik-11B on owned 4090 | Gemini 3.8 Flash High | Difference |
|---|---:|---:|---:|
| sustained output decode | 96.15 tok/s | ~300–305 tok/s | Flash ~3.1× faster |
| 100-token JSON result, decode only | ~1.04s | ~0.33s | ~0.71s |
| 500-token result, decode only | ~5.20s | ~1.65–1.67s | ~3.5s |

This is **not** an end-to-end conclusion. The published Flash output-speed
figure is a cloud benchmark, while the local record lacks Bielik prefill and
TTFT; cloud queue/network/effort and local prompt processing can dominate a
small structured response. For SRT classification, choose by measured p95:

```text
SRT total = deterministic preprocessing + prefill/TTFT + small JSON decode + validation
```

The local 4090 route is therefore viable when it meets the precision floor and
its measured p95 total is acceptable, especially for batch/offline work or to
avoid plan/API capacity. Gemini Flash remains the speed-first fallback for
urgent work or local queue saturation. Capture the next Bielik
`prompt eval time`/TTFT line before claiming either route wins long transcripts.

Source: [Artificial Analysis Gemini 3.8 Flash release analysis](https://artificialanalysis.ai/articles/gemini-3-8-flash)
(reported figures must be refreshed monthly).

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
| 1.5 — unified-memory workstation | evaluate Mac Studio M5 Max 128GB against the 4090 using the exact SRT fixture, then M5 Ultra only if >128GB is required | fast MLX local serving plus 30B–120B+ private-model capacity in a quiet compact machine | purchased on Apple peak-AI claims without proving the target model, context, runtime, precision and p95 |
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
| M1 Max bytes-per-token study | directly relevant 64GB M1 Max measurements and bandwidth method | https://github.com/rajeeja/local-llm-bench-m1max |
| NVIDIA DGX Spark specification | 128GB unified-memory capacity and official product positioning | https://www.nvidia.com/en-au/products/workstations/dgx-spark/ |
| llama.cpp DGX Spark benchmarks | reproducible PP/TG figures by model and context | https://github.com/ggml-org/llama.cpp/blob/master/benches/dgx-spark/dgx-spark.md |
| Ryzen AI Max+ 395 128GB run | reproducible 27B, 64K/128K Vulkan PP/TG/TTFT reference | https://github.com/erstmalreden/qwen3.8-27b-ryzen-ai-max-395-benchmarks |
| Apple M5 Studio technical specifications | authoritative M5 Max/Ultra memory and bandwidth configurations | https://support.apple.com/en-gb/128107 |
| M5 Studio M5 Max run | external MLX Qwen 4B and 30B-A3B PP/TG discovery; reproduce before purchase | https://tbreak.com/mac-studio-m5-max-review-local-ai/ |
| M5 Studio M5 Ultra run | independent 14B / gpt-oss-120B / Qwen 235B PP, TTFT and TG evidence | https://www.maclife.de/test/mac-studio-m5-ultra-test-100127629.html |
| M1 Max Splash claim | experimental 35B-A3B 144 tok/s lead; inspect full context/TTFT before use | https://www.reddit.com/r/LocalLLM/comments/1wqngu9/splash_on_m1_part_2_35ba3b_at_144_tok_s_on_a_2021/ |
| RTX 4090 Qwen measurements | 4B/8B candidate figures; community synthesis, not a purchase guarantee | https://markaicode.com/benchmarks/ollama-qwen-3-rtx-4090-latency-benchmark/ |
| RTX 5090 comparison | 32GB candidate runs with context, TTFT, memory and runtime displayed | https://llm-bench.io/hardware/rtx-5090 |
| RTX PRO 6000 context scaling | 96GB long-context/prefill/decode evidence; treat price claims as volatile | https://www.hardware-corner.net/gpu-llm-benchmarks/rtx-pro-6000-blackwell/ |
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
