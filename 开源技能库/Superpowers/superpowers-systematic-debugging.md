# Superpowers Systematic Debugging

## 4 阶段框架

```
1. 复现（Reproduce） → 稳定触发
2. 定位（Isolate） → 缩小范围
3. 根因（Root Cause） → 找到真正原因
4. 修复（Fix） → 最小变更

```


`diagnose` 是 6 阶段流程（更细化，含假设提出和定向插桩）；`systematic-debugging` 是 4 阶段框架（更简洁）。两者都强调**先建立反馈环，不随机试**。

## 与 diagnose 的对比

| 维度 | systematic-debugging | diagnose |
|---|---|---|
| 阶段数 | 4 | 6 |
| 假设方法 | 隐式 | 显式（先列假设再验证） |
| 适用 | 常规 bug | 复杂 bug（多个可能原因） |

## 何时用

- ✅ 任何生产 bug 调查
- ✅ 测试失败的根因分析
- ❌ 一眼能看出的语法错误（直接改即可）

## 与其他概念的关系

- concepts/feedback-loop-driven-debugging — systematic-debugging 是其工程化实现
- concepts/ai-debugging — AI 辅助调试的 6 阶段流程
- skills/diagnose/SKILL — 6 阶段更细化版本

## Sources

- concepts/feedback-loop-driven-debugging — 调试反馈环方法论
