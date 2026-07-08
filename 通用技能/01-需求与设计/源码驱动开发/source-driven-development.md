# Source-Driven Development

> **Skill 文件**：SKILL.md · SKILL.original.md

**Source-Driven Development** 解决「AI 写框架代码总是过时」的问题。AI 的训练数据有截止日期，框架 API 更新后 AI 可能仍在使用旧写法，导致代码无法运行或使用废弃接口。

相关：concepts/context-engineering · skills/grill-with-docs/grill-with-docs

## 问题根源

AI 模型的知识有截止日期。对于快速迭代的框架（如 Next.js、shadcn/ui、LangChain），版本间 API 变化巨大，AI 用旧 API 的概率极高。

## 解决方案

在对话开始时，给 AI 提供**当前版本**的官方文档或源码，并明确说：

> 「请根据这份文档而不是你的训练数据来写代码」

## 操作步骤

1. 找到框架当前版本的文档 URL 或 changelog
2. 在对话开始时将文档内容粘贴给 AI（或提供 URL 让 AI 抓取）
3. 明确告知 AI 以文档为准而非训练记忆
4. 对重要 API 调用，让 AI 引用它使用的具体文档来源

## 信息来源优先级

| 来源 | 适用场景 |
|------|----------|
| 官方文档站 | 最权威，适合 API 参考 |
| GitHub releases / changelog | 了解版本间变化 |
| npm / PyPI 包页面 | 查看最新版本号和简要说明 |
| 框架源码 | 文档不完整时直接看实现 |

## 适用场景

这个技能在使用**快速迭代框架**时特别重要：

- **Next.js**：App Router vs Pages Router 差异巨大
- **shadcn/ui**：组件 API 频繁调整
- **LangChain**：Python/JS 两个版本均高速演进
- **任何近一年内有重大版本升级的框架**

## 验证方式

让 AI 在代码旁边标注「参考了文档的第 X 章 / 第 X 行」，确保它确实在使用你提供的文档而非训练记忆。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`source-driven-development`

**本地文件**：`skills/source-driven-development/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/source-driven-development" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/source-driven-development" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/source-driven-development/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/source-driven-development/SKILL.original.md` 替换 `SKILL.md`。
