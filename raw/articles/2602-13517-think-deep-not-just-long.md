# Think Deep, Not Just Long: Measuring LLM Reasoning Effort via Deep-Thinking Tokens

**Source**: arXiv 2602.13517v1
**Authors**: Wei-Lin Chen, Liqian Peng, Tian Tan, Chao Zhao, Blake JianHang Chen, Ziqian Lin, Alec Go, Yu Meng
**Published**: 2026-02-13
**Categories**: cs.CL
**PDF**: https://arxiv.org/pdf/2602.13517

## Abstract

Large language models (LLMs) have demonstrated impressive reasoning capabilities by scaling test-time compute via long Chain-of-Thought (CoT). However, recent findings suggest that raw token counts are unreliable proxies for reasoning quality: increased generation length does not consistently correlate with accuracy and may instead signal "overthinking," leading to performance degradation. In this work, we quantify inference-time effort by identifying deep-thinking tokens -- tokens where internal predictions undergo significant revisions in deeper model layers prior to convergence. Across four challenging mathematical and scientific benchmarks (AIME 24/25, HMMT 25, and GPQA-diamond) and a diverse set of reasoning-focused models (GPT-OSS, DeepSeek-R1, and Qwen3), we show that deep-thinking ratio (the proportion of deep-thinking tokens in a generated sequence) exhibits a robust and consistently positive correlation with accuracy, substantially outperforming both length-based and confidence-based baselines. Leveraging this insight, we introduce Think@n, a test-time scaling strategy that prioritizes samples with high deep-thinking ratios. We demonstrate that Think@n matches or exceeds standard self-consistency performance while significantly reducing inference costs by enabling the early rejection of unpromising generations based on short prefixes.

## Key Findings

1. **Raw token count correlates negatively with accuracy** (avg r=-0.544) -- longer output does not mean better reasoning
2. **Deep-thinking ratio (DTR) correlates strongly with accuracy** (avg r=0.828) -- tokens where internal predictions undergo significant revision in deeper layers
3. **Think@n strategy**: Test-time scaling by selecting samples with high DTR, matching/exceeding self-consistency at ~half the compute cost
4. **Tested models**: GPT-OSS, DeepSeek-R1, Qwen3
5. **Tested benchmarks**: AIME 2024/2025, HMMT 2025, GPQA-diamond

## Methodology

- Projects intermediate-layer hidden states into vocabulary space
- Compares each layer's prediction distribution to final-layer distribution using Jensen-Shannon divergence
- Tokens whose distributions don't converge until deeper layers are "deep-thinking tokens"
- DTR = proportion of deep-thinking tokens in a generated sequence

## Think@n Strategy

- Generate multiple samples
- Estimate DTR from short prefixes
- Early-halt unpromising generations (low DTR)
- Aggregate responses with higher DTR
- Achieves self-consistency-level performance at approximately half the inference cost
