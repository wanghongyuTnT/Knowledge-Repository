# 第六章：Eval、Trace 与 Metrics

## Eval 是什么

Eval 是用明确标准判断 Agent 是否真正完成任务，以及过程是否安全、稳定、可接受。

它的演化路径是：

~~~text
软件测试
→ 机器学习验证集和基准评测
→ 大模型人工 / 模型评分
→ Agent 的结果、轨迹、安全、成本评测
~~~

Eval 诞生的原因是：模型输出不是固定字符串，Agent 即使最终回答正确，也可能调用错误工具、越权、跳过审批或重复执行。

## ATK completed 为什么不等于 Eval Pass

~~~text
ATK completed
~~~

只能说明 ATK 认为自己的内部任务完成，不能证明：

- Runtime 状态正确；
- 没有调用错误工具；
- 没有修改范围外文件；
- 没有跳过错误；
- commonMain 没有平台依赖；
- Android 业务没有受到影响；
- Review 和验证已经通过。

## Eval 的三种方式

### 确定性检查

适合检查：

~~~text
文件是否越界
是否存在 Android import
是否存在旧依赖路径
重试次数是否超限
ATK 错误是否被处理
~~~

### LLM Judge

适合判断方案是否合理、逻辑是否完整、Review 解释是否充分。但不能替代权限和安全校验。

### 人工评审

适合判断 Android 业务行为、关键架构取舍和高风险改动。

## Hard Gate

以下情况应直接失败：

~~~text
修改范围外文件
commonMain 直接依赖 Android
跳过鉴权或 ATK 错误
绕过人工审批
重复执行高风险动作
~~~

## Trace

Trace 记录 Agent 做了什么：

~~~text
taskId
当前状态
Agent 名称
Prompt 版本
Tool 名称和参数
Tool 结果
错误和重试次数
耗时和成本
人工确认
文件 Diff
~~~

## Metrics

常见指标：

- Task Success Rate：任务成功率；
- Tool Error Rate：工具错误率；
- Review Defect Rate：Review 后仍发现问题的比例；
- Recovery Rate：中断后恢复成功率；
- Human Escalation Rate：人工介入比例；
- Latency：延迟；
- Token Cost：模型成本。
