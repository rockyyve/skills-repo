# performance-optimization (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 优化性能前为什么要先测量"的问题。

## AI 优化性能的常见失误

- 优化了不是瓶颈的地方（感觉慢的地方不一定是真正慢的地方）
- 引入复杂性换来微小收益（得不偿失）
- 优化后没有验证效果（不知道有没有用）

## 先测量再优化

用工具找到真实瓶颈，不靠直觉：

- **后端**：profiler（py-spy、pprof、JVM profiler）、日志时间戳
- **前端**：浏览器 Performance 面板、Lighthouse
- **数据库**：EXPLAIN ANALYZE、慢查询日志

## 给 AI 的约束模板

```
先帮我找出哪里最慢，给我数据。
不要先讨论优化方案，先给我测量结果。

```


让 AI 先做诊断，而不是直接给优化方案——这避免了优化错误位置。

## 优化验证流程

1. 优化前记录基准数据（响应时间、内存、CPU）
2. 实施优化
3. 用同样条件重新测量
4. 对比数据，确认有改善

**没有对比就不知道有没有效果。**

## 常见真实瓶颈

- **N+1 查询**：循环里每次都查数据库
- **没有索引的数据库查询**：全表扫描
- **重复的网络请求**：同一数据请求多次，缺少缓存
- **大对象的不必要序列化**：在每次请求时序列化不变的数据

## 不要提前优化

如果没有性能问题，不需要优化。如果有性能问题，先测量，再优化，再验证。

## 关联

- skills/code-review-gu-fa-bian-cheng/code-review-gu-fa-bian-cheng — 性能是代码审查五角度之一
- skills/ci-cd-and-automation-gu-fa-bian-cheng/ci-cd-and-automation-gu-fa-bian-cheng — 可在 CI 中加入性能回归检测

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`performance-optimization`

**本地文件**：`skills/performance-optimization-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/performance-optimization-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/performance-optimization-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/performance-optimization-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/performance-optimization-古法编程/SKILL.original.md` 替换 `SKILL.md`。
