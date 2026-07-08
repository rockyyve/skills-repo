# Security and Hardening（安全和加固）

## 概述

Web 应用的安全优先开发实践。把每个外部输入当敌意的，每个密钥当神圣的，每个授权检查当强制的。安全不是一个阶段——它是触碰用户数据、认证或外部系统的每行代码的约束。

## 什么时候用

- 构建任何接受用户输入的东西
- 实现认证或授权
- 存储或传输敏感数据
- 集成外部 API 或服务
- 添加文件上传、webhook 或回调
- 处理支付或 PII 数据

## 三级边界系统

### 始终执行（无例外）

- **在系统边界验证所有外部输入**（API 路由、表单处理器）
- **参数化所有数据库查询** — 永不拼接用户输入到 SQL
- **编码输出**防止 XSS（用框架自动转义，不要绕过）
- **所有外部通信用 HTTPS**
- **用 bcrypt/scrypt/argon2 哈希密码**（永不存明文）
- **设置安全头**（CSP、HSTS、X-Frame-Options、X-Content-Type-Options）
- **用 httpOnly、secure、sameSite cookie** 存会话
- **每次发布前运行 `npm audit`**

### 先问（需要人类批准）

- 添加新认证流程或改认证逻辑
- 存储新类别的敏感数据（PII、支付信息）
- 添加新的外部服务集成
- 改 CORS 配置
- 添加文件上传处理器
- 修改限流
- 授予提升权限或角色

### 绝不做

- **永不提交密钥**到版本控制（API key、密码、token）
- **永不记录敏感数据**（密码、token、完整信用卡号）
- **永不信任客户端验证**作为安全边界
- **永不禁用安全头**为方便
- **永不将 `eval()` 或 `innerHTML`** 用于用户提供的数据
- **永不把会话存在客户端可访问存储**（localStorage 存认证 token）
- **永不暴露堆栈跟踪**或内部错误详情给用户

## OWASP Top 10 防护

### 1. 注入（SQL、NoSQL、OS 命令）

```typescript
// BAD: SQL 注入
const query = `SELECT * FROM users WHERE id = '${userId}'`;

// GOOD: 参数化查询
const user = await db.query('SELECT * FROM users WHERE id = $1', [userId]);

// GOOD: ORM 参数化输入
const user = await prisma.user.findUnique({ where: { id: userId } });

```


### 2. 损坏的认证

```typescript
import { hash, compare } from 'bcrypt';

const SALT_ROUNDS = 12;
const hashedPassword = await hash(plaintext, SALT_ROUNDS);
const isValid = await compare(plaintext, hashedPassword);

app.use(session({
 secret: process.env.SESSION_SECRET,
 resave: false,
 saveUninitialized: false,
 cookie: {
 httpOnly: true,
 secure: true,
 sameSite: 'lax',
 maxAge: 24 * 60 * 60 * 1000,
 },
}));

```


### 3. 跨站脚本（XSS）

```typescript
// BAD: 把用户输入当 HTML 渲染
element.innerHTML = userInput;

// GOOD: 用框架自动转义
return <div>{userInput}</div>;

// 如果必须渲染 HTML，先清理
import DOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(userInput);

```


### 4. 损坏的访问控制

```typescript
app.patch('/api/tasks/:id', authenticate, async (req, res) => {
 const task = await taskService.findById(req.params.id);

 if (task.ownerId !== req.user.id) {
 return res.status(403).json({
 error: { code: 'FORBIDDEN', message: 'Not authorized to modify this task' }
 });
 }

 const updated = await taskService.update(req.params.id, req.body);
 return res.json(updated);
});

```


### 5. 安全配置错误

```typescript
import helmet from 'helmet';
app.use(helmet());

app.use(helmet.contentSecurityPolicy({
 directives: {
 defaultSrc: ["'self'"],
 scriptSrc: ["'self'"],
 styleSrc: ["'self'", "'unsafe-inline'"],
 imgSrc: ["'self'", 'data:', 'https:'],
 connectSrc: ["'self'"],
 },
}));

app.use(cors({
 origin: process.env.ALLOWED_ORIGINS?.split(',') || 'http://localhost:3000',
 credentials: true,
}));

```


## 输入验证模式

### 边界 Schema 验证

```typescript
import { z } from 'zod';

const CreateTaskSchema = z.object({
 title: z.string().min(1).max(200).trim(),
 description: z.string().max(2000).optional(),
 priority: z.enum(['low', 'medium', 'high']).default('medium'),
 dueDate: z.string().datetime().optional(),
});

app.post('/api/tasks', async (req, res) => {
 const result = CreateTaskSchema.safeParse(req.body);
 if (!result.success) {
 return res.status(422).json({
 error: {
 code: 'VALIDATION_ERROR',
 message: 'Invalid input',
 details: result.error.flatten(),
 },
 });
 }
 const task = await taskService.create(result.data);
 return res.status(201).json(task);
});

```


## npm audit 结果分诊

不是所有审计发现都需要立即行动：

```
npm audit 报告漏洞
├── 严重性：critical 或 high
│ ├── 漏洞代码在你应用中可达？
│ │ ├── 是 → 立即修复
│ │ └── 否（仅 dev 依赖） → 尽快修复但不阻塞
│ └── 有修复？
│ ├── 有 → 更新到修复版本
│ └── 没有 → 检查变通方案、考虑替换依赖
├── 严重性：moderate
│ ├── 生产可达？ → 下个发布周期修
│ └── 仅 dev？ → 方便时修，记录 backlog
└── 严重性：low
 └── 跟踪，常规依赖更新时修

```


## 限流

```typescript
import rateLimit from 'express-rate-limit';

app.use('/api/', rateLimit({
 windowMs: 15 * 60 * 1000,
 max: 100,
 standardHeaders: true,
 legacyHeaders: false,
}));

app.use('/api/auth/', rateLimit({
 windowMs: 15 * 60 * 1000,
 max: 10,
}));

```


## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "这是内部工具，安全无所谓" | 内部工具也会被攻破。攻击者瞄准最弱环节。 |
| "以后再加安全" | 安全改造比一开始构建难 10 倍。现在就加。 |
| "不会有人来攻击这个" | 自动扫描器会找到它。隐匿不是安全。 |
| "框架处理安全" | 框架提供工具，不是保证。你仍然需要正确使用它们。 |

## 危险信号

- 用户输入直接传给数据库查询、shell 命令或 HTML 渲染
- 源码或提交历史中有密钥
- API 端点没有认证或授权检查
- 缺少 CORS 配置或通配符（`*`）源
- 认证端点无限流
- 堆栈跟踪或内部错误暴露给用户
- 依赖有已知严重漏洞

## 验证

实现安全相关代码后：

- [ ] `npm audit` 没有 critical 或 high 漏洞
- [ ] 源码和 git 历史中没有密钥
- [ ] 所有用户输入在系统边界验证
- [ ] 每个受保护端点检查认证和授权
- [ ] 响应中有安全头（浏览器 DevTools 检查）
- [ ] 错误响应不暴露内部细节
- [ ] 认证端点有限流
