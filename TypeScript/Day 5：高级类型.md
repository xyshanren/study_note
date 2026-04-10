> 学习日期：2026-04-07（周一）

## 学习目标

1. 掌握联合类型与交叉类型
2. 理解条件类型
3. 学会使用映射类型

---

## 1. 联合类型（Union Types）

联合类型表示值可以是多种类型之一。

### 1.1 基础联合类型

```typescript
// ===== 联合类型基础 =====

// 字符串或数字
let value: string | number;
value = "hello";   // ✅
value = 123;       // ✅
// value = true;  // ❌ Error

// 函数参数联合类型
function printId(id: number | string): void {
    console.log(`ID: ${id}`);
}

printId(123);        // ID: 123
printId("abc123");   // ID: abc123

// 数组联合类型
let arr: (string | number)[];
arr = [1, 2, "three", 4];
```

### 1.2 字面量类型

```typescript
// ===== 字面量类型 =====

// 具体的字符串值
let status: "pending" | "success" | "error";
status = "pending";   // ✅
// status = "unknown"; // ❌

// 具体的数字值
let direction: 0 | 1 | 2 | 3;
direction = 0;  // ✅
// direction = 5; // ❌

// 结合类型别名
type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE";

function request(method: HTTPMethod, url: string): void {
    console.log(`${method} ${url}`);
}

request("GET", "/api/users");
// request("PATCH", "/api/users"); // ❌
```

---

## 2. 交叉类型（Intersection Types）

交叉类型将多个类型合并成一个类型，同时拥有所有类型的属性。

### 2.1 基础交叉类型

```typescript
// ===== 交叉类型基础 =====

interface Person {
    name: string;
    age: number;
}

interface Worker {
    company: string;
    salary: number;
}

// 既是 Person 又是 Worker
type Employee = Person & Worker;

const employee: Employee = {
    name: "张三",
    age: 30,
    company: "科技公司",
    salary: 15000
};
```

### 2.2 联合与交叉组合

```typescript
// ===== 组合使用 =====

type ID = string | number;

interface User {
    id: ID;
    name: string;
}

interface Admin {
    level: number;
    permissions: string[];
}

// 用户可能是普通用户或管理员
type UserOrAdmin = User | Admin;

function processUser(user: UserOrAdmin): void {
    // user 只能访问 User 的属性
    console.log(user.name);  // ✅
    // console.log(user.level); // ❌
    
    // 需要类型守卫
    if ("level" in user) {
        console.log(user.level);  // ✅ 在 if 分支中
    }
}
```

---

## 3. 条件类型（Conditional Types）

根据条件选择类型。

### 3.1 基础条件类型

```typescript
// ===== 基础条件类型 =====

type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">;   // true
type B = IsString<123>;        // false

// 判断是否是数组
type IsArray<T> = T extends any[] ? true : false;

type C = IsArray<string[]>;   // true
type D = IsArray<string>;     // false
```

### 3.2 常用内置条件类型

```typescript
// ===== TypeScript 内置条件类型 =====

// Extract: 从 T 中提取可以赋值给 U 的类型
type T1 = Extract<string | number, string>;  // string
type T2 = Extract<"a" | "b" | "c", "a" | "f">;  // "a"

// Exclude: 从 T 中排除可以赋值给 U 的类型
type T3 = Exclude<string | number, string>;  // number
type T4 = Exclude<"a" | "b" | "c", "a">;   // "b" | "c"

// NonNullable: 排除 null 和 undefined
type T5 = NonNullable<string | null | undefined>;  // string

// ReturnType: 获取函数返回类型
type T6 = ReturnType<() => string>;  // string

// Parameters: 获取函数参数类型
type T7 = Parameters<(a: string, b: number) => void>;  // [string, number]
```

---

## 4. 映射类型（Mapped Types）

根据已有类型创建新类型。

### 4.1 基础映射类型

```typescript
// ===== 基础映射 =====

type Props = {
    name: string;
    age: number;
    email: string;
};

// 将所有属性变为可选
type PartialProps = Partial<Props>;
// 等价于:
// { name?: string; age?: number; email?: string; }

// 将所有属性变为只读
type ReadonlyProps = Readonly<Props>;
// 等价于:
// { readonly name: string; readonly age: number; readonly email: string; }

// 提取部分属性
type PickNameAndEmail = Pick<Props, "name" | "email">;
// 等价于:
// { name: string; email: string; }

// 排除部分属性
type OmitAge = Omit<Props, "age">;
// 等价于:
// { name: string; email: string; }
```

### 4.2 自定义映射类型

```typescript
// ===== 自定义映射类型 =====

// 将所有属性变为可选并添加问号前缀
type Optional<T> = {
    [P in keyof T]?: T[P];
};

// 将所有属性变为必填
type Required<T> = {
    [P in keyof T]-?: T[P];
};

// 给所有属性添加前缀
type WithPrefix<T, P extends string> = {
    [K in keyof T as `${P}${K & string}`]: T[K];
};

interface User {
    name: string;
    age: number;
}

type PrefixedUser = WithPrefix<User, "user_">;
// user_name: string
// user_age: number
```

### 4.3 键名映射

```typescript
// ===== 键名转换 =====

// 移除可选修饰符
type RemoveOptional<T> = {
    [P in keyof T]: T[P];
};

// 键名转为大写
type UppercaseKeys<T> = {
    [K in keyof T as Uppercase<K & string>]: T[K];
};

interface Config {
    host: string;
    port: number;
}

type UppercaseConfig = UppercaseKeys<Config>;
// HOST: string
// PORT: number
```

---

## 5. 实战应用

### 5.1 API 响应类型处理

```typescript
// ===== API 响应类型 =====

interface ApiSuccess<T> {
    code: 200;
    data: T;
    message: "success";
}

interface ApiError {
    code: 400 | 401 | 403 | 404 | 500;
    data: null;
    message: string;
}

// 联合类型：成功或失败
type ApiResponse<T> = ApiSuccess<T> | ApiError;

// 提取成功时的数据类型
type SuccessData<T> = Extract<ApiResponse<T>, { code: 200 }>["data"];

// 使用示例
type UserResponse = ApiResponse<{ id: number; name: string }>;
// 可能是 { code: 200, data: { id, name }, message: "success" }
// 或 { code: 404, data: null, message: "User not found" }
```

### 5.2 表单验证类型

```typescript
// ===== 表单类型 =====

interface FormField {
    label: string;
    value: string;
    error?: string;
}

type Form<T> = {
    [K in keyof T]: FormField;
};

interface LoginForm {
    username: FormField;
    password: FormField;
}

// 自动生成表单类型
const loginForm: Form<LoginForm> = {
    username: { label: "用户名", value: "" },
    password: { label: "密码", value: "" }
};

// 提取表单字段值
type FormValues<T> = {
    [K in keyof T]: T[K]["value"];
};

type LoginValues = FormValues<LoginForm>;
// { username: string; password: string }
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：联合类型 ==========
// 1. 定义一个类型，可以是 "small" | "medium" | "large"
// 2. 写一个函数，接受 T | null，返回 T | undefined

// ========== 练习 2：交叉类型 ==========
// 1. 合并 User 和 Address 类型
// 2. 写一个函数，接受 User | Admin，处理不同角色

// ========== 练习 3：条件类型 ==========
// 1. 实现一个 NeverIfEmpty<T>，如果 T 是空对象，返回 never
// 2. 使用 Extract 从联合类型中提取特定类型

// ========== 练习 4：映射类型 ==========
// 1. 创建只包含字符串类型属性的类型
// 2. 创建将所有方法变为可选的类型
```

---

## 🎯 今日自检清单

- [ ] 联合类型 `|`
- [ ] 交叉类型 `&`
- [ ] 条件类型 `extends ? :`
- [ ] 映射类型 `[P in keyof T]`
- [ ] 内置工具类型（Partial, Pick, Omit）

---

## 下一步

明天我们将学习：**类型推断与守卫**
