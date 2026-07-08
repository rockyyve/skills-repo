# Source-Driven Development（文档驱动开发）

## 概述

每个框架特定的代码决策必须有官方文档支持。不要凭记忆实现——验证、引用，让用户看到你的来源。训练数据会过时，API 会被废弃，最佳实践会演变。这个技能确保用户拿到可信赖的代码，因为每个模式都追溯到权威来源。

## 什么时候用

- 用户想要遵循给定框架当前最佳实践的代码
- 构建样板、启动代码或会被复制到整个项目的模式
- 用户明确要求文档化、验证或"正确"的实现
- 实现框架推荐方式重要的功能
- 审查或改进使用框架特定模式的代码

**什么时候不用：**

- 正确性不依赖特定版本（重命名变量、修错字、移动文件）
- 跨版本都一样的纯逻辑
- 用户明确要速度不要验证

## 流程

```
检测 ──→ 获取 ──→ 实现 ──→ 引用
 │ │ │ │
 ▼ ▼ ▼ ▼
 什么 获取 遵循 展示
 技术栈 相关 文档化 你的
 文档 模式 来源

```


### 步骤 1：检测技术栈和版本

读项目的依赖文件识别精确版本：

```
package.json → Node/React/Vue/Angular/Svelte
composer.json → PHP/Symfony/Laravel
requirements.txt / pyproject.toml → Python/Django/Flask
go.mod → Go
Cargo.toml → Rust
Gemfile → Ruby/Rails

```


明确说你发现了什么：

```
STACK DETECTED:
- React 19.1.0 (from package.json)
- Vite 6.2.0
- Tailwind CSS 4.0.3
→ Fetching official docs for the relevant patterns.

```


如果版本缺失或模糊，**问用户**。不要猜——版本决定哪些模式是正确的。

### 步骤 2：获取官方文档

获取你要实现的功能的具体文档页面。不是首页，不是完整文档——是相关页面。

**来源层级（按权威性）：**

| 优先级 | 来源 | 示例 |
|-------|------|------|
| 1 | 官方文档 | react.dev, docs.djangoproject.com |
| 2 | 官方博客 / changelog | react.dev/blog |
| 3 | Web 标准参考 | MDN, web.dev |
| 4 | 浏览器/运行时兼容性 | caniuse.com, node.green |

**不权威——永远不要作为主要来源引用：**

- Stack Overflow 答案
- 博客文章或教程（即使流行的）
- AI 生成的文档或摘要
- 你自己的训练数据（这就是重点——验证它）

**获取要精确：**

```
BAD: Fetch the React homepage
GOOD: Fetch react.dev/reference/react/useActionState

```


### 步骤 3：按文档模式实现

写匹配文档的代码：

- 用文档中的 API 签名，不用记忆的
- 如果文档展示新方式，用新的
- 如果文档废弃了某个模式，不用废弃版本
- 如果文档没覆盖，标记为未验证

**当文档和已有项目代码冲突时：**

```
CONFLICT DETECTED:
The existing codebase uses useState for form loading state,
but React 19 docs recommend useActionState for this pattern.
(Source: react.dev/reference/react/useActionState)

Options:
A) Use the modern pattern — consistent with current docs
B) Match existing code — consistent with codebase
→ Which approach do you prefer?

```


暴露冲突。不要默默选一个。

### 步骤 4：引用你的来源

每个框架特定模式都有引用。用户必须能验证每个决策。

**代码注释中：**

```typescript
// React 19 form handling with useActionState
// Source: https://react.dev/reference/react/useActionState#usage
const [state, formAction, isPending] = useActionState(submitOrder, initialState);

```


**引用规则：**

- 完整 URL，不缩写
- 优先带锚点的深度链接
- 支持非明显决策时引用相关段落
- 如果你找不到某模式的文档，明确说出来：

```
UNVERIFIED: I could not find official documentation for this
pattern. This is based on training data and may be outdated.
Verify before using in production.

```


对无法验证的东西诚实比虚假自信更有价值。

## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "我对这个 API 很自信" | 自信不是证据。训练数据包含过时模式。验证。 |
| "获取文档浪费 token" | 幻觉一个 API 浪费更多。用户调试一小时发现函数签名变了。 |
| "文档不会有我需要的" | 如果文档没覆盖，那是有价值的信息——这个模式可能不被官方推荐。 |

## 危险信号

- 写框架特定代码不检查该版本的文档
- 用"I believe"或"I think"说 API 而不是引用来源
- 实现模式不知道它适用哪个版本
- 引用 Stack Overflow 或博客而不是官方文档
- 用因为训练数据出现的废弃 API
- 不读 `package.json` / 依赖文件就实现
- 交付代码没有框架特定决策的来源引用

## 验证

使用文档驱动开发实现后：

- [ ] 从依赖文件识别了框架和库版本
- [ ] 为框架特定模式获取了官方文档
- [ ] 所有来源是官方文档，不是博客或训练数据
- [ ] 代码遵循当前版本文档中展示的模式
- [ ] 非明显决策包含完整 URL 的来源引用
- [ ] 没用废弃 API
- [ ] 文档和已有代码的冲突已呈现给用户
- [ ] 无法验证的内容已明确标记为未验证
