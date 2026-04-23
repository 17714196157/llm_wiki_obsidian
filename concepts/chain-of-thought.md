---
title: Chain-of-Thought (CoT)
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [pretraining, fine-tuning]
sources: [raw/articles/2602-13517-think-deep-not-just-long.md]
---

# Chain-of-Thought (CoT)

## 定义

**Chain-of-Thought (CoT)** 是一种提示技术，引导大语言模型生成显式的推理步骤（思考轨迹），而非直接输出最终答案。由 Wei et al. (2022) 提出。

## 基本原理

- 通过生成中间推理步骤，模型可以更可靠地解决复杂问题
- CoT 使推理过程 "可见"，便于调试和理解
- 是 [[test-time-scaling]] 的核心机制之一

## 发展脉络

1. **Zero-shot CoT** -- "Let's think step by step" 提示
2. **Few-shot CoT** -- 提供带推理步骤的示例
3. **Long CoT** -- 鼓励模型生成更长的思考轨迹 (o1, DeepSeek-R1 等)
4. **自适应 CoT** -- 根据问题动态调整推理长度

## 局限性

- [[llm-overthinking]] -- 更长的 CoT 不一定更好
- 原始 token 数是不可靠的推理质量代理
- 过度推理可能导致性能退化

## 与 Deep-Thinking Ratio 的关系

[[deep-thinking-ratio]] 提供了一种超越 CoT 长度的更细粒度度量：
- 不关注 "生成了多少 token"
- 关注 "每个 token 的内部预测是否经历了深层修订"
- 高 DTR 的 CoT = 有效推理
- 低 DTR 的长 CoT = [[llm-overthinking]]

## 相关概念

- [[test-time-scaling]] -- 推理时计算扩展
- [[llm-overthinking]] -- 过度推理现象
- [[deep-thinking-ratio]] -- 推理努力度量
