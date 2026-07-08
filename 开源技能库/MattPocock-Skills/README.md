# MattPocock-Skills

Matt Pocock 的完整技能集 —— 21 个 Markdown 文件，2026 年 3 月一夜冲上 GitHub Trending #1。

## 📚 来源

- **GitHub 仓库**: [mattpocock/skills](https://github.com/mattpocock/skills)
- **作者**: Matt Pocock
- **理念**: "Skills for Real Engineers" —— 保留工程师对流程的完全控制权

## 🎯 设计哲学

反对接管开发流程的框架（如 GSD、BMAD、Spec-Kit），而是把每个痛点做成 ~50 行可组合的 skill，让工程师自由组合使用。

## 📋 技能清单（按使用频率）

### 🔥 Engineering（日常开发主力）

| 技能 | 定位 | 何时用 |
| ------ | ------ | -------- |
| [grill-with-docs](./grill-with-docs/) | 拷问需求 + 自动更新 CONTEXT.md/ADR | **每次新需求前** |
| test-driven-development | red-green-refactor 垂直切片 TDD | 写新功能 / 修 bug |
| [diagnose](./diagnose/) | 6 阶段调试方法论，先建反馈环 | 难复现 bug、性能回退 |
| [to-prd](./to-prd/) | 把对话凝结成 PRD 并提交 issue | 拷问完成后 |
| [to-issues](./to-issues/) | PRD/计划拆成端到端垂直切片 issue | PRD 落地后 |
| [triage](./triage.md) | Issue 状态机分类，按 label 流转 | 接手新 backlog |
| [improve-codebase-architecture](./improve-codebase-architecture/) | 找"可加深模块"机会，周期性精修 | 每隔几天 / 重构周 |
| [zoom-out](./zoom-out.md) | 让 Agent 站在系统层面再讲一遍 | 进入陌生模块 |
| [setup-matt-pocock-skills](./setup-matt-pocock-skills/) | 初始化 issue tracker / 标签 / 文档路径 | 装完 skills 第一次跑 |

### 🛠️ Productivity（通用工作流）

| 技能 | 定位 |
| ------ | ------ |
| [caveman-mode](./caveman-mode.md) | 极简通信模式，token 立省 ~75% |
| [grill-the-user](./grill-the-user.md) | 非代码场景的需求拷问 |

### 🧰 Misc（偶尔用一次）

- git-guardrails-claude-code - 在 hook 层拦破坏性 Git 操作
- migrate-to-shoehorn - 测试断言迁移工具
- scaffold-exercises - Matt 的课程题目脚手架
- setup-pre-commit - Husky + lint-staged 一键配置

## 💡 核心技能推荐

### 立即可用


- **[caveman-mode](./caveman-mode.md)** - 节省 75% token，长会话必备
- **[grill-with-docs](./grill-with-docs/)** - 需求澄清的标准流程
- **[triage](./triage.md)** - 快速整理 backlog

### 系统化提升


- **[diagnose](./diagnose/)** - 系统化调试方法
- **[improve-codebase-architecture](./improve-codebase-architecture/)** - 持续架构精修
- **[to-prd](./to-prd/) + [to-issues](./to-issues/)** - 需求到任务的标准链路

## 🔄 与通用技能的关系

本目录展示 MattPocock Skills 的完整体系。这些技能同时也存在于通用技能目录中（按开发阶段分类），方便按场景查找：

- 需求与设计阶段 → grill-with-docs, to-prd, to-issues
- 开发实施阶段 → test-driven-development, improve-codebase-architecture
- 调试与优化阶段 → diagnose, triage

## 📖 使用建议

1. **新手入门**: caveman-mode → grill-with-docs → test-driven-development
2. **完整工作流**: grill-with-docs → to-prd → to-issues → test-driven-development → diagnose
3. **持续改进**: triage → improve-codebase-architecture → zoom-out

## 🔗 外部资源

- [GitHub - mattpocock/skills](https://github.com/mattpocock/skills)
- [深度解读文章](https://www.studynil.com/ai/)

---

**总计**: 10 个技能（22 个文件）  
**最后更新**: 2026-07-03
