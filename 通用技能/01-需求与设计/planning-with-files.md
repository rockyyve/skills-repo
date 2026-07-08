# planning-with-files

Local Agent Skills 中的文件化规划 skill（v2.10.0）。**把复杂任务的规划状态持久化到文件**，解决 context 清空后丢失进度的问题。

## 触发时机

- 复杂多步骤任务（需要 >5 次工具调用）
- 研究项目
- 任何需要跨 session 保持进度的任务

## 三个核心文件

| 文件           | 内容                         |
| -------------- | ---------------------------- |
| `task_plan.md` | 任务分解、依赖关系、当前阶段 |
| `findings.md`  | 研究发现、中间结论           |
| `progress.md`  | 已完成步骤、当前状态、下一步 |

## 关键特性：/clear 后自动恢复

每次工具调用前，hook 自动读取 `task_plan.md` 前 30 行注入上下文。即使用户执行 `/clear` 清空对话，下一条消息时 AI 能自动恢复到中断前的状态。

## 与 Superpowers writing-plans 的对比

|        | planning-with-files | superpowers writing-plans |
| ------ | ------------------- | ------------------------- |
| 持久化 | 文件跨 session）    | 内存（当前 session）      |
| 粒度   | 任务级              | 微步骤级（2-5 分钟）      |
| 恢复   | 自动（hook）        | 手动                      |

## 相关

- entities/local-agent-skills — 所属 skill 系统
- skills/superpowers-writing-plans — Superpowers 的计划生成 skill
- concepts/context-rot — 文件化规划解决 context rot 的方式
