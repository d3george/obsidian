---
source: cursor-plan
---


---
name: portable-rest-query-drizzle
overview: 一个可嵌入任意 Node 服务的开源库：实现 Payload 风格的 REST 资源接口（find/findById/count/create/updateById/deleteById）与查询参数（where/select/sort/page/limit/include），协议层 ORM 无关，首个适配器支持 Postgres+Drizzle，并内置字段白名单、复杂度预算与一致错误模型。
todos: []
isProject: false
---

# 设计方案：Portable REST Query（Payload 风格）+ Drizzle 适配器

## 1. 背景与目标

你欣赏 Payload 的 REST API 设计（统一 CRUD 端点 + 强查询参数），但不一定想引入 Payload 整个平台。本方案把“通用数据 API 能力”抽出来做成一个可复用、可控、可裁剪的开源库。

- **项目类型**：库（library），不是框架/服务。
- **首要支持**：Postgres + Drizzle。
- **可集成**：任意 Node HTTP 框架（Express/Hono/Fastify/TanStack Start server 等）。
- **可控性目标**：
  - 字段白名单（select/sort/filter/ include）
  - 复杂度预算（防滥用、防慢查询）
  - 明确的扩展点（policy / hooks / adapter）

## 2. 核心概念

### 2.1 协议层（Protocol） vs 适配器层（Adapter）

- **Protocol**：解析请求参数 -> 生成 AST（抽象语法树）+ 执行计划（plan）。不依赖 ORM。
- **Adapter**：把 AST/plan 编译成具体 ORM 查询（Drizzle），并负责批量加载 include 关系、组装嵌套结果。

### 2.2 Resource Registry（类似 Payload collections，但更窄）

你需要注册资源（collection）以声明允许的查询能力。

最小信息：

- `name`：资源名（如 `modules`）
- `id`：主键字段
- `fields`：字段类型与能力白名单（selectable/sortable/filterable）
- `relations`：可 include 的关系（路径、外键、目标资源、最大深度）
- `policy`：访问控制（读/写/字段级过滤）

## 3. API 形态（v0.1）

### 3.1 路由

- Find：`GET /api/:collection`
- FindByID：`GET /api/:collection/:id`
- Count：`GET /api/:collection/count`
- Create：`POST /api/:collection`
- UpdateByID：`PATCH /api/:collection/:id`
- DeleteByID：`DELETE /api/:collection/:id`

> v0.1 建议先以“只读 + count”为主，写操作在 v0.2 打开。原因：where + 写操作是最容易被滥用的部分。

### 3.2 查询参数（Payload 风格，但裁剪）

- `limit`：每页数量（默认 20，上限 100）
- `page`：页码（默认 1）
- `sort`：`sort=title,-createdAt`
- `select`：`select=title,slug,createdAt`
- `where`：结构化过滤（建议 JSON）
- `include`：替代 `depth` 的显式展开：`include=sections,sections.lessons`

暂缓：`locale/fallback-locale`、`joins`、无限深度 `depth`。

## 4. where 的格式选择（推荐 JSON）

### 4.1 为什么推荐 JSON

- 可安全解析（无 SQL 注入）
- 表达 and/or 与操作符清晰
- 更容易做复杂度预算

### 4.2 where JSON 示例

- 字段等值：

```json
{"title": {"equals": "Hello"}}
```

- 组合条件：

```json
{"and": [
  {"state": {"equals": "published"}},
  {"rank": {"gte": "0|hzzzz:"}}
]}
```

- in：

```json
{"moduleType": {"in": ["workshop", "tutorial"]}}
```

### 4.3 操作符范围（第一版）

- string：`equals, not_equals, contains, starts_with, in, not_in, is_null`
- number/date：`equals, gt, gte, lt, lte, in, is_null`
- enum：`equals, in`

并通过 Resource 注册表限制：只有 `filterable` 字段可出现在 where。

## 5. include（关系展开）策略

目标：像 Payload 的 `depth/populate` 一样“好用”，但更可控。

- 采用 `include`（显式路径）
- 全局限制：最大 2 层（可配置）
- Adapter 实现：**批量加载**（避免 N+1）

示例：

- `/api/modules?include=sections,sections.lessons`

执行计划：

1. 查询 modules 列表 -> 得到 moduleIds
2. 一次性查询 sections `WHERE module_id IN (...)`
3. 一次性查询 lessons `WHERE section_id IN (...)`
4. 内存组装：module.sections[].lessons[]

## 6. 安全与性能：复杂度预算（必须）

建议内置默认预算：

- `limit <= 100`
- `where.maxDepth <= 4`
- `where.maxClauses <= 20`
- `include.maxPaths <= 5`
- `include.maxDepth <= 2`
- `sort.maxFields <= 3`

失败返回 400，错误码例如 `QUERY_TOO_COMPLEX`。

## 7. 错误模型（统一 JSON）

建议格式：

```json
{
  "errors": [
    {"code": "INVALID_QUERY", "message": "...", "path": "where.and[0].state"}
  ]
}
```

并规范：

- 400：参数/解析/超预算
- 401：未登录
- 403：无权限
- 404：资源或记录不存在
- 409：唯一冲突（写操作）

## 8. 关键类型与接口（TypeScript 设计）

### 8.1 Resource 定义

```ts
export type FieldType =
  | "string"
  | "number"
  | "boolean"
  | "date"
  | "enum"
  | "json";

export type FieldCapabilities = {
  selectable?: boolean;
  sortable?: boolean;
  filterable?: boolean;
};

export type FieldDef = {
  type: FieldType;
  caps: FieldCapabilities;
};

export type PolicyCtx = {
  user?: { id: string; roles: string[] };
};

export type ResourcePolicy = {
  canRead?: (ctx: PolicyCtx) => boolean;
  canWrite?: (ctx: PolicyCtx) => boolean;
  // 字段级裁剪
  filterSelect?: (ctx: PolicyCtx, requested: string[]) => string[];
  // 记录级过滤（可选）
  addWhere?: (ctx: PolicyCtx) => unknown; // 返回 where JSON
};

export type RelationDef = {
  name: string;
  to: string;              // 目标资源名
  kind: "many" | "one";
  localKey: string;        // 本表字段（例如 module.id）
  foreignKey: string;      // 目标表字段（例如 section.moduleId）
};

export type ResourceDef = {
  name: string;
  idField: string;
  fields: Record<string, FieldDef>;
  relations?: Record<string, RelationDef>;
  policy?: ResourcePolicy;
};

export type Registry = {
  resources: Record<string, ResourceDef>;
};
```

### 8.2 Query AST（协议层输出）

```ts
export type SortItem = { field: string; dir: "asc" | "desc" };

export type QueryAst = {
  select?: string[];
  where?: unknown; // 结构化 where（JSON）
  sort?: SortItem[];
  page: number;
  limit: number;
  include?: string[]; // 路径列表："sections", "sections.lessons"
};
```

## 9. Demo：最小可运行示例（伪代码，便于你后续实现）

下面给出一个 Hono 示例（你也可以换 Express/Fastify），重点展示：注册资源 -> 解析 query -> Drizzle 执行 -> 返回。

### 9.1 注册资源（与你项目 schema 类似）

```ts
import { registry } from "portable-rest-query";

export const appRegistry = registry({
  resources: {
    modules: {
      name: "modules",
      idField: "id",
      fields: {
        id: { type: "string", caps: { selectable: true } },
        slug: { type: "string", caps: { selectable: true, filterable: true, sortable: true } },
        title: { type: "string", caps: { selectable: true, filterable: true, sortable: true } },
        moduleType: { type: "enum", caps: { selectable: true, filterable: true, sortable: true } },
        state: { type: "enum", caps: { selectable: true, filterable: true, sortable: true } },
        rank: { type: "string", caps: { selectable: true, filterable: true, sortable: true } },
        createdAt: { type: "date", caps: { selectable: true, sortable: true } },
      },
      relations: {
        sections: {
          name: "sections",
          to: "sections",
          kind: "many",
          localKey: "id",
          foreignKey: "moduleId",
        },
      },
      policy: {
        canRead: () => true,
        addWhere: (ctx) => {
          // public only published
          return ctx.user?.roles.includes("admin")
            ? {}
            : { state: { equals: "published" } };
        },
      },
    },

    sections: {
    
  name: "sections",
      idField: "id",
      fields: {
        id: { type: "string", caps: { selectable: true } },
        moduleId: { type: "string", caps: { selectable: true, filterable: true } },
        title: { type: "string", caps: { selectable: true, filterable: true, sortable: true } },
        rank: { type: "string", caps: { selectable: true, sortable: true } },
      },
      relations: {
        lessons: {
          name: "lessons",
          to: "lessons",
          kind: "many",
          localKey: "id",
          foreignKey: "sectionId",
        },
      },
    },

    lessons: {
      name: "lessons",
      idField: "id",
      fields: {
        id: { type: "string", caps: { selectable: true } },
        sectionId: { type: "string", caps: { selectable: true, filterable: true } },
        slug: { type: "string", caps: { selectable: true, filterable: true, sortable: true } },
        title: { type: "string", caps: { selectable: true, filterable: true, sortable: true } },
        state: { type: "enum", caps: { selectable: true, filterable: true, sortable: true } },
        rank: { type: "string", caps: { selectable: true, sortable: true } },
      },
      policy: {
        canRead: () => true,
      },
    },
  },
});
```

### 9.2 Handler + Adapter（Drizzle）

```ts
import { Hono } from "hono";
import { parseQuery } from "portable-rest-query/protocol";
import { createRestHandler } from "portable-rest-query/core";
import { drizzleAdapter } from "portable-rest-query/adapter-drizzle";

// 你的 drizzle 实例与表 schema
import { db } from "./db";
import { module, section, lesson } from "./schema";

const adapter = drizzleAdapter({
  db,
  tables: {
    modules: module,
    sections: section,
    lessons: lesson,
  },
});

const handler = createRestHandler({
  registry: appRegistry,
  adapter,
  options: {
    maxLimit: 100,
    maxWhereDepth: 4,
    maxWhereClauses: 20,
    maxIncludeDepth: 2,
    maxIncludePaths: 5,
  },
});

export const app = new Hono();

app.get("/api/:collection", async (c) => {
  const ast = parseQuery(c.req.query());
  const ctx = { user: c.get("user") };
  const collection = c.req.param("collection");
  const result = await handler.find({ collection, ast, ctx });
  return c.json(result);
});

app.get("/api/:collection/count", async (c) => {
  const ast = parseQuery(c.req.query());
  const ctx = { user: c.get("user") };
  const collection = c.req.param("collection");
  const result = await handler.count({ collection, ast, ctx });
  return c.json(result);
});

app.get("/api/:collection/:id", async (c) => {
  const ctx = { user: c.get("user") };
  const collection = c.req.param("collection");
  const id = c.req.param("id");
  const result = await handler.findById({ collection, id, ctx });
  return c.json(result);
});
```

### 9.3 请求示例

- 列表 + 过滤 + 排序 + 字段选择：

```http
GET /api/modules?limit=10&page=1&sort=-createdAt&select=title,slug,moduleType&where={"state":{"equals":"published"}}
```

- 带 include：

```http
GET /api/modules?include=sections,sections.lessons&where={"slug":{"equals":"intro-to-node"}}
```

## 10. 版本路线图（建议）

- **v0.1（只读）**：find/findById/count + where/select/sort/pagination/include（2 层）+ 复杂度预算 + 错误模型
- **v0.2（写操作）**：create/updateById/deleteById + zod body 校验 + 409 冲突
- **v0.3**：cursor pagination（可选），全文检索扩展点（Postgres tsquery 可选）
- **v0.4**：locale（若确有需求），更复杂关系策略

## 11. 你后续实现时最容易踩的坑（提前规避）

- **where 太通用**：会变成 query planner，建议“白名单字段 + 限制操作符 + 复杂度预算”。
- **include 递归**：易循环、易 N+1，建议显式 include + 批量加载 + 深度上限。
- **权限合并**：policy 的 `addWhere` 要与用户 where 做 AND 合并，并在 parse/plan 阶段完成。
- **select 与 include**：select 只作用于主资源；include 子资源可后续加 `populate` 细粒度控制。

## 12. 需要你最终确认的 2 个选项（实现前）

- **where 参数格式**：`where` 用 JSON 字符串（推荐）还是更像 Payload 的 querystring DSL。
- **include 命名**：是否坚持 Payload 的 `depth/populate` 术语，还是用更直观的 `include`。
