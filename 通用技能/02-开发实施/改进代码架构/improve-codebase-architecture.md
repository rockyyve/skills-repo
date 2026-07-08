# improve-codebase-architecture

> **Skill 文件**：SKILL.md · SKILL.original.md

mattpocock/skills 中的架构体检 skill。建议每隔几天或重构周期跑一次。

## 检查目标

- **浅模块**（shallow modules）— 接口宽、实现简单、信息隐藏弱的模块
- **耦合泄漏** — 实现细节暴露到接口层
- **测试 seam 不佳** — 难以在不修改实现的情况下插入测试

## 核心概念：加深模块

目标是把模块"加深"（deepen）：**小接口隐藏更多实现复杂度**，提高 locality 和 leverage。

这来自 John Ousterhout 的 _A Philosophy of Software Design_ 中的 "deep modules" 概念——见 concepts/architecture-decay 中的引用。

## 吸收的废弃 Skill

吸收了 `design-an-interface`（基于"Design It Twice"生成多个接口设计方案）的能力，作为 interface design 辅助材料。

## 在工作流中的位置

```
每周 / 重构周 → improve-codebase-architecture

```


不在主工作流的关键路径上，而是**周期性维护**操作。

## 架构改善 vs 重构（古法编程 视角）

**重构**是等价变换——行为不变，结构改变。**架构改善**是改变组件之间的关系，影响更广、风险更高。

### 越改越浅的原因

AI 看到的是局部，不知道改动对整个系统的影响。没有全局视图的架构改动容易把系统改得更脆弱。

### 正确的架构改善流程

1. 先画出（或文字描述）**现有架构图**
2. 明确**改善目标**（要解决什么问题）
3. 分步改，**每步可独立验证**

### 给 AI 的约束模板

```
先描述这个改动会影响哪些组件。
再列出你准备怎么一步一步改。
等我确认后再开始，不要一次性全改。

```


### 风险控制

架构改善必须有**集成测试**覆盖，不能只靠单元测试。单元测试无法验证组件之间的关系是否正确。

> 古法编程 视角：架构改善是最容易让 AI 失控的任务，必须保持最细粒度的控制。

## 关

- concepts/architecture-decay — AI 加速架构腐烂的机制，以及 deep modules 概念
- entities/mattpocock-skills — 仓库全貌
- skills/deprecation-and-migration-gu-fa-bian-cheng/deprecation-and-migration-gu-fa-bian-cheng — 架构迁移的渐进式替换策略
- skills/documentation-and-adrs-gu-fa-bian-cheng/documentation-and-adrs-gu-fa-bian-cheng — 架构决策应配套 ADR 记录

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`improve-codebase-architecture`

**本地文件**：`skills/improve-codebase-architecture/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/improve-codebase-architecture" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/improve-codebase-architecture" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/improve-codebase-architecture/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/improve-codebase-architecture/SKILL.original.md` 替换 `SKILL.md`。
