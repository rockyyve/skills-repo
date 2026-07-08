# API and Interface Design

> **Skill 文件**：SKILL.md · SKILL.original.md

**API and Interface Design** 解决「AI 设计接口时跳过契约直接写实现」的问题。接口一旦被调用就难以修改，先定契约的成本远低于后改接口。

相关：skills/incremental-implementation/incremental-implementation · concepts/vertical-slice-tdd

## 契约优先原则

**先定义，后实现**。让 AI 输出接口定义，人工确认后再写实现代码。

```
第一步：「先输出这个模块的接口签名，不要写实现」
第二步：（人工审查接口）
第三步：「接口确认，现在写实现」

```


## AI 应先输出的产物

根据技术栈选择适合的形式：

- **TypeScript**：`interface` / `type` 定义
- **REST API**：OpenAPI / Swagger spec 片段
- **函数**：函数签名 + JSDoc / docstring（包含参数类型和返回值）
- **数据库**：表结构 / Schema 定义

## 接口审查清单

在让 AI 开始实现之前，确认以下每一项：

- [ ] 输入边界是否明确（什么是合法输入，什么会被拒绝）
- [ ] 错误情况是否覆盖（每种错误返回什么）
- [ ] 命名是否与项目已有命名一致
- [ ] 是否破坏已有调用方（向后兼容性）
- [ ] 对外 API 是否考虑了版本化策略

## 为什么不能边设计边实现

AI 写实现时会做隐式决策：

> 「这个参数要不要可选？」「错误用异常还是返回值？」「命名用 get 还是 fetch？」

这些决策一旦写进实现就被固化。先定契约，则这些决策在最廉价的阶段（没有代码依赖时）就被显式化和审查。

## 版本化考虑

如果是对外 API（面向其他团队或外部用户），AI 生成接口时需要额外考虑：

- 能否向后兼容（新增字段 vs 修改字段）
- 废弃路径（deprecated 标记 + 迁移指南）
- 版本号策略（URL 版本 `/v1/` vs Header 版本）

## 与 incremental-implementation 的关系

契约优先定义了「做什么」（接口），增量实现 定义了「怎么做」（每步只实现一个可验证的小步骤）。两者结合使用效果最佳。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`api-and-interface-design`

**本地文件**：`skills/api-and-interface-design/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/api-and-interface-design" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/api-and-interface-design" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/api-and-interface-design/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/api-and-interface-design/SKILL.original.md` 替换 `SKILL.md`。
