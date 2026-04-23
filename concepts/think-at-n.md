---
title: Think@n
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [inference]
sources: [raw/articles/2602-13517-think-deep-not-just-long.md]
---

# Think@n

## 定义

**Think@n** 是一种基于 [[deep-thinking-ratio]] (DTR) 的 test-time scaling 策略，通过优先选择和聚合高 DTR 样本来提升推理性能，同时降低计算成本。

来源：[[think-deep-not-just-long]] (Chen et al., 2026)

## 算法流程

```
输入: 问题 Q, 样本数 n
1. 并行生成 n 个响应样本
2. 对每个样本，从短前缀估计 DTR
3. 基于 DTR 进行早期筛选：
   - 低 DTR 样本 → 提前终止（节省计算）
   - 高 DTR 样本 → 继续生成
4. 对高 DTR 样本的答案进行聚合（多数投票）
5. 返回聚合后的答案
```

## 核心优势

| 指标 | Think@n | Self-Consistency |
|------|---------|------------------|
| 准确率 | 相当或更优 | 基准 |
| 推理成本 | ~50% | 100% |
| 机制 | DTR 驱动的早期终止 | 无差别生成所有样本 |

## 为什么有效

1. **早期信号**：DTR 可以从短前缀可靠估计，无需等待完整生成
2. **质量筛选**：高 DTR 样本更可能包含有效推理
3. **成本节约**：低 DTR 样本提前终止，避免浪费计算资源

## 与相关方法的对比

- **Cons@n** (标准 Self-Consistency)：对所有样本多数投票，成本高
- **长 CoT**：单一长推理，存在 [[llm-overthinking]] 风险
- **Think@n**：结合并行采样 + 质量筛选 + 早期终止

## 开放问题

- Think@n 的最优样本数 n 如何确定？
- 在不同任务类型上是否需要调整 DTR 阈值？
- 能否与其他 test-time scaling 方法结合使用？

## 相关概念

- [[deep-thinking-ratio]] -- DTR 度量
- [[test-time-scaling]] -- 推理时计算扩展
- [[llm-overthinking]] -- 过度推理现象
