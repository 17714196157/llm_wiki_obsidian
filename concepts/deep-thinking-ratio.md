---
title: Deep-Thinking Ratio (DTR)
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [inference, architecture]
sources: [raw/articles/2602-13517-think-deep-not-just-long.md]
---

# Deep-Thinking Ratio (DTR)

## 定义

**Deep-Thinking Ratio (DTR)** 是一种衡量大语言模型推理时 "思考努力" (thinking effort) 的指标。它通过识别 **deep-thinking tokens** -- 即在更深层中内部预测在收敛前经历显著修正的 token -- 来量化推理过程中的计算努力程度。

DTR 定义为生成序列中 deep-thinking tokens 的比例。

## 核心发现

来源：[[think-deep-not-just-long]] (Chen et al., 2026)

| 指标 | 与准确率的相关系数 | 含义 |
|------|-------------------|------|
| 输出 token 数 (长度) | r = -0.544 | 负相关，越长表现越差 |
| Deep-Thinking Ratio | r = +0.828 | 强正相关，越高表现越好 |
| 置信度基线 | 显著低于 DTR | 不如 DTR 可靠 |

测试范围：
- 4 个基准：AIME 2024/2025, HMMT 2025, GPQA-diamond
- 3 个模型族：[[gpt-oss]], [[deepseek-r1]], [[qwen3]]

## 计算方法

1. **前向传播**：对于自回归语言模型的第 t 个生成步骤，产生 L 层的残差流状态 {h_{t,l}}_{l=1}^{L}
2. **投影到词汇空间**：将每层的隐藏状态通过未绑定的 LM head 投影到词汇空间，得到每层的预测分布
3. **Jensen-Shannon 散度 (JSD)**：计算每层分布与最终层分布之间的 JSD
4. **收敛层判定**：找到从哪一层开始，后续所有层的 JSD 都低于阈值 g（论文中 g=0.5）
5. **Deep-thinking 判定**：如果收敛层在深度分数 ρ = 0.85 之后的层中，则该 token 为 deep-thinking token
6. **DTR 计算**：DTR(S) = (1/T) × Σ I[c_t ∈ L_deep-thinking]

### 超参数

| 参数 | 默认值 | 含义 |
|------|--------|------|
| g (settling threshold) | 0.5 | JSD 收敛阈值 |
| ρ (depth fraction) | 0.85 | deep-thinking 深度分数 |

## 与长度指标的对比

传统的 test-time compute 代理指标（输出 token 数）与准确率呈**负相关**，表明长推理轨迹可能反映的是冗余、误导或错误放大的思考（[[llm-overthinking]]）。DTR 与此相反，提供了一种**机制上可解释**的思考努力度量。

## 应用：Think@n

基于 DTR 的 test-time scaling 策略 [[think-at-n]]：
- 并行生成多个样本
- 从短前缀估计 DTR
- 提前终止低 DTR 的生成
- 聚合高 DTR 样本
- 在达到与 self-consistency 相当或更优的性能的同时，减少约 50% 的推理成本

## 关键观察

从论文的热力图分析中：
- 功能性/模板词（如 "and", "is", "boxed"）通常在较浅层收敛
- 运算符后的补全（如 "+", "="）和答案 token（如 "13", "(D)"）需要更深层才收敛
- 答案 token 在首次出现后，会在较浅层逐渐 "浮现"

## 开放问题

- DTR 是否适用于非推理任务（创意写作、翻译）？
- 不同模型架构（MoE, RNN）中 DTR 的行为如何？
- DTR 能否用于训练时的奖励信号？

## 相关概念

- [[llm-overthinking]] -- 过度推理现象
- [[test-time-scaling]] -- 推理时计算扩展
- [[think-at-n]] -- 基于 DTR 的采样策略
