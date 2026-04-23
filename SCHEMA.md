# Wiki Schema

## Domain
大语言模型（LLM）训练与应用 — 涵盖模型架构、预训练、微调、对齐、推理优化、部署、评测、Agent 应用等全链路知识。

## Conventions
- 文件名：小写英文，连字符分隔，无空格（如 `transformer-architecture.md`）
- 每个 wiki 页面必须以 YAML frontmatter 开头（见下方格式）
- 使用 `[[wikilinks]]` 在页面间互链（每页至少 2 个出站链接）
- 更新页面时必须 bump `updated` 日期
- 新页面必须加入 `index.md` 对应分类，按字母序排列
- 每个操作必须追加到 `log.md`

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
---
```

## Tag Taxonomy

### 模型与架构
- `architecture` - 模型架构设计（Transformer, MoE, RWKV 等）
- `model` - 具体模型实体（Llama, Qwen, Claude 等）

### 训练
- `pretraining` - 预训练（数据、策略、算力）
- `fine-tuning` - 微调（SFT, LoRA, QLoRA 等）
- `alignment` - 对齐（RLHF, DPO, ORPO 等）
- `data` - 训练数据（采集、清洗、合成）

### 推理与部署
- `inference` - 推理优化（KV cache, speculative decoding 等）
- `quantization` - 量化（GPTQ, AWQ, GGUF 等）
- `serving` - 部署服务（vLLM, TGI, TensorRT-LLM 等）
- `edge` - 端侧部署与优化

### 应用
- `agent` - Agent 框架与应用
- `rag` - 检索增强生成
- `tool-use` - 工具调用 / Function Calling
- `multimodal` - 多模态（视觉、语音、视频）

### 评测与安全
- `benchmark` - 评测基准与方法
- `safety` - 安全、红队、护栏
- `evaluation` - 评估方法学

### 组织与生态
- `company` - 公司/实验室
- `open-source` - 开源项目/社区
- `paper` - 重要论文

### 元标签
- `comparison` - 对比分析
- `timeline` - 时间线/里程碑
- `tutorial` - 教程/指南

规则：页面上使用的每个标签必须出现在本分类体系中。如需新增标签，先在此处添加再使用，防止标签泛滥。

## Page Thresholds
- **创建页面**：当实体/概念在 2+ 来源中出现，或是一个来源的核心主题
- **更新已有页面**：当来源提及已有内容时追加信息
- **不创建页面**：路过提及、次要细节、或不在领域范围内的内容
- **拆分页面**：当超过 ~200 行时，拆分为子主题并互链
- **归档页面**：当内容被完全取代时，移至 `_archive/`，从 index 中移除

## Entity Pages
每个重要实体一个页面，包括：
- 概述 / 是什么
- 关键事实和数据
- 与其他实体的关系（[[wikilinks]]）
- 来源引用

## Concept Pages
每个概念/主题一个页面，包括：
- 定义 / 解释
- 当前知识状态
- 开放问题或争议
- 相关概念（[[wikilinks]]）

## Comparison Pages
对比分析，包括：
- 对比对象和原因
- 对比维度（优先使用表格）
- 结论或综合判断
- 来源

## Update Policy
当新信息与已有内容冲突时：
1. 检查日期 — 更新的来源通常取代旧的
2. 如果真正矛盾，标注两种立场及日期和来源
3. 在 frontmatter 中标记：`contradictions: [page-name]`
4. 在 lint 报告中标记供用户审查
