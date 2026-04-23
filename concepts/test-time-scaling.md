---
title: Test-Time Scaling (推理时计算扩展)
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [inference]
sources: [raw/articles/2602-13517-think-deep-not-just-long.md]
---

# Test-Time Scaling (推理时计算扩展)

## 定义

**Test-Time Scaling** 指在推理阶段增加计算资源以提升模型性能的策略，与训练时的 scaling（增加模型大小、训练数据）相对。核心思想是：通过让模型在推理时 "思考更多" 来获得更好的结果。

## 主要范式

### 1. Long CoT (长链式推理)
- 鼓励模型生成更长的思考轨迹
- OpenAI o1/o3, DeepSeek-R1, Claude 等均采用此策略
- **问题**：存在 [[llm-overthinking]] 风险，长度与性能非正相关

### 2. 重复采样 (Repeated Sampling)
- 生成多个独立响应，通过投票/聚合选择最终答案
- 标准方法：**Self-Consistency** (多数投票)
- **问题**：所有样本计算成本相同，浪费在 "无希望" 的样本上

### 3. 自适应推理 (Adaptive Reasoning)
- 根据问题难度或中间信号动态调整推理策略
- **Think@n** -- 基于 [[deep-thinking-ratio]] 的早期终止策略
- 优点：在达到相当性能的同时减少约 50% 推理成本

## Think@n 策略

来源：[[think-deep-not-just-long]] (Chen et al., 2026)

```
1. 并行生成 n 个样本
2. 从短前缀估计每个样本的 DTR
3. 终止低 DTR 的样本（早期拒绝）
4. 聚合剩余高 DTR 样本的答案
```

**效果**：匹配或超越标准 self-consistency，计算成本减半。

## 对比：Test-Time vs Train-Time Scaling

| 维度 | Train-Time Scaling | Test-Time Scaling |
|------|-------------------|-------------------|
| 计算时机 | 训练阶段 | 推理阶段 |
| 资源需求 | 极高（数千 GPU） | 较低（灵活调整） |
| 效果 | 通用能力提升 | 特定任务提升 |
| 成本回收 | 一次训练，多次推理 | 每次推理都消耗 |

## 开放问题

- Test-time scaling 的 "最优预算" 如何确定？
- 不同任务类型（数学推理、代码生成、创意写作）的最优策略是否不同？
- 如何在不增加延迟的情况下实现 test-time scaling？

## 相关概念

- [[chain-of-thought]] -- CoT 推理范式
- [[deep-thinking-ratio]] -- 推理努力的度量
- [[llm-overthinking]] -- 过度推理现象
- [[think-at-n]] -- 基于 DTR 的采样策略
