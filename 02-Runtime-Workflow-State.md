# 第二章：Runtime、Workflow 与 State

## Runtime 是什么

Runtime 可以理解为：

~~~text
任务调度器 + 状态机 + 工具执行器 + 异常处理器
~~~

它负责创建任务、选择下一步、调用模型和工具、保存状态、处理超时、暂停恢复、权限和终止。

## Workflow 与 Runtime 的区别

| 问题 | Workflow | Runtime |
|---|---|---|
| 未发货能否退款 | 定义规则 | 执行规则 |
| 下一步是什么 | 定义路径 | 调度执行 |
| 工具超时怎么办 | 定义分支 | 执行重试或暂停 |
| 用户确认期间怎么办 | 定义审批节点 | 保存状态并等待恢复 |

## Agent Loop

~~~text
观察
→ 决策
→ 行动
→ 获取结果
→ 更新状态
→ 再决策
~~~

固定流程优先使用 Workflow；只有步骤不确定、需要探索时，才让 LLM 动态规划。

## 迁移任务状态

~~~text
CREATED
→ INPUT_VALIDATED
→ ANALYZING
→ PLAN_READY
→ WAITING_APPROVAL
→ TRANSLATING
→ REVIEWING
→ COMPLETED
~~~

异常状态：

~~~text
AUTH_BLOCKED
RETRYING
REVIEW_FAILED
HUMAN_INTERVENTION
~~~

## 重要区别

业务状态和执行状态不要混在一起：

~~~text
业务状态：未发货、已发货、退款中、已退款
执行状态：查询超时、鉴权失败、结果未知
~~~

操作失败和结果未知也不同。结果未知时必须先查询当前状态，不能直接重复副作用操作。

## Runtime 的终止条件

- 最大步骤数；
- 最大执行时间；
- 最大重试次数；
- 最大成本；
- 用户拒绝；
- 鉴权失败；
- Review 失败且无法自动修复；
- 任务成功。
