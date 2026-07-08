# CI/CD and Automation（CI/CD 和自动化）

## 概述

自动化质量门禁，确保没有变更在通过测试、lint、类型检查和构建之前到达生产。CI/CD 是每个其他技能的执行机制——它持续地抓住人类和 Agent 遗漏的东西。

**左移：** 在管线中尽可能早地抓问题。在 lint 中抓到的 bug 花分钟；同一个 bug 在生产中抓到花小时。

**更快更安全：** 更小批次和更频繁发布降低风险而不是增加。一个 3 个变更的部署比一个 30 个的更容易调试。

## 什么时候用

- 设置新项目的 CI 管线
- 添加或修改自动化检查
- 配置部署管线
- 变更需要触发自动化验证时
- 调试 CI 失败

## 质量门禁管线

每个变更合并前都过这些门禁：

```
Pull Request Opened
 │
 ▼
┌─────────────────┐
│ LINT CHECK │ eslint, prettier
│ ↓ pass │
│ TYPE CHECK │ tsc --noEmit
│ ↓ pass │
│ UNIT TESTS │ jest/vitest
│ ↓ pass │
│ BUILD │ npm run build
│ ↓ pass │
│ INTEGRATION │ API/DB 测试
│ ↓ pass │
│ E2E (可选) │ Playwright/Cypress
│ ↓ pass │
│ SECURITY AUDIT │ npm audit
│ ↓ pass │
│ BUNDLE SIZE │ bundlesize check
└─────────────────┘
 │
 ▼
 准备审查

```


**不能跳过任何门禁。** 如果 lint 失败，修 lint——不要禁用规则。如果测试失败，修代码——不要跳过测试。

## GitHub Actions 配置

### 基本 CI 管线

```yaml
# .github/workflows/ci.yml
name: CI

on:
 pull_request:
 branches: [main]
 push:
 branches: [main]

jobs:
 quality:
 runs-on: ubuntu-latest
 steps:
 - uses: actions/checkout@v4
 - uses: actions/setup-node@v4
 with:
 node-version: '22'
 cache: 'npm'
 - name: Install dependencies
 run: npm ci
 - name: Lint
 run: npm run lint
 - name: Type check
 run: npx tsc --noEmit
 - name: Test
 run: npm test -- --coverage
 - name: Build
 run: npm run build
 - name: Security audit
 run: npm audit --audit-level=high

```


## 把 CI 失败反馈给 Agent

CI 和 AI Agent 配合的力量在于反馈循环：

```
CI 失败
 │
 ▼
复制失败输出
 │
 ▼
反馈给 Agent：
"CI 管线失败，报错：
[粘贴具体错误]
修复问题，推送前本地验证。"
 │
 ▼
Agent 修复 → 推送 → CI 再跑

```


## 部署策略

### Feature Flags

Feature flags 把部署和发布解耦：

- **部署代码但不启用。** 尽早合并到 main，准备好时启用。
- **回滚不用重新部署。** 禁用 flag 代替回滚代码。
- **Canary 新功能。** 对 1% 用户启用，然后 10%，然后 100%。

**Flag 生命周期：** 创建 → 测试启用 → Canary → 全量上线 → 移除 flag 和死代码。永远不清理的 flag 变成技术债。

### 分阶段上线

```
PR 合并到 main
 │
 ▼
 Staging 部署（自动）
 │ 手动验证
 ▼
 生产部署
 │
 ▼
 监控错误（15 分钟窗口）
 │
 ├── 检测到错误 → 回滚
 └── 干净 → 完成

```


## CI 优化

当管线超过 10 分钟时，按影响顺序应用这些策略：

```
慢 CI 管线？
├── 缓存依赖
├── 并行运行作业
├── 只跑变更影响的部分
├── 用矩阵构建分片测试套件
└── 用更大 runner

```


## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "CI 太慢" | 优化管线，不要跳过。5 分钟管线能防几小时调试。 |
| "这个变更很 trivial，跳过 CI" | Trivial 变更也会破坏构建。CI 对 trivial 变更快得很。 |
| "以后再加 CI" | 没有 CI 的项目会累积损坏状态。第一天就设置。 |

## 危险信号

- 项目没有 CI 管线
- CI 失败被忽略或静音
- 在 CI 中禁用测试让管线通过
- 生产部署没有 staging 验证
- 没有回滚机制
- 密钥存在代码或 CI 配置文件中（不在密钥管理器）

## 验证

设置或修改 CI 后：

- [ ] 所有质量门禁存在（lint、类型、测试、构建、审计）
- [ ] 每个 PR 和 push to main 都触发
- [ ] 失败阻塞合并（配置了分支保护）
- [ ] CI 结果反馈到开发循环
- [ ] 密钥存在密钥管理器中
- [ ] 部署有回滚机制
- [ ] 测试套件管线 10 分钟以内
