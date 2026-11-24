# Can you compare the classic attention mechanism and paged attention mechanism for transformer model architecture?

## 1. Abstract

Anyone serving a transformer language model to many users must decide how attention stores its key-value (KV) cache, which grows with every token. Both mechanisms compute the same attention. Classic attention reads each request's cache from one contiguous region reserved up front; paged attention reads it from small blocks allocated as tokens arrive. When cache memory limits how many requests run together, paged attention sustains 1.7 to 2.7 times the request rate of an idealized contiguous system at similar latency. When memory is plentiful and requests are short, that lead shrinks.

**Key takeaways:**

- The attention arithmetic is identical; only the cache layout differs.
- Contiguous reservation can leave as little as 20.4% of cache memory in use.
- Blocks waste at most part of one block per request, and requests can share them.
- Paged attention's attention kernel takes 20 to 26% longer than FasterTransformer's.

## 2. Terminology

| Term | Definition |
|---|---|
| **Query, key, value (Q, K, V)** | The three matrices attention works on: queries, and an unordered set of key-value pairs [\[1\]](../sources/attention-wikipedia-machine-learning.md#standard-scaled-dot-product-attention). |
| **KV cache** | The keys and values of earlier tokens, kept so they are not recomputed for each new token [\[2\]](../sources/transformer-wikipedia-deep-learning-architecture.md#kv-caching). |
| **Prefill, decode** | Prefill computes the cache for the prompt; decode then generates tokens one at a time [\[2\]](../sources/transformer-wikipedia-deep-learning-architecture.md#kv-caching). |
| **Block** | A fixed number of tokens' keys and values, stored together [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention). |
| **Block table** | The map from a request's logical blocks to physical blocks in GPU memory [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager). |
| **[→]** | Our own reasoning, after a statement or on a section's heading: a conclusion, comparison or example no cited source states. |
| **[?]** | A gap, after a statement or on a heading: something the question needs that the sources don't answer. |

## 3. The choices

The choice comes up whenever a GPU serves a transformer to many users at once [→]. It decides how many requests fit on the GPU together: batch size is limited by GPU memory, and throughput is memory-bound [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving).

The base scenario is a chat service serving OPT-13B on one GPU [→]. One token's keys and values take 800 KB, so one request of 2,048 tokens can need 1.6 GB [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). A GPU holds tens of gigabytes, so even with all of it given to the cache, only a few tens of requests fit [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). Prompts vary in length, and nobody knows in advance how long each answer will be [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving).

### 3.1 Classic attention reads one contiguous cache

Classic attention computes $\mathrm{softmax}\left(QK^{\mathsf{T}}/\sqrt{d_k}\right)V$ [\[1\]](../sources/attention-wikipedia-machine-learning.md#standard-scaled-dot-product-attention), in several heads at once [\[6\]](../sources/attention-wikipedia-machine-learning.md#multi-head-attention). The paper measures it in two classic designs: FasterTransformer, an inference engine optimized for latency, and Orca, a serving system optimized for throughput [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). Serving systems before paged attention stored each request's KV cache as one contiguous tensor, because most deep learning operators expect contiguous memory [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). Not knowing the output length, they reserved space for the request's maximum possible length [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving).

The difference a newcomer misses: the cost is not in the arithmetic but in the reservation. Reserved slots, over-provisioning for the maximum length and the memory allocator's gaps all waste memory [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). In the paper's measurements, the memory actually used can be as low as 20.4% [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). How much a classic system reserves changes the gap: against Orca (Max), which reserves the full 2,048 tokens, vLLM sustains 2.7 to 8 times the request rate, and against FasterTransformer up to 22 times [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling).

Classic attention comes closest when one condition changes: memory is plentiful for the requests served. Take OPT-175B, whose server leaves a large GPU memory space for the cache, serving the short requests of the Alpaca dataset. There, Orca (Oracle) and Orca (Pow2) batch many requests despite the waste. The workload becomes bound by computation rather than memory, and vLLM's lead over them is less pronounced [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). There, the classic kernel's speed counts for more (see Speed) [→]. The sources measure no workload where classic attention wins end to end [?].

### 3.2 Paged attention reads the cache in blocks

Paged attention "allows storing continuous keys and values in non-contiguous memory space" [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention). It splits each request's KV cache into blocks of a fixed number of tokens and computes attention block by block, which gives the same result [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention). A block manager allocates a physical block only when a request needs one, fills each request's blocks from left to right, and records the mapping in a block table; only the last block's unfilled positions are reserved [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager). The idea is virtual memory's paging: fixed-size pages, mapped to physical pages, allocated as needed [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager).

The difference from its nearest neighbor, classic attention: the reservation is gone, but the kernel must look up and fetch each block separately [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention), [\[8\]](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark).

Paged attention wins in the base scenario. vLLM is the serving system built on it [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract). On ShareGPT conversations, vLLM batches 2.2 times as many OPT-13B requests at once as Orca (Oracle) [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). Orca (Oracle) is a contiguous system given every output length in advance, an upper bound no real system reaches [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). Across the paper's three models, vLLM sustains 1.7 to 2.7 times Orca (Oracle)'s request rate at similar latency [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). If the chat service's users ask for several samples or a beam search, the lead grows, because the shared prompt's blocks are shared [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving), [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract).

## 4. When to choose which

```mermaid
flowchart TD
    A["Does cache memory limit the batch?"] -->|yes| P["Paged attention"]
    A -->|no| B["Do requests share prompts or beams?"]
    B -->|yes| P
    B -->|no| C["Classic attention is competitive"]
    classDef pick fill:#dbeafe,color:#111
    class P pick
```

*Figure 4-1: The two conditions the scenarios changed, cache memory and sharing, as questions; the shaded answer is the base scenario's [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling), [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving), [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract).*

- **If cache memory limits how many requests run at once, use paged attention** [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). Long or varied requests are the usual sign [→].
- **If requests share a prompt, or use parallel sampling or beam search, use paged attention:** blocks can be shared, and contiguous reservations cannot be [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving), [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract).
- **If memory is plentiful for the requests served and nothing is shared, classic attention is competitive** [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling).

## 5. At a glance

| Dimension | Classic attention (contiguous cache) | Paged attention (block cache) |
|---|---|---|
| What the attention computes | $\mathrm{softmax}(QK^{\mathsf{T}}/\sqrt{d_k})V$ [\[1\]](../sources/attention-wikipedia-machine-learning.md#standard-scaled-dot-product-attention) | The same, block by block [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention) |
| Memory reserved and wasted | The maximum length, up front; as little as 20.4% used [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving) | Part of the last block per request [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager) |
| Sharing | Prevented by each request's reservation [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving) | Within and across requests [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract) |
| Speed | The faster attention kernel [\[8\]](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark) | Attention kernel takes 20 to 26% longer [\[8\]](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark); 1.7 to 2.7 times Orca (Oracle)'s request rate [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling) |
| Running out of memory | Not described in the sources [?] | Preempts whole requests; swaps or recomputes their blocks [\[10\]](../sources/paged-attention-kwon-sosp2023.md#45-scheduling-and-preemption) |
| What operating it takes | Standard contiguous kernels [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving) | A block-aware kernel [\[11\]](../sources/paged-attention-vllm-design-doc.md#concepts) and a block size, 16 by default [\[12\]](../sources/paged-attention-kwon-sosp2023.md#73-comparing-recomputation-and-swapping) |

## 6. Dimensions to think through

### 6.1 What the attention computes

This dimension rules out a worry: both give the same outputs. Paged attention rewrites the attention sum as a sum over blocks [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention), [\[1\]](../sources/attention-wikipedia-machine-learning.md#standard-scaled-dot-product-attention). Only where the keys and values are read from changes [→].

### 6.2 Memory reserved and wasted

Memory decides the batch size (see The choices). Classic attention's contiguous cache wastes memory three ways: slots reserved for future tokens, internal fragmentation from reserving the maximum length, and external fragmentation from the allocator [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). Paged attention reserves only the last block's unfilled positions, which removes most of that waste [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager). At 16 tokens a block, a request wastes at most 15 token slots: 15 × 800 KB = 12 MB for OPT-13B [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager), [\[12\]](../sources/paged-attention-kwon-sosp2023.md#73-comparing-recomputation-and-swapping), [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). A contiguous cache can reserve up to 2,048 × 800 KB ≈ 1,638 MB per request, so the paged waste is under 1% of that: 12 ÷ 1,638 ≈ 0.7% [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving).

![Line chart of KV cache memory one OPT-13B request holds against tokens so far: a dashed flat line at 1,638 MB for the contiguous cache, and a line rising from 0 to 1,638 MB at 2,048 tokens for 16-token blocks](figures/how-does-classic-attention-compare-with-paged-attention-in-transformers/memory-per-request.svg)

*Figure 6-1: A contiguous cache holds 2,048 × 800 KB = 1,638 MB from the first token (dashed); blocks hold only what the request has used, in 16-token steps of 12.8 MB, so a request that stops early frees the difference [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving), [\[4\]](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager), [\[12\]](../sources/paged-attention-kwon-sosp2023.md#73-comparing-recomputation-and-swapping).*

### 6.3 Sharing

Sharing matters when requests repeat a prompt, because each copy of its cache takes memory that could hold another request [→]. Parallel samples from one prompt can share the prompt's cache, which was 12% of cache memory in the paper's experiment [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving). Beam search can save up to 55% through sharing [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving), at beam width 6 for OPT-13B on Alpaca [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). Blocks make this sharing possible within and across requests [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract); a contiguous reservation prevents it [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving).

### 6.4 Speed

Speed matters once memory stops limiting the batch: then each step's kernel time sets the throughput [→]. Reading through a block table costs time: extra lookups, branches and handling of variable lengths make the attention kernel take 20 to 26% longer than FasterTransformer's [\[8\]](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark). Only the attention operator pays this, not the model's other layers [\[8\]](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark). End to end, the larger batch outweighs it while memory is the limit [\[8\]](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark), [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling).

### 6.5 Running out of memory

Output lengths are unknown, so paged attention can run out of free blocks mid-answer [\[10\]](../sources/paged-attention-kwon-sosp2023.md#45-scheduling-and-preemption). vLLM then preempts the latest requests first, evicting all of a request's blocks or none [\[10\]](../sources/paged-attention-kwon-sosp2023.md#45-scheduling-and-preemption). It brings them back by swapping them to CPU memory or by recomputing them in one prefill step [\[10\]](../sources/paged-attention-kwon-sosp2023.md#45-scheduling-and-preemption). Recomputation suits small blocks and swapping large ones; from 16 to 64 tokens a block, the two perform alike [\[12\]](../sources/paged-attention-kwon-sosp2023.md#73-comparing-recomputation-and-swapping). How contiguous systems behave when memory runs out is not described in the sources read [?].

### 6.6 What operating it takes

A setting matters because a wrong value costs throughput on the workload it was chosen for [→]. Paged attention adds one setting, the block size. Too small, and the GPU reads the cache with too little parallelism; too large, and fragmentation grows inside blocks and sharing becomes less likely [\[13\]](../sources/paged-attention-kwon-sosp2023.md#72-impact-of-block-size). A block size of 16 works for most workloads [\[13\]](../sources/paged-attention-kwon-sosp2023.md#72-impact-of-block-size), and vLLM uses it by default [\[12\]](../sources/paged-attention-kwon-sosp2023.md#73-comparing-recomputation-and-swapping). Classic attention needs no such setting [→]. It uses the contiguous tensors that most framework operators expect [\[5\]](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving).

## 7. Pitfalls

- **Expecting different answers.** Paged attention computes the same attention [\[3\]](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention). If outputs differ, look at the implementation, not the mechanism [→].
- **Reading the headline as universal.** The paper's 2 to 4 times throughput is against FasterTransformer and Orca at the same latency, and grows with longer sequences, larger models and more complex decoding [\[9\]](../sources/paged-attention-kwon-sosp2023.md#abstract). Against Orca (Oracle), which knows every output length, it is 1.7 to 2.7 times on ShareGPT [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling). With short requests and plentiful memory, the gap narrows [\[7\]](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling).

## 8. Where to go next

- How does vLLM schedule requests across GPUs?
- How does FlashAttention speed up the attention arithmetic itself?

## 9. References

### 9.1 Sources read

- [paged-attention-kwon-sosp2023](../sources/paged-attention-kwon-sosp2023.md): the memory problem, the paged design, scheduling under memory pressure, and the measurements (abstract; sections 3, 4.1, 4.2, 4.5, 6.2 and 7.1 to 7.3).
- [paged-attention-vllm-design-doc](../sources/paged-attention-vllm-design-doc.md): how the paged kernel divides its work.
- [attention-wikipedia-machine-learning](../sources/attention-wikipedia-machine-learning.md): the attention formula and multi-head attention.
- [transformer-wikipedia-deep-learning-architecture](../sources/transformer-wikipedia-deep-learning-architecture.md): the KV cache, prefill and decode.
- The original attention paper (Vaswani and colleagues, 2017) is recorded as a link; no claim here cites it.

### 9.2 Citations

1. [attention-wikipedia-machine-learning: Standard scaled dot-product attention](../sources/attention-wikipedia-machine-learning.md#standard-scaled-dot-product-attention)
2. [transformer-wikipedia-deep-learning-architecture: KV caching](../sources/transformer-wikipedia-deep-learning-architecture.md#kv-caching)
3. [paged-attention-kwon-sosp2023: 4.1 PagedAttention](../sources/paged-attention-kwon-sosp2023.md#41-pagedattention)
4. [paged-attention-kwon-sosp2023: 4.2 KV Cache Manager](../sources/paged-attention-kwon-sosp2023.md#42-kv-cache-manager)
5. [paged-attention-kwon-sosp2023: 3 Memory Challenges in LLM Serving](../sources/paged-attention-kwon-sosp2023.md#3-memory-challenges-in-llm-serving)
6. [attention-wikipedia-machine-learning: Multi-head attention](../sources/attention-wikipedia-machine-learning.md#multi-head-attention)
7. [paged-attention-kwon-sosp2023: 6.2 Basic Sampling](../sources/paged-attention-kwon-sosp2023.md#62-basic-sampling)
8. [paged-attention-kwon-sosp2023: 7.1 Kernel Microbenchmark](../sources/paged-attention-kwon-sosp2023.md#71-kernel-microbenchmark)
9. [paged-attention-kwon-sosp2023: Abstract](../sources/paged-attention-kwon-sosp2023.md#abstract)
10. [paged-attention-kwon-sosp2023: 4.5 Scheduling and Preemption](../sources/paged-attention-kwon-sosp2023.md#45-scheduling-and-preemption)
11. [paged-attention-vllm-design-doc: Concepts](../sources/paged-attention-vllm-design-doc.md#concepts)
12. [paged-attention-kwon-sosp2023: 7.3 Comparing Recomputation and Swapping](../sources/paged-attention-kwon-sosp2023.md#73-comparing-recomputation-and-swapping)
13. [paged-attention-kwon-sosp2023: 7.2 Impact of Block Size](../sources/paged-attention-kwon-sosp2023.md#72-impact-of-block-size)

## 10. Metadata

- **Version:** 2.2
- **Template:** comparison
- **Language:** en
- **Model:** claude-opus-5-5
- **Created:** 2026-10-27
- **Last modified:** 2026-11-24
- **Words:** 1,813 (about 8 min)
- **History:**
  - 2.2 (2026-11-24): An SVG figure drawn from the narrative's own cited numbers, where a chart shows what a diagram can't
  - 2.1 (2026-11-24): Lint: classic attention comes closest rather than wins when memory is plentiful; Orca (Oracle) as an upper bound and the Orca (Max) and FasterTransformer gaps; the baselines introduced; the default block size cited to its section; one name per candidate; why each dimension matters
  - 2.0 (2026-11-24): Regenerated under sllmwiki 1.4.0's guides: a new spine and outline on the paper's OPT-13B scenario; the decision rule now rests on the paper's measurements (memory-bound against compute-bound workloads, the kernel's 20 to 26% overhead) instead of known output lengths; new dimensions for running out of memory and the block size; read sections 4.5, 6.2 and 7.1 to 7.3
  - 1.1 (2026-10-27): Lint: corrected two overstated claims (waste, sharing), split statements that were both cited and marked, cited the Q, K, V definition
  - 1.0 (2026-10-27): First version.
- **Reading plan:**
  1. `transformer-wikipedia-deep-learning-architecture`, in full: medium relevance, authority C, undated
  2. `attention-wikipedia-machine-learning`, in full: high relevance, authority C, undated
  3. `paged-attention-vllm-design-doc`, in full: high relevance, authority A, undated
  4. `paged-attention-kwon-sosp2023`, in full: high relevance, authority A, 2023-09-12
  5. `attention-vaswani-2017-attention-is-all-you-need`, not read: a link, no saved copy
