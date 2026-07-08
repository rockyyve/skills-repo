# ci-cd-and-automation (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 项目怎么设置自动化质量门"的问题。

## 质量门的作用

每次提交自动检查，**防止已修复的问题复现**。没有 CI，修复的 bug 会被后续的 AI 改动重新引入。

## 最小 CI 配置

每次 PR 必须通过：

1. **Lint** — 代码风格检查（ESLint、Ruff、golangci-lint）
2. **类型检查** — 静态类型验证（tsc、mypy、go vet）
3. **单元测试** — 核心逻辑的快速测试

这三项是最低要求，也是最高 ROI 的投入。

## 让 AI 配置 CI

```
我的项目使用 [语言] + [测试框架]。
帮我生成一个 GitHub Actions 配置，包含 lint、类型检查和测试。
要求每个步骤独立，失败时能看到是哪一步出问题。

```


## CI 速度原则

**超过 5 分钟的 CI 会被绕过。** 开发者等不了就会 push 跳过检查。优化方向：

- 缓存依赖安装
- 并行跑独立步骤
- 快速失败（先跑最可能失败的检查）

## 自动化内容（按优先级）

| 优先级 | 内容 |
|--------|------|
| 必须 | 代码格式检查、类型检查、单元测试 |
| 推荐 | 集成测试、代码覆盖率报告 |
| 可选 | 安全扫描（Snyk、Trivy）、性能回归检测 |

## 渐进式配置原则

**不要一开始就配置复杂 CI。** 先从基础三项开始，项目成熟后再逐步添加。复杂的 CI 配置本身也需要维护。

## 关联

- skills/git-workflow-and-versioning-gu-fa-bian-cheng/git-workflow-and-versioning-gu-fa-bian-cheng — CI 是 push 后的下一步
- skills/deprecation-and-migration-gu-fa-bian-cheng/deprecation-and-migration-gu-fa-bian-cheng — 用 CI 检测旧接口调用残留
- skills/shipping-and-launch-gu-fa-bian-cheng/shipping-and-launch-gu-fa-bian-cheng — CI 通过是上线前提条件

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`ci-cd-and-automation`

**本地文件**：`skills/ci-cd-and-automation-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/ci-cd-and-automation-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/ci-cd-and-automation-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/ci-cd-and-automation-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/ci-cd-and-automation-古法编程/SKILL.original.md` 替换 `SKILL.md`。
