# Performance Optimization（性能优化）

## 概述

优化前先测量。没有测量的性能工作就是猜——而猜会导致过早优化，增加复杂度却不改善关键部分。先分析，找真正瓶颈，修复，再测量。只优化测量证明重要的部分。

## 什么时候用

- 规格中有性能需求（加载时间预算、响应时间 SLA）
- 用户或监控报告慢行为
- Core Web Vitals 分数低于阈值
- 你怀疑一个变更引入了回归
- 构建处理大数据集或高流量的功能

**什么时候不用：** 没有问题证据前不要优化。过早优化增加的复杂度比它获得的性能更贵。

## Core Web Vitals 目标

| 指标 | 好 | 需要改进 | 差 |
|------|---|---------|---|
| **LCP** (Largest Contentful Paint) | ≤ 2.5s | ≤ 4.0s | > 4.0s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | ≤ 500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | ≤ 0.25 | > 0.25 |

## 优化工作流

```
1. 测量 → 用真实数据建立基线
2. 识别 → 找到真正瓶颈（不是假设的）
3. 修复 → 解决特定瓶颈
4. 验证 → 再次测量，确认改善
5. 守卫 → 加监控或测试防止回归

```


### 步骤 1：测量

两种互补方法——都用：

- **合成（Lighthouse、DevTools Performance 标签）：** 可控条件，可复现。最适合 CI 回归检测和隔离特定问题。
- **RUM（web-vitals 库、CrUX）：** 真实条件下的真实用户数据。必须用来验证修复确实改善了用户体验。

**前端：**

```bash
import { onLCP, onINP, onCLS } from 'web-vitals';

onLCP(console.log);
onINP(console.log);
onCLS(console.log);

```


**后端：**

```bash
console.time('db-query');
const result = await db.query(...);
console.timeEnd('db-query');

```


### 从哪里开始测量

用症状决定先测什么：

```
什么慢？
├── 首次页面加载
│ ├── 包太大？ → 测 bundle 大小，检查代码分割
│ ├── 服务器响应慢？ → 在 DevTools Network 瀑布图测 TTFB
│ └── 渲染阻塞资源？ → 检查 CSS/JS 阻塞
├── 交互感觉迟钝
│ ├── 点击时 UI 冻结？ → 分析主线程，查找长任务（>50ms）
│ └── 表单输入延迟？ → 检查重渲染
└── 后端 / API
 ├── 单端点慢？ → 分析数据库查询，检查索引
 └── 所有端点慢？ → 检查连接池、内存、CPU

```


### 步骤 3：修复常见反模式

#### N+1 查询（后端）

```typescript
// BAD: N+1
const tasks = await db.tasks.findMany();
for (const task of tasks) {
 task.owner = await db.users.findUnique({ where: { id: task.ownerId } });
}

// GOOD: 单查询 join/include
const tasks = await db.tasks.findMany({
 include: { owner: true },
});

```


#### 无界数据获取

```typescript
// BAD: 取所有记录
const allTasks = await db.tasks.findMany();

// GOOD: 分页带限制
const tasks = await db.tasks.findMany({
 take: 20,
 skip: (page - 1) * 20,
 orderBy: { createdAt: 'desc' },
});

```


#### 不必要的重渲染（React）

```tsx
// BAD: 每次渲染创建新对象
function TaskList() {
 return <TaskFilters options={{ sortBy: 'date', order: 'desc' }} />;
}

// GOOD: 稳定引用
const DEFAULT_OPTIONS = { sortBy: 'date', order: 'desc' } as const;
function TaskList() {
 return <TaskFilters options={DEFAULT_OPTIONS} />;
}

```


#### 大 Bundle 大小

```typescript
// GOOD: 懒加载不常用功能
const ChartLibrary = lazy(() => import('./ChartLibrary'));

// GOOD: 路由级代码分割
const SettingsPage = lazy(() => import('./pages/Settings'));

```


## 性能预算

```
JavaScript bundle: < 200KB gzipped（初始加载）
CSS: < 50KB gzipped
Images: < 200KB per image（首屏以上）
Fonts: < 100KB total
API response time: < 200ms (p95)
Lighthouse Performance score: ≥ 90

```


## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "以后再优化" | 性能债会复利增长。现在修明显反模式，推迟微优化。 |
| "我机器上很快" | 你的机器不是用户的。在代表性硬件和网络上分析。 |
| "这个优化很明显" | 你没测过就不知道。先分析。 |
| "用户不会注意 100ms" | 研究表明 100ms 延迟影响转化率。 |

## 危险信号

- 优化没有分析数据支撑
- 数据获取中有 N+1 查询模式
- 列表端点没有分页
- 图片没有尺寸、懒加载或响应式大小
- Bundle 大小增长没有审查
- 生产没有性能监控
- `React.memo` 和 `useMemo` 到处都是

## 验证

任何性能相关变更后：

- [ ] 有前后测量数据（具体数字）
- [ ] 确定并解决了特定瓶颈
- [ ] Core Web Vitals 在"好"阈值内
- [ ] Bundle 大小没有显著增长
- [ ] 新数据获取代码没有 N+1 查询
- [ ] 已有测试仍然通过
