# Wiki-Picadcl

这是一个[karpathy](https://gist.github.com/442a6bf555914893e9891c11519de94f.git)提出的想法：由 LLM agent 维护的个人 wiki 仓库。

人的职责是挑选资料、提出问题和判断方向；agent 的职责是把资料摄入 wiki，维护摘要、概念、实体、问题归档、交叉链接、索引和日志。

## 目录

- `raw/`：原始资料层。保存不可变来源材料，按主题放在 `raw/<topic>/`。
- `wiki/`：知识库层。保存由 agent 生成和维护的 Markdown 页面。
- `wiki/topics/<topic>/`：单个主题的知识目录，包含 `overview.md`、`_index.md`、`sources/`、`concepts/`、`entities/`、`questions/`。
- `wiki/topics/_shared/`：跨主题共享概念或实体。
- `wiki/index.md`：全局内容索引。
- `wiki/log.md`：维护时间线日志。

## 工作流

- `ingest`：把 `raw/<topic>/` 中的新材料整理进 wiki。
- `query`：基于已有 wiki 回答问题，必要时把长期有价值的回答归档。
- `lint`：检查孤立页面、缺失索引、重复概念、缺少来源和陈旧说法。

具体维护规则见 `AGENTS.md`。

## Quick Start

- 先运行`/wiki-init`skill，来创建`log.md`与`index.md`文件。
- `tests/llm-wiki`可以作为第一份进入`raw/<topic>`的测试文件，将其移动到`raw`中即可。
- 运行`/ingest`skill，agent将整理出wiki。
- 运行`/query`skill，可以与agent进行讨论，提问。
