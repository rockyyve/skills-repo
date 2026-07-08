# code-review (古法编程 视角)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 说完成了，但代码能不能合并没人知道"的问题。

> 另见 superpowers-requesting-code-review（mattpocock 视角，侧重如何发起 review）和 superpowers-receiving-code-review（如何接收 review）。本页聚焦于使用 AI 做代码审查本身的方法论。

## 核心前提

**"完成了"不等于"可以合并"。**

AI Agent 写完代码后，功能跑通只是起点。代码是否符合团队标准、是否安全、是否可维护，需要独立的审查流程。

## 审查顺序

**先看测试，再看实现。** 测试告诉你 AI 以为自己解决了什么问题——这是最快发现理解偏差的入口。

## 五个审查角度

| 角度       | 检查项                                       |
| ---------- | -------------------------------------------- |
| **正确性** | 核心需求、边界条件、旧行为保留、错误路径处理 |
| **可读性** | 命名是否自解释、函数长度、条件分支层次       |
| **架构**   | 是否符合项目既有模式，改动放在了正确的位置   |
| **安全**   | 用户输入验证、权限检查、敏感数据处理         |
| **性能**   | 循环内的外部请求、无界查询不必要的重复计算   |

## 问题分级

- **必须修**：功能错误、安全风险 — 阻塞合并
- **应该修**：可维护性问题、架构位置不对 — 本次或下次修
- **可后续**：命名小瑕疵、格式偏好 — 记录即可，不阻塞

## AI 自审 Prompt 模板

```
请按以下五个角度审查这段 diff：正确性、可读性、架构、安全、性能。
只列出真实存在的问题，不要泛泛而谈。
把问题分成三类：必须修、应该修、可后续。

```


## 古法编程原则

AI Agent 可以写代码，也可以帮做代码审查，但**最终判断必须在人手里**。审查结果是建议，决策是人的责任。

## 关联

- skills/superpowers-requesting-code-review — 如何向 AI 发起代码审查请求
- skills/superpowers-receiving-code-review — 如何处理收到的代码审查
- skills/security-and-hardening-gu-fa-bian-cheng/security-and-hardening-gu-fa-bian-cheng — 安全维度的专项检查
- skills/code-simplification-gu-fa-bian-cheng/code-simplification-gu-fa-bian-cheng — 可读性问题的后续处理

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`code-review`

**本地文件**：`skills/code-review-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/code-review-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/code-review-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/code-review-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/code-review-古法编程/SKILL.original.md` 替换 `SKILL.md`。
