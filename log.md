# Wiki Log

> 所有 wiki 操作的时序记录。只追加，不修改。
> 格式：`## [YYYY-MM-DD] action | subject`
> 操作类型：ingest, update, query, lint, create, archive, delete
> 当文件超过 500 条时，重命名为 log-YYYY.md 并重新开始。

## [2026-04-23] create | Wiki 初始化
- 领域：大语言模型训练与应用
- 创建结构：SCHEMA.md, index.md, log.md
- 目录：raw/{articles,papers,transcripts,assets}, entities, concepts, comparisons, queries

## [2026-04-23] ingest | Think Deep, Not Just Long (arXiv 2602.13517)
- 来源：https://arxiv.org/pdf/2602.13517
- 保存原文：raw/articles/2602-13517-think-deep-not-just-long.md, raw/articles/2602-13517-think-deep-not-just-long.html
- 创建实体页：entities/gpt-oss.md, entities/deepseek-r1.md, entities/qwen3.md
- 创建概念页：concepts/deep-thinking-ratio.md, concepts/llm-overthinking.md, concepts/test-time-scaling.md, concepts/think-at-n.md, concepts/chain-of-thought.md
- 更新：index.md (7 页面)
