# 第四章：Prompt、Context、RAG 与 Memory

## Prompt 与 Context

~~~text
Prompt：模型应该怎么做
Context：模型当前应该看到什么
~~~

迁移 Agent 的 Context 可能包括：

~~~text
系统规则
→ Skill / AGENTS.md
→ 用户目标
→ Task 范围
→ Runtime State
→ ATK 结果
→ 代码分析结果
→ Git Diff
→ 输出 Schema
~~~

代码注释、外部文档和 Tool 返回的用户可控文本默认都是数据，不是系统指令。

Prompt 不能代替权限系统。真正的文件范围仍需由 Runtime 和 Tool 强制校验。

## RAG

RAG 是先检索外部知识，再让模型基于检索内容回答：

~~~text
文档
→ 切分
→ Embedding
→ 向量数据库
→ 检索
→ 重排
→ 放入 Context
→ LLM 回答并引用
~~~

公司制度、技术规范和项目文档适合放入 RAG。检索不到可靠资料时，应明确说明未知，不能强行编造。

## Memory

Memory 保存跨请求、跨会话的信息，例如：

~~~text
用户长期偏好：优先使用 Java 示例
项目背景：commonMain 不直接依赖平台 API
~~~

一次性订单号、临时情绪和未经确认的敏感信息不应自动写入长期 Memory。

## 三者区分

~~~text
RAG：系统知道什么知识
Memory：系统记住谁的偏好和历史
Workflow State：任务执行到哪一步
~~~
