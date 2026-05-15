# AutoPaperReader

AutoPaperReader 是一个自动化「找论文 -> 读论文 -> 写报告」的 agent 项目。

它的目标是把一篇论文从检索、下载源码、分析论文、分析配套代码，到最终输出结构化中文阅读报告的流程串起来，并把产物（包括 Markdown 报告和 HTML 展示网页）统一组织到固定目录中。

**注：建议开启 Agent Teams 特性 `export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`**

## 项目目标

- 根据论文标题、方法名或 arXiv 链接定位目标论文
- 下载并解压 arXiv 源码
- 在发现公开代码时自动拉取代码仓库
- 派发子 agent 完成论文阅读和代码分析
- 在 `docs/` 中生成结构化中文报告

## 目录结构

```text
paper_reader/
├── README.md
├── LICENSE
├── CLAUDE.md
├── .claude/
│   └── agents/
│       ├── paper-analyst.md
│       └── code-analyst.md
├── docs/
│   └── .gitkeep
└── sources/
    └── .gitkeep
```

说明：

- `CLAUDE.md` 是主线 agent 的工作流说明
- `.claude/agents/` 存放子 agent 的执行规范
- `sources/` 用于存放单篇论文的源码与代码仓库
- `docs/` 用于存放论文阅读报告

## 工作流概览

1. 输入论文题目、方法名或 arXiv 链接
2. 主线 agent 定位论文并下载 arXiv 源码
3. 若存在公开代码，则一并拉取代码仓库
4. 派发 `paper-analyst` 生成论文阅读报告
5. 若存在代码，再派发 `code-analyst` 补充实现观察
6. 在 `docs/<slug>.md` 生成最终中文报告

## 产物约定

每篇论文使用统一 slug 命名：

```text
<arxiv_id>-<short-name>
```

例如：

```text
2310.06825-mistral-7b
1706.03762-attention
```

对应产物位置：

- `sources/<slug>/arxiv/`：论文源码
- `sources/<slug>/code/`：配套代码仓库
- `docs/<slug>.md`：阅读报告

## License

本项目使用 [MIT License](./LICENSE)。
