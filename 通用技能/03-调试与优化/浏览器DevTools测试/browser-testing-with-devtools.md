# Browser Testing with DevTools

> **Skill 文件**：SKILL.md · SKILL.original.md

**Browser Testing with DevTools** 解决「AI 写完前端怎么用浏览器验证」的问题。AI 写完代码不等于功能正确，需要在真实浏览器中系统性验证。

相关：skills/frontend-ui-engineering/frontend-ui-engineering · concepts/feedback-loop-driven-debugging

## 核心原则

> AI 写完前端代码 ≠ 功能正确。真实浏览器验证是不可跳过的步骤。

## 验证清单

每次 AI 交付前端代码后，按顺序检查：

- [ ] **渲染正确**：页面能正常显示，没有明显布局错乱
- [ ] **交互响应**：按钮点击、表单提交、导航等交互正确
- [ ] **网络请求**：Network 面板确认请求发出、响应正确、状态码正常
- [ ] **控制台无错误**：Console 面板无红色报错（警告酌情处理）
- [ ] **移动端适配**：切换到移动端视图检查响应式布局

## DevTools 各面板用途

| 面板            | 用途                                  |
| --------------- | ------------------------------------- |
| **Network**     | 查看请求/响应、状态码、请求体、响应体 |
| **Console**     | 查看 JS 错误、警告、日志输出          |
| **Elements**    | 检查 DOM 结构、CSS 样式是否符合预期   |
| **Application** | 查看 localStorage、Cookie、Session    |
| **Sources**     | 在源码级别设断点调试                  |

## 把 DevTools 信息反馈给 AI

发现问题后，把 DevTools 截图或错误信息直接给 AI：

```
「Console 面板显示以下错误：[粘贴错误信息]
Network 面板显示请求返回 404，URL 是 [URL]
请分析原因并修复」

```


AI 能解读 DevTools 的输出，但需要你提供这些信息。

## 测试路径

不要只测「主路径」，还需要覆盖：

| 路径类型     | 示例                       |
| ------------ | -------------------------- |
| **主路径**   | 正常填写表单并提交         |
| **边界路径** | 空数据、超长输入、特殊字符 |
| **错误路径** | 网络失败、服务器报错       |
| **权限路径** | 未登录访问、无权限操作     |

## 与 browse-daemon 的区别

- **Browser Testing with DevTools**（本技能）：**人工验证**，适合开发过程中的快速检查
- **browse-daemon / 自动化测试**：AI 自动化验证，适合回归测试

两者互补，不互相替代。参考 concepts/feedback-loop-driven-debugging 理解为什么浏览器验证也是建立反馈回路的一种方式。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`browser-testing-with-devtools`

**本地文件**：`skills/browser-testing-with-devtools/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/browser-testing-with-devtools" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/browser-testing-with-devtools" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/browser-testing-with-devtools/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/browser-testing-with-devtools/SKILL.original.md` 替换 `SKILL.md`。
