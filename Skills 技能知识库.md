---
title: Skills 技能知识库
created: 2026-07-03
updated: 2026-07-03
---

# Skills 技能知识库

完整的 AI 辅助开发技能知识库，包含开源技能体系和通用开发技能。

## 📊 统计信息

- **技能文件总数**：113 个
- **主要分类**：2 大类（开源技能库 + 通用技能）
- **开源技能库**：3 个体系（Superpowers、古法编程、MattPocock-Skills）
- **通用技能**：按开发阶段分 5 类
- **来源**：Obsidian Vault skills 目录

## 🗂️ 目录结构

### 📚 开源技能库

来自开源社区的完整技能体系。

#### Superpowers (15 个文件)

obra 的系统化 AI 辅助开发工作流。

**核心工作流**：

```
brainstorming → writing-plans → executing-plans 
  → requesting-code-review → receiving-code-review 
  → finishing-branch

```


**主要技能**：

- [superpowers-brainstorming](/knowledge/100) - 苏格拉底式需求挖掘
- [superpowers-writing-plans](/knowledge/110) - 生成细粒度计划
- [superpowers-executing-plans](/knowledge/102) - 执行开发计划
- [superpowers-tdd](/knowledge/97) - TDD 工作流
- [superpowers-requesting-code-review](/knowledge/105) - 请求代码审查
- [superpowers-receiving-code-review](/knowledge/104) - 接收审查反馈
- [superpowers-systematic-debugging](/knowledge/106) - 系统化调试
- [superpowers-verification-before-completion](/knowledge/109) - 完成前验证
- [superpowers-dispatching-parallel-agents](/knowledge/101) - 调度并行 Agents
- 更多...

**适用场景**：复杂项目、团队协作、需要系统化可重复流程

#### 古法编程 (28 个文件)

经过验证的传统软件工程最佳实践。

**主要领域**：

- **代码质量**
  - [代码审查](/knowledge/42) - 代码审查方法
  - [代码简化](/knowledge/39) - 代码简化技巧

- **版本控制与发布**
  - [Git工作流与版本管理](/knowledge/69) - Git 工作流
  - [发布与上线](/knowledge/46) - 发布上线
  - [CI-CD与自动化](/knowledge/66) - CI/CD 自动化

- **性能与安全**
  - [性能优化](/knowledge/55) - 性能优化
  - [安全加固](/knowledge/36) - 安全加固

- **文档规范**
  - [文档与ADR](/knowledge/52) - 文档与 ADR

**适用场景**：所有项目，特别是需要稳定性和可维护性的场景

#### MattPocock-Skills (4 个文件)

Matt Pocock 的工程实用主义技能集。

- [caveman-mode](/knowledge/92) - 穴居人模式，精简 AI 输出节省 75% token
- [setup-matt-pocock-skills](/knowledge/81) - 完整技能包配置

**设计理念**：工程实用主义、关注成本、简洁高效

**适用场景**：长时间会话、批量任务、快速迭代

---

### 🛠️ 通用技能

按软件开发生命周期阶段分类的通用技能。

#### 01-需求与设计 (28 个文件)

把模糊想法变成可执行的规格。

**需求澄清**：

- [想法精炼](/knowledge/125) - 把模糊想法精炼成具体需求
- [grill-the-user](/knowledge/134) - 苏格拉底式需求拷问
- [需求拷问（带文档）](/knowledge/128) - 带项目上下文的需求澄清

**设计规划**：

- [规格驱动开发](/knowledge/112) - 规格驱动开发
- [源码驱动开发](/knowledge/130) - 源码驱动开发
- [planning-with-files](/knowledge/133) - 用文件管理计划
- [任务拆解](/knowledge/115) - 任务分解
- [生成PRD](/knowledge/121) - 生成产品需求文档
- [生成Issues](/knowledge/118) - 转换为开发 Issues
- [prototype](/knowledge/135) - 快速原型开发

#### 02-开发实施 (13 个文件)

从需求到代码的实现过程。

**开发方法**：

- [测试驱动开发](/knowledge/140) - 测试驱动开发
- [增量实现](/knowledge/147) - 增量实现策略

**架构设计**：

- [API与接口设计](/knowledge/150) - API 设计原则
- [改进代码架构](/knowledge/144) - 改进代码库架构

#### 03-调试与优化 (10 个文件)

发现问题、定位根因的系统化方法。

- [调试与错误恢复](/knowledge/154) - 调试与错误恢复
- [问题诊断](/knowledge/160) - 系统化诊断方法
- [triage](/knowledge/162) - 问题分级
- [浏览器DevTools测试](/knowledge/157) - 浏览器 DevTools 测试

#### 04-发布与维护 (1 个文件)

从代码到生产环境的交付流程。

- [handoff](/knowledge/163) - 工作交接指南

#### 05-专项技能 (14 个文件)

跨领域的专业技能。

**前端开发**：

- [前端UI工程](/knowledge/165) - 前端 UI 工程实践

**上下文工程**：

- [上下文工程](/knowledge/168) - 上下文管理与优化

**知识管理**：

- [wiki-capture](/knowledge/171) - 捕获知识到 Wiki
- [wiki-query](/knowledge/172) - Wiki 查询技巧
- [wiki-research](/knowledge/173) - Wiki 研究方法
- [wiki-synthesize](/knowledge/174) - 知识综合

**工具与协作**：

- [skill-creator](/knowledge/175) - 技能创建器
- [continuous-learning-v2](/knowledge/176) - 持续学习
- [cross-linker](/knowledge/177) - 交叉链接工具
- [zoom-out](/knowledge/96) - 放大视角思考

---

## 🔥 推荐学习路径

### 🌱 新手路径（从零开始）

**目标**：掌握 AI 辅助开发的基本流程

1. [想法精炼](/knowledge/125) - 学会把想法变成需求
2. [grill-the-user](/knowledge/134) - 需求澄清技巧
3. [规格驱动开发](/knowledge/112) - 规格驱动开发
4. [测试驱动开发](/knowledge/140) - 测试驱动开发
5. [caveman-mode](/knowledge/92) - 提升效率

### 🚀 Superpowers 完整路径

**目标**：掌握系统化的 AI 辅助开发工作流

1. [superpowers-brainstorming](/knowledge/100) - 需求对齐
2. [superpowers-writing-plans](/knowledge/110) - 编写计划
3. [superpowers-executing-plans](/knowledge/102) - 执行计划
4. [superpowers-tdd](/knowledge/97) - TDD 工作流
5. [superpowers-requesting-code-review](/knowledge/105) - 请求审查
6. [superpowers-finishing-a-development-branch](/knowledge/103) - 完成分支

### 🏗️ 古法编程路径（传统工程实践）

**目标**：掌握经过验证的软件工程最佳实践

1. [代码审查](/knowledge/42) - 代码审查
2. [Git工作流与版管理](/knowledge/69) - Git 工作流
3. [CI-CD与自动化](/knowledge/66) - CI/CD
4. [性能优化](/knowledge/55) - 性能优化
5. [安全加固](/knowledge/36) - 安全加固

### ⚡ 效率提升路径

**目标**：最大化开发效率

1. [caveman-mode](/knowledge/92) - 节省 75% token
2. [上下文工程](/knowledge/168) - 上下文优化
3. [增量实现](/knowledge/147) - 增量实现
4. [superpowers-dispatching-parallel-agents](/knowledge/101) - 并行协作

---

## 📚 按场景选择技能

### 我刚有一个想法

→ [想法精炼](/knowledge/125) → [grill-the-user](/knowledge/134) → [superpowers-brainstorming](/knowledge/100)

### 需求还不明确

→ [需求拷问（带文档）](/knowledge/128) → [规格驱动开发](/knowledge/112)

### 准备开始写代码

→ [superpowers-writing-plans](/knowledge/110) → [测试驱动开发](/knowledge/140)

### 代码写完了

→ [代码审查](/knowledge/42) → [代码简化](/knowledge/39)

### 出现 Bug

→ [调试与错误恢复](/knowledge/154) → [superpowers-systematic-debugging](/knowledge/106)

### 性能有问题

→ [性能优化](/knowledge/55)

### 准备发布

→ [superpowers-finishing-a-development-branch](/knowledge/103) → [发布与上线](/knowledge/46)

### 需要交接工作

→ [文档与ADR](/knowledge/52) → [handoff](/knowledge/163)

---

## 📊 分类统计

| 分类 | 文件数 | 占比 |
|------|--------|------|
| **开源技能库** | **47 个** | **42%** |
| ├─ Superpowers | 15 个 | 13% |
| ├─ 古法编程 | 28 个 | 25% |
| └─ MattPocock-Skills | 4 个 | 4% |
| **通用技能** | **66 个** | **58%** |
| ├─ 01-需求与设计 | 28 个 | 25% |
| ├─ 02-开发实施 | 13 个 | 12% |
| ├─ 03-调试与优化 | 10 个 | 9% |
| ├─ 04-发布与维护 | 1 个 | 1% |
| └─ 05-专项技能 | 14 个 | 12% |
| **总计** | **113 个** | **100%** |

---

## 🔗 外部资源

- **Superpowers**：[GitHub - obra/superpowers](https://github.com/obra/superpowers)
- **古法编程**：[StudyNil - AI 编程](https://www.studynil.com/ai/)
- **Matt Pocock Skills**：[GitHub - mattpocock/skills](https://github.com/mattpocock/skills)

---

## 📝 维护说明

- **源数据**：`/Users/asher/Documents/Obsidian Vault/skills`
- **分类原则**：
  - 开源库技能按来源分类（Superpowers、古法编程、MattPocock-Skills）
  - 通用技能按开发阶段分类（需求与设计、开发实施、调试与优化、发布与维护、专项技能）
- **更新策略**：在 Obsidian Vault 中编辑，定期同步到此目录
- **最后更新**：2026-07-03

---

**Happy Coding!**
