---
name: migrate-to-shoehorn
description: 把测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。当用户提到 shoehorn、想替换测试里的 `as`，或需要部分测试数据时使用。
---

# 迁移到 Shoehorn

## 为什么用 shoehorn？

`shoehorn` 让你在测试里传入部分数据，同时让 TypeScript 满意。它用类型安全的替代写法取代 `as` 断言。

**只用于测试代码。** 绝不在生产代码里使用 shoehorn。

测试里用 `as` 的问题：

- 训练上不让你用它
- 必须手动写出目标类型
- 故意传入错误数据时要写双重 `as`（`as unknown as Type`）

## 安装

```bash
npm i @total-typescript/shoehorn
```

## 迁移模式

### 只需少量属性的大对象

之前：

```ts
type Request = {
  body: { id: string };
  headers: Record<string, string>;
  cookies: Record<string, string>;
  // ...20 more properties
};

it("gets user by id", () => {
  // Only care about body.id but must fake entire Request
  getUser({
    body: { id: "123" },
    headers: {},
    cookies: {},
    // ...fake all 20 properties
  });
});
```

之后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

it("gets user by id", () => {
  getUser(
    fromPartial({
      body: { id: "123" },
    }),
  );
});
```

### 从 `as Type` 迁移到 `fromPartial()`

之前：

```ts
getUser({ body: { id: "123" } } as Request);
```

之后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

getUser(fromPartial({ body: { id: "123" } }));
```

### 从 `as unknown as Type` 迁移到 `fromAny()`

之前：

```ts
getUser({ body: { id: 123 } } as unknown as Request); // wrong type on purpose
```

之后：

```ts
import { fromAny } from "@total-typescript/shoehorn";

getUser(fromAny({ body: { id: 123 } }));
```

## 各自的使用场景

| 函数            | 用途                                               |
| --------------- | -------------------------------------------------- |
| `fromPartial()` | 传入能通过类型检查的部分数据                       |
| `fromAny()`     | 传入故意写错的数据（保留自动补全）                 |
| `fromExact()`   | 强制要求完整对象（之后可以换成 fromPartial）       |

## 工作流

1. **收集需求**，询问用户：
   - 哪些测试文件里的 `as` 断言正在引发问题？
   - 他们是不是在处理那种只有少数属性重要的大对象？
   - 他们是否需要为错误测试传入故意写错的数据？

2. **安装并迁移**：
   - [ ] 安装：`npm i @total-typescript/shoehorn`
   - [ ] 找出带 `as` 断言的测试文件：`grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"`
   - [ ] 把 `as Type` 换成 `fromPartial()`
   - [ ] 把 `as unknown as Type` 换成 `fromAny()`
   - [ ] 加上来自 `@total-typescript/shoehorn` 的导入
   - [ ] 跑类型检查来验证
