---
name: ingest
description: 将原始来源文件摄入这个项目的多主题 LLM wiki。当用户要求处理、导入、消化、摘要或整合 raw 下某个主题目录中的新材料，并写入 wiki/topics 下对应主题目录时使用；包括创建摘要(overview.md)、主题内的索引(_index.md)、来源(sources)、更新概念(concepts)/实体(entities)/问题页面(questions)，同时刷新全局索引(wiki/index.md)，并按照 AGENTS.md 追加日志。
---

# ingest

## 概览

使用这个 skill 将原始来源转化为持续维护的 wiki 页面。遵守 `AGENTS.md` 中的项目规则：raw 文件是来源材料，`wiki/topics/<topic>/` 是被维护的知识层，`wiki/index.md` 是全局索引，`wiki/log.md` 是只追加的活动时间线。

## 工作流

### 1. 识别来源和主题

- 如果用户指定了来源文件，在 `raw/` 下定位它。
- 如果来源文件还不在 `raw/<topic>/` 中，就根据用户请求或文件上下文推断主题。只有在主题确实含糊时才询问用户。
- 如果主题不存在，创建：
  - `raw/<topic>/`
  - `wiki/topics/<topic>/sources/`
  - `wiki/topics/<topic>/concepts/`
  - `wiki/topics/<topic>/entities/`
  - `wiki/topics/<topic>/questions/`
  - `wiki/topics/<topic>/overview.md`
  - `wiki/topics/<topic>/_index.md`

### 2. 阅读现有上下文

- 阅读 `AGENTS.md`。
- 阅读 `wiki/index.md`。
- 如果主题的 `overview.md` 和 `_index.md` 已存在，阅读它们。
- 使用 `rg` 搜索主题目录，查找相关概念、实体、重复术语或既有摘要。
- 在写入更新前，阅读相关的现有页面。

### 3. 创建或更新来源摘要

在 `wiki/topics/<topic>/sources/` 中创建或更新页面。内容包括：

- 来源路径，以及原始 URL（如果有）。
- 简洁摘要。
- 关键主张。
- 提取出的概念。
- 提取出的实体。
- 矛盾或张力。
- 开放问题。

所有主张都必须扎根于来源材料。如果来源不清楚，要明确说明。

### 4. 整合进被维护的 wiki

只创建或更新真正有用的页面：

- `wiki/topics/<topic>/concepts/`：用于可复用的想法、方法、框架或反复出现的概念。
- `wiki/topics/<topic>/entities/`：用于人物、组织、项目、工具、产品或地点。
- `wiki/topics/_shared/`：仅当某个概念或实体明确跨多个主题适用时使用。

优先更新现有页面，而不是创建近似重复页面。在相关页面之间添加 Obsidian 链接，例如 `[[页面标题]]`。

### 5. 更新索引和概览

- 当主题被创建、重命名，或出现重要入口页面时，更新 `wiki/index.md`。
- 每当新增或大幅修订来源、概念、实体或问题页面时，更新 `wiki/topics/<topic>/_index.md`。
- 当本次摄入改变了主题的当前理解、范围、关键结论或开放问题时，更新 `wiki/topics/<topic>/overview.md`。

### 6. 追加日志

向 `wiki/log.md` 追加一条带日期的记录：

```markdown
## [YYYY-MM-DD] ingest | Source title

- 主题：`<topic>`
- 来源：`raw/<topic>/<file>`
- 更新：页面链接或路径
- 备注：关键整合决策、矛盾或开放问题
```

保持 `wiki/log.md` 只追加：按照现有时间顺序风格，在旧记录上方或合适位置添加新记录；除非是在修正明显笔误，否则不要改写历史事实。


## 最终回复

摄入完成后，报告：

- 已处理的来源。
- 使用的主题。
- 新创建的页面。
- 已更新的现有页面。
- 仍未解决的问题或矛盾。
