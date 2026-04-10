> 学习日期：2026-04-02（周四）

## 学习目标

1. 掌握 TypeScript 函数类型定义
2. 理解泛型基础与实际应用
3. 能在 Vue 3 中使用函数类型和泛型

---

## 1. 函数类型

### 1.1 基础函数类型

```typescript
// ===== 函数声明的类型定义 =====

// 方式1：直接指定参数和返回类型
function add(a: number, b: number): number {
    return a + b;
}

// 方式2：函数表达式
const multiply = function(a: number, b: number): number {
    return a * b;
};

// 方式3：箭头函数
const greet = (name: string): string => {
    return `你好，${name}！`;
};

// 方式4：完整的函数类型注解
let calculate: (a: number, b: number) => number;

calculate = function(x, y) {
    return x + y;
};
```

### 1.2 可选参数与默认参数

```typescript
// ===== 可选参数（用 ? 标记）=====
function createUser(name: string, age?: number): string {
    if (age !== undefined) {
        return `${name}，${age}岁`;
    }
    return name;
}

createUser("张三");        // "张三"
createUser("李四", 25);    // "李四，25岁"

// ===== 默认参数 =====
function greet(name: string, greeting: string = "你好"): string {
    return `${greeting}，${name}！`;
}

greet("王五");            // "你好，王五！"
greet("赵六", "早上好");  // "早上好，赵六！"

// ===== 剩余参数 =====
function sum(...numbers: number[]): number {
    return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3, 4, 5);  // 15
```

### 1.3 函数重载

```typescript
// ===== 函数重载：同一函数名，不同参数类型/数量 =====

// 方式1：重载签名
function getInfo(id: number): string;           // 重载签名1
function getInfo(name: string): object;         //重载签名2
function getInfo(value: number | string): any { // 实现签名
    if (typeof value === "number") {
        return `用户ID: ${value}`;
    } else {
        return { name: value, status: "active" };
    }
}

getInfo(123);        // "用户ID: 123"
getInfo("张三");      // { name: "张三", status: "active" }

// 方式2：联合类型（简单场景）
function process(value: string | string[]): string {
    if (Array.isArray(value)) {
        return value.join(", ");
    }
    return value;
}
```

### 1.4 this 类型

```typescript
// ===== this 类型定义 =====

// 在普通函数中，this 需要显式声明
function getFullName(this: { firstName: string; lastName: string }) {
    return this.firstName + " " + this.lastName;
}

const person = {
    firstName: "张",
    lastName: "三",
    getFullName
};

person.getFullName();  // "张三"

// ===== 箭头函数中的 this =====
class User {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    // 箭头函数自动捕获外 this
    getName = () => {
        return this.name;
    };
}
```

---

## 2. 泛型（Generics）

泛型让代码可以适用于多种类型，同时保持类型安全。

### 2.1 泛型基础

```typescript
// ===== 不用泛型：每个类型写一个函数 =====
function identityString(arg: string): string {
    return arg;
}

function identityNumber(arg: number): number {
    return arg;
}

// ===== 用泛型：一个函数搞定所有类型 =====
function identity<T>(arg: T): T {
    return arg;
}

identity<string>("hello");    // 指定类型参数
identity(123);                // 自动推断
identity(true);               // 推断为 boolean
```

### 2.2 泛型接口与类

```typescript
// ===== 泛型接口 =====
interface Container<T> {
    value: T;
    getValue(): T;
}

const stringContainer: Container<string> = {
    value: "Hello",
    getValue() {
        return this.value;
    }
};

const numberContainer: Container<number> = {
    value: 42,
    getValue() {
        return this.value;
    }
};

// ===== 泛型类 =====
class Box<T> {
    private content: T;
    
    constructor(content: T) {
        this.content = content;
    }
    
    get(): T {
        return this.content;
    }
    
    set(value: T): void {
        this.content = value;
    }
}

const box = new Box<string>("书籍");
box.get();  // "书籍"
box.set("玩具");
```

### 2.3 泛型约束

```typescript
// ===== 约束：让泛型必须满足某些条件 =====

// 方式1：extends 约束
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(arg: T): void {
    console.log(arg.length);
}

logLength("hello");      // 5，字符串有 length
logLength([1, 2, 3]);    // 3，数组有 length
logLength({ length: 10 }); // 10，对象有 length
// logLength(123);        // ❌ Error：数字没有 length

// 方式2：keyof 约束
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person = { name: "张三", age: 25 };
getProperty(person, "name");   // "张三"
getProperty(person, "age");   // 25
// getProperty(person, "xxx"); // ❌ Error

// 方式3：多重约束
interface Name {
    name: string;
}

interface Age {
    age: number;
}

function greet<T extends Name & Age>(obj: T): string {
    return `我叫${obj.name}，今年${obj.age}岁`;
}

greet({ name: "李四", age: 30 });  // "我叫李四，今年30岁"
```

### 2.4 常用泛型模式

```typescript
// ===== 泛型工具函数 =====

// 1. 返回 Promise
async function fetchData<T>(url: string): Promise<T> {
    const response = await fetch(url);
    return response.json();
}

// 2. 数组处理
function firstElement<T>(arr: T[]): T | undefined {
    return arr[0];
}

const nums = [1, 2, 3];
firstElement(nums);           // 1

const strs = ["a", "b"];
firstElement(strs);           // "a"

// 3. 交换两个值
function swap<T, U>(a: T, b: U): [U, T] {
    return [b, a];
}

swap(1, "one");  // ["one", 1]

// 4. 创建指定长度的数组
function createArray<T>(length: number, value: T): T[] {
    return Array(length).fill(value);
}

createArray(3, "x");  // ["x", "x", "x"]
```

---

## 3. Vue 3 中的实际应用

### 3.1 泛型组件

```typescript
// ===== Vue 3 泛型组件示例 =====

// 父组件
<template>
  <List :items="users">
    <template #item="{ item }">
      <span>{{ item.name }}</span>
    </template>
  </List>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import List from './List.vue'

interface User {
  id: number
  name: string
}

const users = ref<User[]>([
  { id: 1, name: '张三' },
  { id: 2, name: '李四' }
])
</script>

// List.vue 泛型实现
<script setup lang="ts" generic="T">
import { defineProps } from 'vue'

interface Props<T> {
  items: T[]
}

const props = defineProps<Props<T>>()
</script>
```

### 3.2 泛型 API 请求

```typescript
// ===== 通用 API 请求函数 =====
interface ApiResponse<T> {
    code: number
    data: T
    message: string
}

async function request<T>(
    url: string, 
    options?: RequestInit
): Promise<ApiResponse<T>> {
    const response = await fetch(url, options);
    return response.json();
}

// 使用
interface User {
    id: number
    name: string
}

const result = await request<User[]>('/api/users');
// result.data 是 User[] 类型
```

---

## 📝 课后练习

创建练习文件 `src/day3-exercise.ts`：

```typescript
// ========== 练习 1：函数类型 ==========
// 1. 写一个计算器函数，接受两个数字和一个操作符（+、-、*、/）
// 2. 写一个可选参数的问候函数，参数：name、greeting（默认"你好"）

// ========== 练习 2：泛型基础 ==========
// 1. 写一个 identity 函数，接受任意类型，返回相同类型
// 2. 写一个 getArrayLength 函数，接受数组，返回长度

// ========== 练习 3：泛型约束 ==========
// 1. 写一个函数，接受有 name 属性的对象，返回 name
// 2. 写一个 merge 函数，合并两个对象

// ========== 练习 4：Vue 中使用泛型 ==========
// 定义一个泛型接口 User，包含 id 和 name
// 写一个函数返回 Promise<User[]>
```

---

## 🎯 今日自检清单

- [ ] 函数参数类型和返回类型定义
- [ ] 可选参数、默认参数、剩余参数
- [ ] 泛型基础 `<T>`
- [ ] 泛型约束 `extends`
- [ ] 在 Vue 中使用泛型

---

## 下一步

明天我们将学习：**类与装饰器**
