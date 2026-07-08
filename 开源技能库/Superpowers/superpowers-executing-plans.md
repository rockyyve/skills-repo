# executing-plans (Superpowers)

Superpowers 的计划执行 skill。按照 writing-plans 生成的计划**分批执行**，不一次性跑完。

## 核心机制

- 每批执行若干任务后**暂停**，等待人工确认
- 人工检查点：验证当前批次结果符合预期
- 确认后继续下一批
- 发现问题时在检查点修正，不是等全部跑完再回头

## 为什么分批

一次性执行所有任务的风险：早期的错误假设会被后续任务放大，等发现时已经有大量需要回滚的工作。分批执行把错误控制在最小范围。

## 在 Superpowers 流程中的位置

```
writing-plans → [executing-plans] → requesting-code-review
 ↑
 每批后人工检查点

```


## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-writing-plans — 上一步：生成计划
- skills/superpowers-requesting-code-review — 下一步：代码审查
- skills/superpowers-verification-before-completion — 执行中的验证原则
