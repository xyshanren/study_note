
> 学习日期：2026-04-01（周三）

## 学习目标

1. 掌握 Interface 的定义与使用
2. 理解 Interface 与 Type Alias 的区别
3. 掌握可选属性、只读属性、索引签名

---

## 1. Interface（接口）

接口用于定义对象的结构，是 TypeScript 的核心特性。

### 1.1 基础定义

```typescript
// ===== 定义一个 Person 接口 =====
interface Person {
    name: string;
    age: number;
    email: string;
}

// 使用接口
const user: Person = {
    name: "张三",
    age: 25,
    email: "zhangsan@example.com"
};

// ❌ 缺少属性会报错
const badUser: Person = {
    name: "李四"
    // Error: Property 'age' is missing
};
```

### 1.2 可选属性（Optional Properties）

```typescript
interface Product {
    id: number;
    name: string;
    price: number;
    description?: string;  // 可选属性（用 ? 标记）
    category?: string;
}

const product1: Product = {
    id: 1,
    name: "iPhone",
    price: 6999
    // description 和 category 可以省略
};

const product2: Product = {
    id: 2,
    name: "MacBook",
    price: 12999,
    description: "M3 芯片笔记本电脑",
    category: "电子产品"
};
```

### 1.3 只读属性（Readonly Properties）

```typescript
interface Config {
    readonly apiKey: string;  // 只读，创建后不能修改
    readonly version: string;
    timeout: number;          // 可读写
}

const config: Config = {
    apiKey: "abc123",
    version: "1.0.0",
    timeout: 5000
};

config.timeout = 10000;      // ✅ OK
// config.apiKey = "xyz";    // ❌ Error: Cannot assign to 'apiKey'

// 只读数组
interface TodoList {
    readonly items: string[];
}

const list: TodoList = {
    items: ["学习", "工作"]
};

list.items.push("休息");     // ✅ 可以修改数组内容
// list.items = ["新列表"];  // ❌ Error: 不能重新赋值整个数组
```

### 1.4 索引签名（Index Signatures）

```typescript
// ===== 动态属性名 =====

// 任意字符串键，值都是 number
interface Scores {
    [subject: string]: number;
}

const scores: Scores = {
    math: 90,
    english: 85,
    chinese: 92
};

// 混合类型：既有固定属性，又有动态属性
interface UserProfile {
    name: string;
    [key: string]: string | number;  // 其他属性可以是 string 或 number
}

const profile: UserProfile = {
    name: "张三",
    age: 25,           // ✅ number
    city: "北京"       // ✅ string
};

// 数字索引签名（数组风格）
interface StringArray {
    [index: number]: string;
}

const arr: StringArray = ["a", "b", "c"];
console.log(arr[0]);  // "a"
```

### 1.5 接口继承（Extending Interfaces）

```typescript
interface Animal {
    name: string;
    age: number;
}

interface Dog extends Animal {
    breed: string;
    bark(): void;
}

const myDog: Dog = {
    name: "旺财",
    age: 3,
    breed: "金毛",
    bark() {
        console.log("汪汪！");
    }
};

// 多重继承
interface Flyable {
    fly(): void;
}

interface Swimmable {
    swim(): void;
}

interface Duck extends Animal, Flyable, Swimmable {
    color: string;
}
```

---

## 2. Type Alias（类型别名）

类型别名可以给任意类型起一个新名字。

### 2.1 基础用法

```typescript
// ===== 基础类型别名 =====
type UserID = string;
type Point = [number, number];  // 元组
type Callback = (data: string) => void;

const id: UserID = "user_123";
const origin: Point = [0, 0];

// ===== 对象类型别名 =====
type User = {
    name: string;
    age: number;
    isActive?: boolean;
};

const user: User = {
    name: "李四",
    age: 30
};

// ===== 联合类型 =====
type Status = "pending" | "success" | "error";
type ID = string | number;

let status: Status = "success";
let userId: ID = 123;  // 或 "abc"

// ===== 交叉类型 =====
type Employee = {
    name: string;
    id: number;
};

type Manager = {
    department: string;
    reports: Employee[];
};

type ManagerEmployee = Employee & Manager;  // 同时拥有两个类型的属性

const manager: ManagerEmployee = {
    name: "王经理",
    id: 1001,
    department: "技术部",
    reports: []
};
```

### 2.2 高级用法

```typescript
// ===== 条件类型 =====
type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">;   // true
type B = IsString<123>;        // false

// ===== 映射类型 =====
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type Partial<T> = {
    [P in keyof T]?: T[P];
};

interface Todo {
    title: string;
    description: string;
}

const readonlyTodo: Readonly<Todo> = {
    title: "学习 TypeScript",
    description: "掌握接口和类型别名"
};
// readonlyTodo.title = "新标题";  // ❌ Error
```

---

## 3. Interface vs Type Alias 对比

| 特性 | Interface | Type Alias |
|------|-----------|------------|
| **定义对象结构** | ✅ 推荐 | ✅ 可以 |
| **定义联合/交叉类型** | ❌ 不可以 | ✅ 可以 |
| **定义原始类型别名** | ❌ 不可以 | ✅ 可以 |
| **定义元组** | ❌ 不可以 | ✅ 可以 |
| **扩展方式** | `extends` 继承 | `&` 交叉类型 |
| **重复声明** | ✅ 自动合并 | ❌ 报错 |
| **性能** | 稍快（声明合并优化） | 正常 |

### 3.1 代码对比

```typescript
// ===== Interface 的声明合并 =====
interface Window {
    myLib: any;
}

interface Window {
    customProp: string;
}

// 最终 Window 包含 myLib 和 customProp

// ===== Type Alias 会报错 =====
type MyType = { a: string };
// type MyType = { b: string };  // ❌ Error: Duplicate identifier

// ===== 扩展方式对比 =====
// Interface 用 extends
interface Animal {
    name: string;
}
interface Dog extends Animal {
    breed: string;
}

// Type Alias 用 &
type Animal2 = { name: string };
type Dog2 = Animal2 & { breed: string };
```

### 3.2 选择建议

```typescript
// ✅ 用 Interface 的场景：
// 1. 定义对象/类的结构
// 2. 需要声明合并（如扩展第三方库类型）
// 3. 面向对象编程风格

interface User {
    name: string;
}

// ✅ 用 Type Alias 的场景：
// 1. 联合类型、交叉类型
// 2. 原始类型别名
// 3. 元组类型
// 4. 复杂类型运算

type Status = "active" | "inactive";
type Point = [number, number];
type NonNullable<T> = T extends null | undefined ? never : T;
```

---

## 4. 实用技巧

### 4.1 接口实现类

```typescript
interface Printable {
    print(): void;
    getContent(): string;
}

class Document implements Printable {
    private content: string;
    
    constructor(content: string) {
        this.content = content;
    }
    
    print(): void {
        console.log(this.content);
    }
    
    getContent(): string {
        return this.content;
    }
}
```

### 4.2 函数接口

```typescript
// 定义函数结构
interface SearchFunc {
    (source: string, subString: string): boolean;
}

const mySearch: SearchFunc = function(src, sub) {
    return src.search(sub) > -1;
};

// 带属性的函数
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    const counter = function(start: number) {
        return String(start);
    } as Counter;
    counter.interval = 123;
    counter.reset = function() {};
    return counter;
}
```

---

## 📝 课后练习

创建练习文件 `src/day2-exercise.ts`：

```typescript
// ========== 练习 1：定义接口 ==========
// 定义一个 Book 接口，包含：
// - title（字符串，必填）
// - author（字符串，必填）
// - pages（数字，必填）
// - isAvailable（布尔值，可选）
// - readonly isbn（字符串，只读）

// 创建一个 Book 对象

// ========== 练习 2：索引签名 ==========
// 定义一个 Dictionary 接口，键是字符串，值也是字符串
// 用它来存储翻译字典

// ========== 练习 3：接口继承 ==========
// 定义 Shape 接口（有 color 属性）
// 定义 Circle 接口继承 Shape（有 radius 属性）
// 定义 Rectangle 接口继承 Shape（有 width 和 height 属性）

// ========== 练习 4：类型别名 ==========
// 定义一个 Response 类型别名，可以是：
// { status: "success", data: any }
// 或 { status: "error", message: string }

// ========== 练习 5：Interface vs Type ==========
// 用 Interface 定义一个 Vehicle
// 用 Type Alias 定义一个 VehicleType（内容相同）
// 说明它们的使用场景区别
```

---

## 🎯 今日自检清单

- [x] Interface 基础定义
- [x] 可选属性 `?` 和只读属性 `readonly`
- [x] 索引签名 `[key: string]: type`
- [x] 接口继承 `extends`
- [ ] Type Alias 基础用法
- [ ] 联合类型 `|` 和交叉类型 `&`
- [ ] Interface vs Type Alias 的区别和使用场景

---

## 下一步

明天我们将学习：**函数与泛型**