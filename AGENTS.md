# LLM Wiki 维护指南

- 这个仓库是一个由 LLM agent 维护的个人 wiki。人的职责是挑选资料、提出问题、判断方向；agent 的职责是维护 wiki，包括摘要、归档、交叉引用、更新索引和记录日志。

- 对话与生成的文件必须使用中文，除了英文术语部分可以保留。

- 如果`wiki/`路径下没有index.md与log.md文件，提醒用户初始化wiki。 

## 三层结构

- `raw/`：原始资料层。这里保存不可变的来源材料。agent 可以读取，但除非用户明确要求，不要修改这里的源文件。
- `raw/<topic>/`：主题来源目录。多个主题并行时，每个主题都应该有自己的原始资料目录，例如 `raw/llm-wiki/`。
- `wiki/`：知识库层。这里保存由 agent 生成和维护的 markdown 页面。agent 可以创建、修订、交叉链接和重组这些页面。
- `wiki/topics/<topic>/`：主题知识目录。每个主题拥有自己的概览、来源摘要、概念、实体和问题归档。
- `wiki/topics/_shared/`：跨主题共享知识目录。只有确实跨多个主题复用的概念或实体才放在这里。
- `wiki/index.md`：全局内容索引。它只负责列出主题入口和重要跨主题页面，不替代每个主题自己的索引。
- `wiki/log.md`：时间线日志。每次 ingest、被归档的问题回答、lint 检查都要追加一条记录。

## 页面规范

- 内部链接使用 Obsidian 风格：`[[概念名称]]`。
- 页面内容要基于来源材料，避免无依据发挥。不确定的内容标记为“开放问题”，不要假装已经定论。
- 文件名尽量稳定，使用 kebab-case，并按类型分目录：
  - `wiki/topics/<topic>/sources/`：该主题的来源摘要，结构参考 `wiki/_templates/source.md` 。
  - `wiki/topics/<topic>/concepts/`：该主题的概念、方法、框架、主题，结构参考 `wiki/_templates/concept.md` 。
  - `wiki/topics/<topic>/entities/`：该主题的人物、组织、项目、工具、产品、地点，结构参考 `wiki/_templates/entity.md` 。
  - `wiki/topics/<topic>/questions/`：该主题中值得长期保留的问题回答、比较、分析，结构参考 `wiki/_templates/question.md` 。
- 每个主题包含`wiki/topics/<topic>/overview.md`作为整一个主题的大纲，结构参考 `wiki/_templates/topic-overview.md`。 
- 每个主题包含`wiki/topics/<topic>/_index.md` 作为主题内索引，结构参考 `wiki/_templates/topic-index.md`。 
- 全局内容索引`wiki/index.md`的结构参考： `wiki/_templates/index.md`。
- 不同主题之间可以互相链接，但不要因为名字相似就强行合并页面；先保持主题上下文清楚。
