# using-superpowers (Superpowers)

Superpowers 的系统介绍 skill。**帮助用户理解整个系统如何工作。**

## 核心内容

- Superpowers 是什么（行为约束系统，不是代码库）
- 各 skill 的触发时机和相互关系
- 如何验证安装成功
- 如何绕过某个流程（"跳过需求确认，直接写"）
- 如何自定义 skill

## 安装验证

开新会话说"帮我规划这个功能"：

- **安装成功**：AI 自动进入结构化 brainstorming，提问而不是直接开写
- **安装失败**：AI 直接开始写代码

## Superpowers 完整 Skill 地图

```
需求阶段: brainstorming
计划阶段: writing-plans
执行阶段: executing-plans → using-git-worktrees
测试阶段: test-driven-development
调试阶段: systematic-debugging
验证阶段: verification-before-completion
审查阶段: requesting-code-review ↔ receiving-code-review
收尾阶段: finishing-a-development-branch
并行扩展: dispatching-parallel-agents
元技能: writing-skills · using-superpowers

```


## 相关

- entities/superpowers — Superpowers 完整知识页（含安装命令）
- references/gu-fa-bian-cheng-superpowers — 中文介绍参考
- synthesis/ai-coding-framework-landscape — 五大框架横向对比
