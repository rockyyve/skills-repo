# Frontend UI Engineering（前端 UI 工程）

## 概述

构建可访问、高性能且视觉精致的生产级用户界面。目标是 UI 看起来像顶级公司的设计感知工程师做的——而不是 AI 生成的。这意味着真正的设计系统遵守、适当的可访问性、深思熟虑的交互模式和没有通用的"AI 美学"。

## 什么时候用

- 构建新 UI 组件或页面
- 修改已有用户面向界面
- 实现响应式布局
- 添加交互或状态管理
- 修复视觉或 UX 问题

## 组件架构

### 文件结构

把组件相关的所有东西放在一起：

```
src/components/
 TaskList/
 TaskList.tsx # 组件实现
 TaskList.test.tsx # 测试
 TaskList.stories.tsx # Storybook stories（如果用）
 use-task-list.ts # 自定义 hook（如果复杂状态）
 types.ts # 组件特定类型

```


### 组件模式

**优先组合而非配置：**

```tsx
// 好：可组合
<Card>
 <CardHeader>
 <CardTitle>Tasks</CardTitle>
 </CardHeader>
 <CardBody>
 <TaskList tasks={tasks} />
 </CardBody>
</Card>

// 避免：过度配置
<Card title="Tasks" headerVariant="large" bodyPadding="md"
 content={<TaskList tasks={tasks} />} />

```


**保持组件聚焦：**

```tsx
export function TaskItem({ task, onToggle, onDelete }: TaskItemProps) {
 return (
 <li className="flex items-center gap-3 p-3">
 <Checkbox checked={task.done} onChange={() => onToggle(task.id)} />
 <span className={task.done ? 'line-through text-muted' : ''}>{task.title}</span>
 <Button variant="ghost" size="sm" onClick={() => onDelete(task.id)}>
 <TrashIcon />
 </Button>
 </li>
 );
}

```


**分离数据获取和展示：**

```tsx
// 容器：处理数据
export function TaskListContainer() {
 const { tasks, isLoading, error } = useTasks();
 if (isLoading) return <TaskListSkeleton />;
 if (error) return <ErrorState message="Failed to load" retry={refetch} />;
 if (tasks.length === 0) return <EmptyState message="No tasks yet" />;
 return <TaskList tasks={tasks} />;
}

// 展示：处理渲染
export function TaskList({ tasks }: { tasks: Task[] }) {
 return (
 <ul role="list" className="divide-y">
 {tasks.map(task => <TaskItem key={task.id} task={task} />)}
 </ul>
 );
}

```


## 避免 AI 美学

AI 生成的 UI 有可识别模式。全部避免：

| AI 默认 | 问题 | 生产级 |
|---|---|---|
| 紫色/靛蓝一切 | 模型默认视觉"安全"调色板 | 用项目实际调色板 |
| 过度渐变 | 视觉噪音，和大多数设计系统冲突 | 平面或匹配设计系统的微妙渐变 |
| 全部圆角 (rounded-2xl) | 最大圆角暗示"友好"但忽略角半径层级 | 设计系统的一致圆角 |
| 通用 hero 区 | 模板驱动布局，和实际内容无关 | 内容优先布局 |
| Lorem ipsum 风格文案 | 占位文本隐藏布局问题 | 真实占位内容 |
| 过大 padding 到处都是 | 平等的大 padding 破坏视觉层级 | 一致的间距比例 |

## 可访问性（WCAG 2.1 AA）

每个组件必须满足这些标准：

### 键盘导航

```tsx
// 每个交互元素必须键盘可访问
<button onClick={handleClick}>Click me</button> // ✓ 默认可聚焦
<div onClick={handleClick}>Click me</div> // ✗ 不可聚焦
<div role="button" tabIndex={0} onClick={handleClick}>Click me</div> // ✓ 但优先 <button>

```


### ARIA 标签

```tsx
<button aria-label="Close dialog"><XIcon /></button>
<label htmlFor="email">Email</label>
<input id="email" type="email" />

```


### 有意义的空状态和错误状态

```tsx
function TaskList({ tasks }: { tasks: Task[] }) {
 if (tasks.length === 0) {
 return (
 <div role="status" className="text-center py-12">
 <h3 className="mt-2 text-sm font-medium">No tasks</h3>
 <p className="mt-1 text-sm text-muted">Get started by creating a new task.</p>
 <Button className="mt-4" onClick={onCreateTask}>Create Task</Button>
 </div>
 );
 }
 return <ul role="list">...</ul>;
}

```


## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "可访问性是锦上添花" | 很多地区是法律要求，也是工程质量标准。 |
| "以后再做响应式" | 回头做响应式比一开始做难 3 倍。 |
| "AI 美学现在没关系" | 它暗示低质量。从一开始就用项目的实际设计系统。 |

## 危险信号

- 超过 200 行的组件（拆分它们）
- 内联样式或任意像素值
- 缺少错误状态、加载状态或空状态
- 没有键盘导航测试
- 仅用颜色表示状态（无文本或图标的红/绿）
- 通用"AI 外观"

## 验证

构建 UI 后：

- [ ] 组件渲染无控制台错误
- [ ] 所有交互元素键盘可访问
- [ ] 屏幕阅读器能传达页面内容和结构
- [ ] 响应式：在 320px、768px、1024px、1440px 可工作
- [ ] 加载、错误和空状态都处理了
- [ ] 遵循项目设计系统
