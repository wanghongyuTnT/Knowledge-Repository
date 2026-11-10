# Codex 多 Agent 架构

## 核心结论

Codex 可以帮你搭建多 Agent，也可以在开发过程中运行子 Agent/并行 Agent，但如果你要做自己的业务系统运行时，通常还是需要一个编排层。

这个编排层可以用：

1. OpenAI Agents SDK
2. LangGraph
3. CrewAI
4. 纯 Python / TypeScript 自己写最小框架

## 关键判断

- 只是想让 AI 帮你开发一个多 Agent 项目：用 Codex 就可以。
- 想让 Codex 自己在开发环境里并行做任务：可以使用 Codex 的 subagents / 多 agent 协作能力。
- 想做一个用户可访问、可部署、可长期运行的业务系统：需要写代码或使用现成框架。
- 想最快抄一个成熟方案：优先 OpenAI Agents SDK 或 LangGraph。

## 推荐路线

### 入门 MVP

Planner Agent → Worker Agent → Critic Agent → Final Agent

### 推荐技术栈

- 新手：OpenAI Agents SDK
- 复杂流程：LangGraph
- 快速角色扮演型工作流：CrewAI
- 想完全可控：Python 自己写 Orchestrator

## 最小伪代码

```python
planner = Agent("Planner", instructions="拆任务")
worker = Agent("Worker", instructions="执行任务")
critic = Agent("Critic", instructions="审核结果")
final = Agent("Final", instructions="汇总输出")

plan = planner.run(user_input)
draft = worker.run(plan)
review = critic.run(draft)

if not review.pass:
    draft = worker.run(review.feedback)

result = final.run(draft)
```

## 实战建议

不要一开始搞 8 个 Agent。先做 3 个：

1. Planner：拆任务
2. Worker：执行
3. Critic：审核

跑通后再拆分专家 Agent。
