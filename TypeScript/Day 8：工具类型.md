> 学习日期：2026-04-10（周四）

## 学习目标

1. 掌握 TypeScript 内置工具类型
2. 能够自定义工具类型
3. 熟练运用工具类型处理 Vue Props

---

## 1. 内置工具类型概述

TypeScript 提供了很多内置工具类型，位于 `lib.es5.d.ts` 中。

---

## 2. 属性修饰工具类型

### 2.1 Partial<T> - 全部属性可选

```typescript
// ===== Partial<T> =====

interface User {
    id: number;
    name: string;
    email: string;
}

// 所有属性变为可选
type PartialUser = Partial<User>;

// 等价于：
type PartialUserEquivalent = {
    id?: number;
    name?: string;
    email?: string;
};

// 实际应用：更新用户信息
function updateUser(id: number, updates: Partial<User>): void {
    // id 必填，updates 的属性都是可选的
}

updateUser(1, { name: "新名字" });  // 只更新 name
```

### 2.2 Required<T> - 全部属性必填

```typescript
// ===== Required<T> =====

interface Config {
    host?: string;
    port?: number;
}

// 所有属性变为必填
type RequiredConfig = Required<Config>;

// 等价于：
type RequiredConfigEquivalent = {
    host: string;
    port: number;
};
```

### 2.3 Readonly<T> - 全部属性只读

```typescript
// ===== Readonly<T> =====

interface Point {
    x: number;
    y: number;
}

// 所有属性变为只读
type ReadonlyPoint = Readonly<Point>;

// 等价于：
type ReadonlyPointEquivalent = {
    readonly x: number;
    readonly y: number;
};

// 实际应用：不可变的配置
const config: Readonly<Config> = {
    host: "localhost",
    port: 8080
};
// config.port = 3000;  // ❌ Error
```

### 2.4 Pick<T, K> - 选取部分属性

```typescript
// ===== Pick<T, K> =====

interface Article {
    id: number;
    title: string;
    content: string;
    author: string;
    createdAt: string;
}

// 只选取 id、title
type ArticlePreview = Pick<Article, "id" | "title">;

// 等价于：
type ArticlePreviewEquivalent = {
    id: number;
    title: string;
};
```

### 2.5 Omit<T, K> - 排除部分属性

```typescript
// ===== Omit<T, K> =====

interface User {
    id: number;
    name: string;
    password: string;
    email: string;
}

// 排除 password
type PublicUser = Omit<User, "password">;

// 等价于：
type PublicUserEquivalent = {
    id: number;
    name: string;
    email: string;
};
```

---

## 3. 联合类型工具类型

### 3.1 Extract<T, U> - 提取类型

```typescript
// ===== Extract<T, U> =====

// 从 T 中提取可以赋值给 U 的类型

type T1 = Extract<string | number, string>;   // string
type T2 = Extract<"a" | "b" | "c", "a" | "f">;  // "a"
type T3 = Extract<number | string | boolean, string | boolean>;  // string | boolean
```

### 3.2 Exclude<T, U> - 排除类型

```typescript
// ===== Exclude<T, U> =====

// 从 T 中排除可以赋值给 U 的类型

type T1 = Exclude<string | number, string>;   // number
type T2 = Exclude<"a" | "b" | "c", "a">;  // "b" | "c"
type T3 = Exclude<number | string | boolean, string | boolean>;  // number
```

### 3.3 NonNullable<T> - 排除 null/undefined

```typescript
// ===== NonNullable<T> =====

type T1 = NonNullable<string | null | undefined>;  // string
type T2 = NonNullable<string[] | null | undefined>;  // string[]
```

---

## 4. 函数工具类型

### 4.1 Parameters<T> - 获取参数类型

```typescript
// ===== Parameters<T> =====

function createUser(name: string, age: number, email: string) {
    return { name, age, email };
}

type CreateUserParams = Parameters<typeof createUser>;
// [string, number, string]

// 提取单个参数
type FirstParam = Parameters<typeof createUser>[0];  // string
```

### 4.2 ReturnType<T> - 获取返回类型

```typescript
// ===== ReturnType<T> =====

function getUser() {
    return { id: 1, name: "张三" };
}

type User = ReturnType<typeof getUser>;
// { id: number; name: string }
```

### 4.3 ConstructorParameters<T> - 获取构造函数参数

```typescript
// ===== ConstructorParameters<T> =====

class User {
    constructor(
        public name: string,
        public age: number
    ) {}
}

type UserConstructorParams = ConstructorParameters<typeof User>;
// [string, number]
```

---

## 5. 实例类型工具类型

### 5.1 InstanceType<T> - 获取实例类型

```typescript
// ===== InstanceType<T> =====

class User {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}

type UserInstance = InstanceType<typeof User>;
// User
```

---

## 6. 自定义工具类型

### 6.1 基础自定义

```typescript
// ===== 自定义工具类型 =====

// 将所有属性变为字符串类型
type Stringify<T> = {
    [K in keyof T]: string;
};

// 将所有属性变为数字类型
type Numberify<T> = {
    [K in keyof T]: number;
};

// 示例
interface User {
    name: string;
    age: number;
}

type StringUser = Stringify<User>;
// { name: string; age: string }
```

### 6.2 带条件的工具类型

```typescript
// ===== 带条件的工具类型 =====

// 只处理函数类型
type FunctionKeys<T> = {
    [K in keyof T]: T[K] extends Function ? K : never;
}[keyof T];

interface User {
    id: number;
    name: string;
    getName(): string;
    getAge(): number;
}

type UserFunctionKeys = FunctionKeys<User>;
// "getName" | "getAge"
```

### 6.3 深层工具类型

```typescript
// ===== 深层 Partial =====
type DeepPartial<T> = {
    [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

interface Company {
    name: string;
    address: {
        city: string;
        street: string;
    };
}

type DeepPartialCompany = DeepPartial<Company>;
// { name?: string; address?: { city?: string; street?: string } }
```

---

## 7. Vue 3 中的实际应用

### 7.1 Props 类型

```typescript
// ===== Vue Props 使用工具类型 =====

interface User {
    id: number;
    name: string;
    email: string;
}

// 定义 props（所有属性可选）
const props = defineProps<Partial<User>>();

// 更新 props（部分属性可选）
function updateUser(updates: Partial<User>) {
    // ...
}
```

### 7.2 表单类型

```typescript
// ===== 表单验证 =====

interface FormData {
    username: string;
    email: string;
    password: string;
}

// 表单初始值
const initialForm: FormData = {
    username: "",
    email: "",
    password: ""
};

// 表单验证规则
type FormRules = Partial<{
    [K in keyof FormData]: {
        required: boolean;
        message: string;
        validator?: (value: string) => boolean;
    }
}>;

const rules: FormRules = {
    username: {
        required: true,
        message: "用户名不能为空"
    },
    email: {
        required: true,
        message: "邮箱不能为空"
    }
};
```

### 7.3 API 响应处理

```typescript
// ===== API 响应工具类型 =====

type ApiSuccess<T> = {
    code: 200;
    data: T;
};

type ApiError = {
    code: number;
    error: string;
};

type ApiResponse<T> = ApiSuccess<T> | ApiError;

// 提取成功数据类型
type SuccessData<T extends ApiResponse<any>> = 
    T extends { code: 200; data: infer D } ? D : never;
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：基础工具类型 ==========
// 1. 使用 Partial 定义可选的 User
// 2. 使用 Pick 选取 User 的 id 和 name
// 3. 使用 Omit 排除 User 的 password

// ========== 练习 2：联合类型工具 ==========
// 1. 使用 Extract 从联合类型提取字符串
// 2. 使用 Exclude 排除特定类型

// ========== 练习 3：函数工具类型 ==========
// 1. 使用 Parameters 获取函数参数类型
// 2. 使用 ReturnType 获取返回类型

// ========== 练习 4：自定义工具类型 ==========
// 1. 创建一个 DeepReadonly 类型
// 2. 创建一个 Nullable<T> 类型

// ========== 练习 5：Vue 中使用 ==========
// 1. 在 Vue 组件中使用 Partial 处理表单
// 2. 创建带验证规则的表单类型
```

---

## 🎯 今日自检清单

- [ ] Partial<T>
- [ ] Required<T>
- [ ] Readonly<T>
- [ ] Pick<T, K>
- [ ] Omit<T, K>
- [ ] Extract<T, U>
- [ ] Exclude<T, U>
- [ ] Parameters<T>
- [ ] ReturnType<T>
- [ ] 自定义工具类型

---

## 第二周总结

这周我们学习了 TypeScript 高级特性：
1. ✅ 高级类型（联合、交叉、条件）
2. ✅ 类型推断与守卫
3. ✅ 模块与命名空间
4. ✅ 工具类型

**下一步**：开始 Vue 3 + TypeScript 实战项目！

---

## 下周预告

- Day 10: Vue 3 项目搭建与 TypeScript 配置
- Day 11: Vue 3 Composition API 与 TypeScript
- Day 12: Vue 3 组件类型定义
- Day 13: Vue Router + TypeScript
- Day 14: Pinia 状态管理 + TypeScript
