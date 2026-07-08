# shipping-and-launch (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 写完功能怎么上线"的问题。

## 核心前提

**上线不只是部署。** 部署只是上线的一个步骤。完整的上线包括：验证、监控配置、回滚准备，以及上线后的观察窗口。

## 上线前检查清单

- [ ] 测试全绿（CI 通过）
- [ ] 没有已知严重 bug
- [ ] 监控已配置（关键指标能看到）
- [ ] 回滚方案已准备（知道怎么回滚、什么情况触发）
- [ ] 安全检查通过（见 skills/security-and-hardening-gu-fa-bian-cheng/security-and-hardening-gu-fa-bian-cheng）

## 让 AI 生成检查清单

```
我要上线一个 [项目类型：Web API / 前端应用 / 数据迁移 / ...]。
帮我生成一个针对这个类型的上线检查清单。

```


不同项目类型的风险点不同，让 AI 生成针对性的清单比通用模板更有用。

## 分阶段发布

如果条件允许：

1. 先发给小部分用户（10% 流量或内部用户）
2. 观察 15–30 分钟，确认关键指标正常
3. 没有异常再逐步扩量到全量

## 监控配置原则

**监控要在上线前配置，不是出了问题才去加。** 上线后才发现没有监控等于盲飞。

关键指标（至少要有）：

- 误率
- 响应时间 / P99 延迟
- 业务核心指标（如注册成功率、支付完成率）

## 回滚条件要提前定义

**提前决定什么情况触发回滚，不要到时候临时判断。**

示例触发条件：

- 错误率超过 1%
- P99 延迟超过 2 秒
- 任何支付相关错误出现

## 上线后观察窗口

上线后不要立刻离开。观察 15–30 分钟，确认：

- 没有错误率飙升
- 关键业务指标正常
- 用户没有反馈异常

## 关联

- skills/ci-cd-and-automation-gu-fa-bian-cheng/ci-cd-and-automation-gu-fa-bian-cheng — CI 通过是上线的前提
- skills/security-and-hardening-gu-fa-bian-cheng/security-and-hardening-gu-fa-bian-cheng — 上线前安全检查
- skills/git-workflow-and-versioning-gu-fa-bian-cheng/git-workflow-and-versioning-gu-fa-bian-cheng — 上线对应的 git tag 和版本管理

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`shipping-and-launch`

**本地文件**：`skills/shipping-and-launch-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/shipping-and-launch-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/shipping-and-launch-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/shipping-and-launch-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/shipping-and-launch-古法编程/SKILL.original.md` 替换 `SKILL.md`。
