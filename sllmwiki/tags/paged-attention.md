# paged-attention

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Paged attention: storing the attention key-value cache in fixed-size blocks, as an operating system pages memory

**Path:** [machine-learning](machine-learning.md) / [llm-serving](llm-serving.md) / paged-attention

## Sources

- [paged-attention-kwon-sosp2023](../sources/paged-attention-kwon-sosp2023.md): Kwon and colleagues (SOSP 2023) introduce PagedAttention and vLLM, an LLM serving system built on it.
- [paged-attention-vllm-design-doc](../sources/paged-attention-vllm-design-doc.md): The vLLM project's design document for its PagedAttention kernel, the GPU code that computes attention when the KV cache is stored in blocks.
