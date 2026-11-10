# 第五章：MCP 与 Multi-Agent

## MCP

MCP 是连接 Agent 与外部工具、数据的通用协议：

~~~text
Host
└── MCP Client
    └── MCP Server
        └── API、数据库、文件或代码仓库
~~~

~~~text
Tool：具体能力
MCP：发现和调用能力的协议
Agent：决定什么时候使用能力
Runtime：可靠执行和管理状态
~~~

MCP Server 不一定包含 LLM，可以只是确定性的工具服务。

如果 Codex 直接执行 atk-agent 命令，从外层看是 Tool 调用；如果 ATK 暴露 MCP Server，则可以通过 MCP 调用 ATK 能力。

## Multi-Agent

多 Agent 适合任务可以拆成专业、相对独立的部分，或需要并行和独立审查：

~~~text
Codex Supervisor
├── Plan Agent
├── Execute Agent
└── Review Agent
~~~

在 creativex.lib-api 场景中，若 ATK 内部已经负责规划，不要再重复建设相同的 Plan Agent。

## 多 Agent 的代价

- 更多模型调用和 Token 成本；
- 更长延迟；
- 上下文传递复杂；
- 结果冲突；
- 文件写入冲突；
- Trace 和排错困难。

能用单 Agent + Workflow 解决的问题，不要先使用多 Agent。
