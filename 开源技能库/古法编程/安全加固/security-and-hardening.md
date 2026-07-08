# security-and-hardening (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 写代码怎么做安全检查"的问题。

## 核心前提

**功能正确不等于安全。** AI 倾向于假设输入是可信的，在快速实现功能时会跳过安全边界的验证。

## 必须检查的四个安全点

| 类别 | 具体风险 |
|------|----------|
| **用户输入** | XSS、SQL 注入、路径遍历、命令注入 |
| **权限** | 每个操作是否验证身份（认证）和授权（是否有权限做这件事） |
| **敏感数据** | 密码、token、密钥不能硬编码；不能出现在日志或前端响应中 |
| **依赖** | 第三方包的已知漏洞（定期扫描，不要盲目信任） |

## AI 安全审查 Prompt

```
你现在是攻击者。找出我刚写的这段代码里的攻击面。
列出你能利用的每一个点，以及攻击方式。

```


让 AI 切换视角是发现安全问题最有效的 prompt 策略。

## 常见的 AI 安全失误

- 忘记验证权限（只验证登录，没验证操作权限）
- 在错误信息里泄露内部细节（stack trace、数据库结构）
- 把 API key 写进代码或配置文件提交到仓库
- 信任客户端传来的用户 ID 或角色信息

## 分层防御原则

**不要假设某一层一定安全，每层都要验证。** 即使有 API Gateway 做了输入过滤，业务层仍然要验证。即使业务层验证了，数据库层也要用参数化查询。

## 何时做安全检查

安全不是最后一步：

- **Spec 阶段**：定义需求时就考虑攻击面
- **Code review 阶段**：作为五个审查角度之一（见 skills/code-review-gu-fa-bian-cheng/code-review-gu-fa-bian-cheng）
- **上线前**：作为发布检查清单的必检项（见 skills/shipping-and-launch-gu-fa-bian-cheng/shipping-and-launch-gu-fa-bian-cheng）

## 关联

- skills/code-review-gu-fa-bian-cheng/code-review-gu-fa-bian-cheng — 安全是代码审查五角度之一
- skills/shipping-and-launch-gu-fa-bian-cheng/shipping-and-launch-gu-fa-bian-cheng — 上线前安全检查清单

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`security-and-hardening`

**本地文件**：`skills/security-and-hardening-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/security-and-hardening-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/security-and-hardening-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/security-and-hardening-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/security-and-hardening-古法编程/SKILL.original.md` 替换 `SKILL.md`。
