# LLM Wiki Manager

This project was inspired by the **LLM Wiki** concept proposed by Andrej Karpathy.

It is a general-purpose **Agent Skill** designed for AI Agent systems (such as OpenClaw, OpenCode, Claude Code, etc.), used to build and manage "knowledge bases compiled by LLMs." It automatically synthesizes scattered raw materials into a structured Wiki knowledge base and provides deep research, auditing, and question-answering capabilities with citations.

> **Note**: This project is currently deeply optimized for the **OpenClaw** environment by default.

## Core Features

*   **Automated Knowledge Ingestion**: Automatically crawls information from URLs, local files, or codebases and stores them in an immutable `raw/` directory.
*   **Intelligent Compilation**: The Agent acts as a "compiler," summarizing, connecting, and synthesizing raw materials into structured Markdown knowledge articles, supporting dual-linking (Obsidian compatible).
*   **Deep Research**: Supports multi-round automated retrieval and synthesis, with specific modes for "Research" and "Thesis."
*   **Knowledge Auditing**: Traces knowledge provenance, evaluates confidence levels, and tracks volatility to prevent model hallucinations.
*   **Session Management**: Automatically captures "Lessons Learned" from Agent debugging sessions and persists them as knowledge.

## Installation

This Skill uses a portable instruction specification file (`SKILL.md`). The installation method is consistent with how your Agent system installs third-party Skills.

### 1. Load Instruction File

In your Agent system configuration, reference the path to `SKILL.md` in this repository.

For example, in **OpenClaw's** `openclaw.json`:

```json
{
  "instructions": [
    "/path/to/llm-wiki-manager/SKILL.md"
  ]
}
```

### 2. Authorize Access

Since Wiki data is usually stored outside the project (e.g., `~/wiki/`), ensure your Agent has read/write permissions for the relevant directories.

## Quick Start

Once the Skill is loaded, you can trigger features using natural language dialogue with your Agent:

*   **Initialize Wiki**: "Help me initialize a Wiki about [Quantum Computing]."
*   **Ingest Information**: "Save this webpage to my Wiki: https://example.com/article"
*   **Execute Compilation**: "Compile the current Wiki and synthesize new articles from ingested data."
*   **Deep Research**: "Conduct deep research on [Kubernetes security policies] and save the results to the Wiki."
*   **Knowledge Q&A**: "What is the conclusion regarding X in my Wiki? Please provide citation links."

## Wiki Directory Structure

The system uses a **Hub-Topic** architecture to manage multiple knowledge bases.

### Hub (HUB/)
The hub tracks all topic wikis and manages global state. Its location is defined in `~/.config/llm-wiki/config.json`.

```text
HUB/
├── wikis.json                     # Registry of all topic wikis
├── _index.md                      # Global index and statistics
├── log.md                         # Global activity log
└── topics/                        # Dedicated folders for each topic
    ├── research-topic-a/          # A topic-specific wiki
    └── .archive/                  # Hidden archive for old topics
```

### Topic Wiki (HUB/topics/<name>/)
Each topic is a self-contained wiki with its own sources, articles, and logs.

```text
HUB/topics/<name>/
├── config.md                      # Title, scope, and custom instructions
├── _index.md                      # Topic-level master index
├── log.md                         # Detailed activity log for this topic
├── raw/                           # Immutable source material (articles, papers, etc.)
├── wiki/                          # Compiled articles (concepts, topics, references)
├── inventory/                     # (Optional) Tracking for items, entities, and tasks
├── datasets/                      # (Optional) Manifests for external large data
└── output/                        # Generated reports, summaries, and projects
```

## Technical Implementation

*   **Core Instructions**: `SKILL.md` defines the complete logic protocol for the Agent to handle the Wiki.
*   **Helper Scripts**:
    *   `scripts/llm-wiki`: A Python-driven deterministic helper tool responsible for structure validation, index rebuilding, and consistency checks.
    *   `scripts/llm-wiki-session`: Responsible for capturing and reconstructing session context and knowledge extraction.

## License

[MIT License](LICENSE)
