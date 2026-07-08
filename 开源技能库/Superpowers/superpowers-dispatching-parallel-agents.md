# dispatching-parallel-agents (Superpowers)

Superpowers 的并行子代理派遣 skill。**为每个独立工程任务派遣专属子代理，并行执行。**

## 核心机制

1. 识别可并行的独立任务（无依赖关系）
2. 为每个任务派遣独立子代理
3. 每个子代理完成后经**两轮审查**：
 - 规格符合度审查（是否完成了要求的功能）
 - 代码质量审查（是否符合工程标准）
4. 两轮审查通过后，进入下一个任务

## 为什么两轮审查

单轮审查可能遗漏规格偏差（代码质量好但功能不对）或质量问题（功能对但代码难维护）。两轮分离关注点，提高可靠性。

## 适用场景

- 多个功能模块可以独立开发
- 测试编写和实现可以并行
- 文档和代码可以并行生成

## 与 Superpowers 整体架构的关系

这个 skill 是 Superpowers 实现"可连续工作数小时不偏轨"的核心机制之一——每个子代理有独立上下文，不受其他任务的 context rot 影响。

## 相关

- entities/superpowers — Superpowers 完整介绍
- concepts/multi-agent-orchestration — 多 Agent 编排的通用概念
- concepts/context-rot — 并行子代理解决 context rot 的方式
- skills/superpowers-executing-plans — 串行执行的对应 skill
