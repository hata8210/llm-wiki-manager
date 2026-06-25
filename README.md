# LLM Wiki Manager

本项目深受 Andrej Karpathy 提出的 **LLM Wiki** 概念启发开发而成。

这是一个通用的 **Agent Skill**，专为 AI Agent 系统（如 OpenClaw, OpenCode, Claude Code 等）设计，用于构建和管理“由 LLM 编译的知识库”。它能将零散的原始素材自动合成为结构化的 Wiki 知识库，并提供深度研究、审计和问答能力。

> **注意**：本项目目前默认针对 **OpenClaw** 环境进行了深度优化。

## 核心功能

*   **自动化知识摄取 (Ingestion)**：自动从 URL、本地文件或代码库抓取信息并存入 `raw/` 目录。
*   **智能编译 (Compilation)**：由 Agent 充当“编译器”，将原始素材合成为结构化的 Markdown 知识文章，支持双向链接 (Obsidian 兼容)。
*   **深度研究 (Research)**：多轮次自动检索与总结，支持课题研究 (Research) 与论文 (Thesis) 模式。
*   **知识审计 (Audit)**：追溯知识来源，评估信心指数 (Confidence) 与时效性 (Volatility)，防止模型幻觉。
*   **会话管理 (Session)**：自动捕获 Agent 调试过程中的经验 (Lessons Learned) 并将其持久化为知识。

## 安装方法

本 Skill 采用通用的指令规范文件 (`SKILL.md`)。安装方法与你的 Agent 系统安装第三方 Skill 的标准方式保持一致。

### 1. 加载指令文件

在你的 Agent 系统配置中，引用本仓库的 `SKILL.md` 路径。

例如在 **OpenClaw** 的 `openclaw.json` 中：

```json
{
  "instructions": [
    "/path/to/llm-wiki-manager/SKILL.md"
  ]
}
```

### 2. 授权访问

由于 Wiki 数据通常存储在项目外部（如 `~/wiki/`），请确保为你的 Agent 开启相应的目录读写权限。

## 快速上手

加载 Skill 后，你可以直接用自然语言与 Agent 对话触发功能：

*   **初始化**：“帮我初始化一个关于 [量子计算] 的 Wiki”
*   **摄取信息**：“把这个网页存入我的 Wiki：https://example.com/article”
*   **执行编译**：“编译当前的 Wiki，生成最新的文章”
*   **深度研究**：“针对 [Kubernetes 安全策略] 进行深度研究，并将结果存入 Wiki”
*   **知识问答**：“我的 Wiki 里关于 X 的结论是什么？请标出引用来源”


## 目录结构

系统采用 **Hub-Topic** 架构，默认存储在 `~/wiki/`：

*   `HUB/wikis.json`：Wiki 注册表。
*   `HUB/topics/<name>/raw/`：不可变的原始素材。
*   `HUB/topics/<name>/wiki/`：由 Agent 编译生成的知识文章。
*   `HUB/topics/<name>/config.md`：存放你的“干预指令”和个性化配置。

## 开发与同步

本项目通过脚本维护与上游同步，请勿手动修改 `references/` 下的文件：
*   `scripts/llm-wiki`：Python 辅助脚本，负责索引构建与结构校验。
*   `scripts/llm-wiki-session`：负责会话捕获与上下文重构。

## 许可

[MIT License](LICENSE)
