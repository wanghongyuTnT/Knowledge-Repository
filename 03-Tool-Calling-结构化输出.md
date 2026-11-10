# 第三章：Tool Calling 与结构化输出

## Tool Calling

Tool Calling 的真实含义是：

~~~text
LLM 输出工具名称和参数
→ Runtime 校验
→ Tool 调用真实 API
→ 返回结构化结果
→ Workflow 决定下一步
~~~

模型不会直接访问数据库，也不应该直接持有高风险权限。

## 工具应当小而明确

~~~text
query_order_status(orderId)
cancel_order(orderId, reason, idempotencyKey)
query_refund_status(orderId)
create_refund(orderId, idempotencyKey)
~~~

不要把查询、取消、退款合并成一个万能工具。查询是只读操作，取消和退款有副作用，应该拥有不同权限和审批策略。

## 参数边界

模型可以从用户话术中提取候选订单号，但以下内容必须由业务系统获取或校验：

- 用户身份；
- 订单是否存在；
- 订单归属；
- 订单状态；
- 退款金额；
- 用户权限。

模型负责提取，不负责证明；模型负责建议，不负责决定真实业务状态。

## 结构化输出

~~~json
{
  "intent": "REFUND_ORDER",
  "orderId": "123"
}
~~~

需要两层校验：

1. 格式校验：字段、类型、枚举、必填项；
2. 业务校验：归属、权限、状态、金额和操作条件。

JSON 合法不等于业务安全。

## Tool 输出

推荐：

~~~json
{
  "success": false,
  "errorCode": "AUTH_401",
  "retryable": false,
  "nextAction": "HUMAN_INTERVENTION"
}
~~~

Runtime 应根据结构化状态处理：

~~~text
RETRYABLE_TIMEOUT → 检查幂等性后有限重试
AUTH_401 → 暂停并修复权限
FATAL_ERROR → 停止并人工处理
TRANSLATED → 进入 Review
~~~
