> 学习日期：2026-04-08（周二）

## 学习目标

1. 理解 TypeScript 类型推断机制
2. 掌握类型守卫技巧
3. 能够处理复杂联合类型

---

## 1. 类型推断（Type Inference）

TypeScript 会根据上下文自动推断变量的类型。

### 1.1 变量类型推断

```typescript
// ===== 变量声明时的推断 =====

// 初始值决定类型
let x = 3;        // 推断为 number
let y = "hello";  // 推断为 string
let z = true;     // 推断为 boolean

// 数组
const arr = [1, 2, 3];     // number[]
const mixed = [1, "two", 3]; // (string | number)[]

// 对象
const user = {
    name: "张三",
    age: 25
};
// 推断为 { name: string; age: number }
```

### 1.2 函数返回值推断

```typescript
// ===== 函数返回值推断 =====

// 明确返回类型
function add(a: number, b: number): number {
    return a + b;
}

// TypeScript 推断返回值
function multiply(a: number, b: number) {
    return a * b;  // 推断为 number
}

// 条件语句的推断
function getValue(value: string | number) {
    if (typeof value === "string") {
        // 在这个分支，TypeScript 知道 value 是 string
        return value.toUpperCase();
    }
    // 在这里，TypeScript 知道 value 是 number
    return value.toFixed(2);
}
```

### 1.3 上下文推断

```typescript
// ===== 上下文类型推断 =====

// 回调函数的参数类型推断
const numbers = [1, 2, 3];

numbers.forEach((num) => {
    // num 自动推断为 number
    console.log(num * 2);
});

// 对象方法的推断
const config = {
    host: "localhost",
    port: 8080,
    connect() {
        // this 自动推断为 { host: string; port: number; connect(): void }
        return `http://${this.host}:${this.port}`;
    }
};
```

### 1.4 as const 断言

```typescript
// ===== as const：让类型更精确 =====

// 不使用 as const
const colors1 = ["red", "green", "blue"];
// 类型: string[]

// 使用 as const
const colors2 = ["red", "green", "blue"] as const;
// 类型: readonly ["red", "green", "blue"]

// 应用场景：确保枚举值类型安全
const HTTP_STATUS = {
    OK: 200,
    NOT_FOUND: 404,
    SERVER_ERROR: 500
} as const;

type StatusCode = typeof HTTP_STATUS[keyof typeof HTTP_STATUS];
// 类型: 200 | 404 | 500
```

---

## 2. 类型守卫（Type Guards）

类型守卫是运行时检查，确保变量在特定分支中是特定类型。

### 2.1 typeof 守卫

```typescript
// ===== typeof 基本用法 =====

function print(value: string | number | boolean): void {
    if (typeof value === "string") {
        // value 是 string 类型
        console.log(value.toUpperCase());
    } else if (typeof value === "number") {
        // value 是 number 类型
        console.log(value.toFixed(2));
    } else {
        // value 是 boolean 类型
        console.log(value ? "是" : "否");
    }
}

// typeof 可以识别的类型：string, number, boolean, undefined, function, object
```

### 2.2 instanceof 守卫

```typescript
// ===== instanceof 用法 =====

class Dog {
    bark(): void {
        console.log("汪汪！");
    }
}

class Cat {
    meow(): void {
        console.log("喵喵！");
    }
}

function makeSound(animal: Dog | Cat): void {
    if (animal instanceof Dog) {
        animal.bark();
    } else {
        animal.meow();
    }
}
```

### 2.3 in 守卫

```typescript
// ===== in 操作符守卫 =====

interface Fish {
    swim(): void;
    kind: "fish";
}

interface Bird {
    fly(): void;
    kind: "bird";
}

function move(animal: Fish | Bird): void {
    if ("swim" in animal) {
        animal.swim();
    } else {
        animal.fly();
    }
}
```

### 2.4 自定义类型守卫

```typescript
// ===== 自定义类型守卫 =====

interface Animal {
    name: string;
}

interface Fish extends Animal {
    swim(): void;
}

interface Bird extends Animal {
    fly(): void;
}

// 返回类型谓词：parameterName is Type
function isFish(animal: Animal): animal is Fish {
    return (animal as Fish).swim !== undefined;
}

function isBird(animal: Animal): animal is Bird {
    return (animal as Bird).fly !== undefined;
}

function move(animal: Animal): void {
    if (isFish(animal)) {
        animal.swim();
    } else if (isBird(animal)) {
        animal.fly();
    }
}
```

### 2.5 可辨识联合（Discriminated Unions）

```typescript
// ===== 可辨识联合模式 =====

// 使用共同属性（可辨识标签）区分类型
type Action =
    | { type: "increment"; payload: number }
    | { type: "decrement"; payload: number }
    | { type: "reset" };

function reducer(action: Action): number {
    switch (action.type) {
        case "increment":
            return action.payload;  // payload 是 number
        case "decrement":
            return action.payload;  // payload 是 number
        case "reset":
            return 0;
    }
}
```

---

## 3. 进阶类型守卫技巧

### 3.1 never 类型与穷尽检查

```typescript
// ===== never 用于穷尽检查 =====

type Shape = 
    | { kind: "circle"; radius: number }
    | { kind: "rectangle"; width: number; height: number }
    | { kind: "triangle"; base: number; height: number };

function area(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "rectangle":
            return shape.width * shape.height;
        case "triangle":
            return 0.5 * shape.base * shape.height;
        default:
            // 如果漏掉某个 case，Shape 会变成 never
            const _exhaustive: never = shape;
            return _exhaustive;
    }
}
```

### 3.2 数组类型守卫

```typescript
// ===== 数组类型守卫 =====

// 检查数组是否为空
function firstOrDefault<T>(arr: T[], defaultValue: T): T {
    if (arr.length === 0) {
        return defaultValue;
    }
    return arr[0];
}

// 检查是否是数组
function processValue(value: unknown): void {
    if (Array.isArray(value)) {
        console.log(value.length);  // value 是 any[]
    }
}
```

### 3.3 Promise 类型守卫

```typescript
// ===== 异步类型处理 =====

async function fetchUser(id: string): Promise<{ name: string } | null> {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) {
        return null;
    }
    return response.json();
}

// 使用时
const user = await fetchUser("123");
if (user !== null) {
    console.log(user.name);  // user 不再是 null
}
```

---

## 4. Vue 3 中的实际应用

### 4.1 Props 类型守卫

```typescript
// ===== Vue Props 类型守卫 =====

interface Props {
    status: "loading" | "success" | "error";
}

const props = defineProps<Props>();

// 根据 status 显示不同内容
if (props.status === "loading") {
    // 这里 props.status 是 "loading"
}
```

### 4.2 响应式数据的类型

```typescript
// ===== ref 和 reactive 类型推断 =====

import { ref, reactive, computed } from 'vue'

// ref 推断
const count = ref(0);           // Ref<number>
const message = ref("hello");   // Ref<string>

// 显式指定类型
const items = ref<string[]>([]); // Ref<string[]>

// computed 推断
const doubleCount = computed(() => count.value * 2);  // ComputedRef<number>

// reactive 推断
const state = reactive({
    user: null as { name: string } | null,
    loading: false
});

// 类型守卫
if (state.user !== null) {
    console.log(state.user.name);  // state.user 是 { name: string }
}
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：类型推断 ==========
// 1. 创建一个数组，让 TypeScript 推断为 number[]
// 2. 使用 as const 创建一个不可变的配置对象

// ========== 练习 2：类型守卫 ==========
// 1. 写一个函数，使用 typeof 检查类型
// 2. 定义两个类，使用 instanceof 检查

// ========== 练习 3：自定义守卫 ==========
// 1. 定义一个接口 Animal 和两个子类型 Fish、Bird
// 2. 写一个 isFish 类型守卫函数

// ========== 练习 4：可辨识联合 ==========
// 1. 定义一个 Action 联合类型
// 2. 写一个 reducer 函数处理所有情况

// ========== 练习 5：Vue 中使用 ==========
// 1. 在 Vue 组件中处理 ref 可能为 null 的情况
// 2. 使用 computed 处理不同状态
```

---

## 🎯 今日自检清单

- [ ] 类型推断机制
- [ ] typeof 守卫
- [ ] instanceof 守卫
- [ ] in 守卫
- [ ] 自定义类型守卫（`is` 谓词）
- [ ] 可辨识联合
- [ ] never 穷尽检查

---

## 下一步

明天我们将学习：**模块与命名空间**
