# API and Interface Design（API 和接口设计）

## 概述

设计稳定的、文档良好的、难以误用的接口。好的接口让正确的事容易做、错误的事难做。这适用于 REST API、GraphQL schema、模块边界、组件 props 和任何一段代码和另一段对话的表面。

## 什么时候用

- 设计新 API 端点
- 定义模块边界或团队间契约
- 创建组件 prop 接口
- 建立影响 API 形状的数据库 schema
- 变更已有公共接口

## 核心原则

### Hyrum's Law

> 有足够多的 API 用户时，你系统的所有可观察行为都会被某人依赖，不管你在契约中承诺了什么。

设计含义：

- **对你暴露什么要有意。** 每个可观察行为都是潜在承诺。
- **不要泄漏实现细节。** 如果用户能观察到，他们就会依赖它。
- **设计时就规划废弃。** 参见 `deprecation-and-migration`。

### One-Version Rule

避免让消费者选择同一依赖或 API 的多个版本。为一次只有一个版本的世界设计——扩展而不是分叉。

### 1. 契约优先

在实现之前定义接口。契约就是规格——实现跟随。

```typescript
interface TaskAPI {
    createTask(input: CreateTaskInput): Promise<Task>
    listTasks(params: ListTasksParams): Promise<PaginatedResult<Task>>
    getTask(id: string): Promise<Task>
    updateTask(id: string, input: UpdateTaskInput): Promise<Task>
    deleteTask(id: string): Promise<void>
}

```


### 2. 一致的错误语义

选一个错误策略并到处使用：

```typescript
interface APIError {
    error: {
        code: string // 机器可读: "VALIDATION_ERROR"
        message: string // 人类可读: "Email is required"
        details?: unknown // 有帮助时的额外上下文
    }
}

// 状态码映射
// 400 → 客户端发了无效数据
// 401 → 未认证
// 403 → 已认证但未授权
// 404 → 资源未找到
// 409 → 冲突
// 422 → 验证失败
// 500 → 服务器错误（永不暴露内部细节）

```


### 3. 在边界验证

信任内部代码。在外部输入进入的系统边缘验证：

```typescript
app.post('/api/tasks', async (req, res) => {
    const result = CreateTaskSchema.safeParse(req.body)
    if (!result.success) {
        return res.status(422).json({
            error: {
                code: 'VALIDATION_ERROR',
                message: 'Invalid task data',
                details: result.error.flatten(),
            },
        })
    }

    const task = await taskService.create(result.data)
    return res.status(201).json(task)
})

```


验证应在的地方：

- API 路由处理器（用户输入）
- 表单提交处理器（用户输入）
- 外部服务响应解析（第三方数据——**始终视为不受信任**）
- 环境变量加载（配置）

### 4. 优先增加而非修改

扩展接口而不破坏已有消费者：

```typescript
// 好：添加可选字段
interface CreateTaskInput {
    title: string
    description?: string
    priority?: 'low' | 'medium' | 'high' // 后来添加，可选
    labels?: string[] // 后来添加，可选
}

// 坏：改已有字段类型或移除字段
interface CreateTaskInput {
    title: string
    // description: string; // 移除——破坏已有消费者
    priority: number // 从 string 改——破坏已有消费者
}

```


### 5. 可预测命名

| 模式      | 约定             | 示例                           |
| --------- | ---------------- | ------------------------------ |
| REST 端点 | 复数名词，无动词 | `GET /api/tasks`               |
| 查询参数  | camelCase        | `?sortBy=createdAt`            |
| 响应字段  | camelCase        | `{ createdAt, updatedAt }`     |
| 布尔字段  | is/has/can 前缀  | `isComplete`, `hasAttachments` |

## REST API 模式

### 资源设计

```
GET /api/tasks → 列出任务
POST /api/tasks → 创建任务
GET /api/tasks/:id → 获取单个任务
PATCH /api/tasks/:id → 部分更新任务
DELETE /api/tasks/:id → 删除任务

```


### 分页

```typescript
// 响应
{
 "data": [...],
 "pagination": {
 "page": 1,
 "pageSize": 20,
 "totalItems": 142,
 "totalPages": 8
 }
}

```


### 部分更新（PATCH）

接受部分对象——只更新提供的：

```typescript
PATCH /api/tasks/123
{ "title": "Updated title" } // 只 title 变，其他保留

```


## TypeScript 接口模式

### 判别联合

```typescript
type TaskStatus =
    | { type: 'pending' }
    | { type: 'in_progress'; assignee: string }
    | { type: 'completed'; completedAt: Date }
    | { type: 'cancelled'; reason: string }

function getStatusLabel(status: TaskStatus): string {
    switch (status.type) {
        case 'pending':
            return 'Pending'
        case 'in_progress':
            return `In progress (${status.assignee})`
        case 'completed':
            return `Done on ${status.completedAt}`
        case 'cancelled':
            return `Cancelled: ${status.reason}`
    }
}

```


### 输入/输出分离

```typescript
// 输入：调用者提供
interface CreateTaskInput {
    title: string
    description?: string
}

// 输出：系统返回（包含服务器生成字段）
interface Task {
    id: string
    title: string
    createdAt: Date
    updatedAt: Date
}

```


## 常见合理化

| 合理化说法           | 现实                                               |
| -------------------- | -------------------------------------------------- |
| "以后再文档化 API"   | 类型就是文档。先定义。                             |
| "现在不需要分页"     | 有人有 100+ 项时你就需要了。从一开始就加。         |
| "PATCH 复杂，用 PUT" | PUT 每次需要完整对象。PATCH 才是客户端实际想要的。 |

## 危险信号

- 端点根据条件返回不同形状
- 错误格式跨端点不一致
- 验证分散在内部代码而非在边界
- 对已有字段的破坏性变更
- 列表端点没有分页
- REST URL 中有动词（`/api/createTask`）
- 第三方 API 响应未经验证就使用

## 验证

设计 API 后：

- [ ] 每个端点有类型化的输入和输出 schema
- [ ] 错误响应遵循单一一致格式
- [ ] 验证只在系统边界发生
- [ ] 列表端点支持分页
- [ ] 新字段是增加性和可选的（向后兼容）
- [ ] 命名在所有端点间一致
