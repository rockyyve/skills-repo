# Frontend UI Engineering

> **Skill 文件**：SKILL.md · SKILL.original.md

**Frontend UI Engineering** 解决「AI 写前端页面总有模板味」的问题。AI 的默认行为是生成通用模板、忽略设计细节、产出堆砌组件的页面。

相关：skills/browser-testing-with-devtools/browser-testing-with-devtools · concepts/context-engineering

## AI 写 UI 的默认失败模式

- 生成通用模板，缺乏品牌感
- 忽略设计细节（间距、对齐、颜色一致性）
- 堆砌组件而非设计页面
- 移动端适配遗漏
- 过度使用 padding/margin 造成视觉臃肿
- 颜色不一致（每个组件用不同的灰度值）

## 四个解决策略

### 策略 1：提供视觉参考

截图 + 「实现类似这个布局」的效果远好于文字描述。

- 提供竞品截图或设计稿
- 说「参考这个布局，但用我们的品牌色」
- 截图 > 文字描述 > 什么都不提供

### 策略 2：提供设计 Token

告诉 AI 具体的值，而不是模糊描述：

```
❌ 「看起来专业」
✅ 主色 #1E40AF，间距 4/8/16/24px，字体 Inter，卡片圆角 8px

```


建立一个简短的设计规范片段，每次开始 UI 任务时粘贴给 AI。

### 策略 3：分离结构和样式

先让 AI 完成**交互逻辑**，再单独处理**视觉细节**。

```
第一轮：「实现表单的提交逻辑和状态管理，样式暂时用 placeholder」
第二轮：「现在处理视觉样式，参考以下设计规范...」

```


混合处理逻辑和样式时，AI 容易两者都做得将就。

### 策略 4：浏览器验证闭环

写完就在浏览器看，不要等「全部完成」再验证。

- 每个组件实现后立即在浏览器检查
- 发现问题立即反馈给 AI，而不是积累到最后
- 参考 skills/browser-testing-with-devtools/browser-testing-with-devtools 了解验证流程

## AI 写 UI 的常见陷阱

| 陷阱       | 表现                          | 应对             |
| ---------- | ----------------------------- | ---------------- |
| 过度间距   | padding/margin 过大，页面空洞 | 提供具体间距值   |
| 颜色不一致 | 每个组件用不同灰度            | 提供颜色 token   |
| 移动端遗漏 | 只考虑桌面端布局              | 明确要求响应式   |
| 模板思维   | 复制粘贴通用组件              | 提供视觉参考截图 |

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`frontend-ui-engineering`

**本地文件**：`skills/frontend-ui-engineering/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/frontend-ui-engineering" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/frontend-ui-engineering" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/frontend-ui-engineering/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/frontend-ui-engineering/SKILL.original.md` 替换 `SKILL.md`。
