# Agent 工程师能力体系

## 核心判断
成熟的 Agent 工程师不是只会调 API 或套 LangChain，而是能把 LLM 变成一个可靠、可控、可评估、可上线的自动化系统。

一句话：

> Agent 工程师 = LLM 能力理解 + 工具调用 + 工作流编排 + 状态管理 + 评估体系 + 工程化部署 + 安全治理。

---

## 1. LLM 基础能力

需要理解：

- Transformer 和上下文窗口的基本原理
- Token、上下文长度、成本、延迟
- Prompt Engineering
- System Prompt / Developer Prompt / User Prompt 的职责边界
- Few-shot、Chain-of-Thought、ReAct、Self-Reflection 等模式
- 模型幻觉、指令冲突、上下文污染
- 不同模型的能力边界：推理、代码、工具调用、长上下文、多模态

重点不是会背论文，而是知道模型在哪些场景容易失败。

---

## 2. Agent 基础架构

必须掌握 Agent 的组成：

- Goal：目标
- State：状态
- Memory：记忆
- Tools：工具
- Planner：规划器
- Executor：执行器
- Evaluator：评估器
- Stop Condition：停止条件

一个成熟 Agent 通常不是一个 prompt，而是一个带控制逻辑的系统。

---

## 3. Loop Agent / ReAct / Reflection

必须理解这些核心执行模式：

### ReAct

Reason + Act：

```text
思考 → 调工具 → 观察结果 → 再思考
```

### Loop Agent

```text
目标 → 观察 → 规划 → 执行 → 验证 → 复盘 → 下一轮
```

### Reflection Agent

```text
生成答案 → 自我检查 → 修正答案
```

### Plan-and-Execute

```text
先拆计划 → 再逐步执行 → 每步验证
```

成熟工程师要知道什么时候用哪种模式。

---

## 4. 工具调用 Tool Use

Agent 真正有用，靠的是工具。

需要掌握：

- Function Calling / Tool Calling
- 参数 schema 设计
- 工具权限控制
- 工具失败重试
- 工具结果压缩
- 工具选择策略
- 工具调用日志
- 幂等性设计
- 外部 API 调用
- 文件读写、代码执行、浏览器操作、数据库查询

工具不是越多越好，而是要可控、可追踪、可恢复。

---

## 5. 工作流编排 Orchestration

这是 Agent 工程的核心。

需要掌握：

- 串行流程
- 并行流程
- 条件分支
- 循环重试
- 人工确认 Human-in-the-loop
- Supervisor / Worker 架构
- 多 Agent 协作
- 状态机
- DAG 工作流
- 失败回滚

真正上线的 Agent 通常不是完全自由行动，而是“有限自主 + 明确边界”。

---

## 6. RAG 和知识库

企业 Agent 大量依赖 RAG。

需要掌握：

- 文档切分 chunking
- embedding
- 向量数据库
- 关键词检索 BM25
- Hybrid Search
- Rerank
- Metadata filtering
- Query rewrite
- Context compression
- 引用溯源
- 权限隔离
- 知识新旧版本管理

成熟 Agent 工程师必须知道：RAG 失败往往不是模型问题，而是检索、切分、排序和权限问题。

---

## 7. Memory 状态与记忆

需要区分：

- Short-term memory：当前任务状态
- Long-term memory：长期用户偏好或业务知识
- Episodic memory：历史任务记录
- Semantic memory：结构化知识
- Working memory：当前推理所需信息

还要掌握：

- 什么时候写入记忆
- 什么时候读取记忆
- 如何防止错误记忆污染后续任务
- 如何做记忆过期、删除、更新

---

## 8. 评估 Evaluation

不会评估，就不能上线。

需要掌握：

- 单轮问答评估
- 多轮任务完成率
- Tool call 成功率
- 任务完成时间
- 成本统计
- 幻觉率
- 引用准确率
- 回归测试集
- LLM-as-Judge
- 人工标注评估
- A/B 测试
- Golden Dataset

对编程 Agent，还要关注：

- 单元测试是否通过
- lint 是否通过
- patch 是否最小化
- 是否引入新 bug
- 是否误改无关文件

---

## 9. Observability 可观测性

Agent 不可观测，就不可维护。

需要掌握：

- prompt 日志
- tool call 日志
- token 消耗
- latency
- trace 链路追踪
- 每轮 state 快照
- 错误分类
- 重试记录
- 用户反馈闭环

生产级 Agent 一定要能回答：

```text
它为什么这么做？
它调用了什么工具？
哪一步失败了？
这次花了多少钱？
是否可以复现？
```

---

## 10. 安全与权限

Agent 比普通应用更危险，因为它会行动。

需要掌握：

- Prompt Injection
- Tool Injection
- Data Exfiltration
- 越权访问
- 敏感信息泄露
- 权限最小化
- 沙箱执行
- 人工审批
- 审计日志
- 速率限制
- 预算限制
- 输出安全过滤

尤其是能发邮件、转账、删文件、改数据库的 Agent，必须有权限边界。

---

## 11. 软件工程能力

成熟 Agent 工程师首先还是工程师。

需要掌握：

- Python / TypeScript 至少一门熟练
- API 设计
- 异步任务队列
- 数据库
- 缓存
- Docker
- CI/CD
- 测试
- 日志
- 配置管理
- 错误处理
- 服务部署
- 云服务基础

不要只会写 prompt。Agent 的难点通常在系统工程。

---

## 12. 常见框架

需要了解但不要迷信框架：

- LangGraph：适合状态机、复杂流程、可控 Agent 编排
- LangChain：生态丰富，适合 RAG 和工具链集成
- OpenAI Agents SDK：适合基于 OpenAI 工具调用、handoff、tracing 的 Agent 应用
- CrewAI：适合角色分工明显的多 Agent 协作
- Microsoft Agent Framework / Semantic Kernel：适合微软生态、企业级编排
- LlamaIndex：适合知识库和 RAG 场景
- AutoGen：早期多 Agent 框架，需注意其维护状态和迁移方向

框架只是工具，核心是架构能力。

---

## 13. 产品与业务理解

Agent 不是炫技，而是解决业务问题。

需要掌握：

- 需求拆解
- ROI 计算
- 自动化边界判断
- 哪些环节必须人工确认
- 用户体验设计
- 失败兜底
- 任务交接
- 可解释性
- 成本收益比

很多 Agent 项目失败，不是模型不行，而是业务流程没拆清楚。

---

## 14. 推荐学习路线

### 第一阶段：单 Agent

- Prompt
- Tool Calling
- 简单 RAG
- 简单状态管理

### 第二阶段：Loop Agent

- ReAct
- Plan-and-Execute
- Evaluator
- Stop Condition
- 错误重试

### 第三阶段：工程化

- 日志
- Trace
- 成本控制
- 测试集
- 权限控制
- 部署

### 第四阶段：多 Agent

- Supervisor / Worker
- Planner / Executor / Reviewer
- 人工确认点
- 任务队列
- 多工具协作

### 第五阶段：生产级 Agent

- 可观测性
- 安全审计
- 回归评估
- 灰度发布
- 监控告警
- 长期维护

---

## 15. 成熟度标准

### 初级

会写 prompt，会调 API，会做 demo。

### 中级

能做工具调用、RAG、简单 Agent 工作流。

### 高级

能做 Loop Agent、多 Agent、评估、日志、权限、部署。

### 成熟 Agent 工程师

能把不稳定的 LLM 能力封装成稳定、可控、可评估、可维护的业务系统。

---

## 结论

成熟 Agent 工程师应该掌握的不是某一个框架，而是一套完整能力：

```text
模型理解
+ Prompt
+ 工具调用
+ 状态管理
+ 工作流编排
+ RAG
+ 评估体系
+ 可观测性
+ 安全权限
+ 工程部署
+ 业务流程设计
```

真正的分水岭是：

> 你不是让模型“看起来聪明”，而是让它在真实环境里稳定完成任务。
