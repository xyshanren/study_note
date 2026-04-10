> 学习日期：2026-04-03（周五）

## 学习目标

1. 掌握 TypeScript 类的使用
2. 理解访问修饰符
3. 了解装饰器基础

---

## 1. TypeScript 类

### 1.1 基础类的定义

```typescript
// ===== 类的定义 =====
class Person {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    
    greet(): string {
        return `你好，我叫${this.name}，今年${this.age}岁`;
    }
}

const person = new Person("张三", 25);
console.log(person.greet());  // "你好，我叫张三，今年25岁"
```

### 1.2 访问修饰符

TypeScript 有三种访问修饰符：

| 修饰符 | 访问范围 |
|--------|----------|
| `public` | 公开（默认） |
| `private` | 私有（仅本类可访问） |
| `protected` | 受保护（子类可访问） |

```typescript
// ===== 访问修饰符示例 =====
class User {
    public name: string;      // 公开
    private password: string; // 私有
    protected id: number;     // 受保护
    
    constructor(name: string, password: string, id: number) {
        this.name = name;
        this.password = password;
        this.id = id;
    }
    
    // 公开方法访问私有属性
    public checkPassword(pwd: string): boolean {
        return this.password === pwd;
    }
}

const user = new User("张三", "123456", 1);
console.log(user.name);           // ✅ "张三"
// console.log(user.password);   // ❌ Error: 私有属性
// console.log(user.id);         // ❌ Error: 受保护属性

// ===== 只读属性 =====
class Config {
    public readonly apiKey: string;
    public readonly version: string = "1.0.0";
    
    constructor(apiKey: string) {
        this.apiKey = apiKey;
        // this.apiKey = "新值";  // ❌ Error: 只读属性不能修改
    }
}
```

### 1.3 继承

```typescript
// ===== 类的继承 =====
class Animal {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    speak(): void {
        console.log(`${this.name} 发出了声音`);
    }
}

class Dog extends Animal {
    breed: string;
    
    constructor(name: string, breed: string) {
        super(name);  // 调用父类构造函数
        this.breed = breed;
    }
    
    // 重写父类方法
    speak(): void {
        console.log(`${this.name}（${this.breed}）汪汪叫`);
    }
}

const dog = new Dog("旺财", "金毛");
dog.speak();  // "旺财（金毛）汪汪叫"
```

### 1.4 抽象类

抽象类不能直接实例化，只能被继承：

```typescript
// ===== 抽象类 =====
abstract class Shape {
    abstract area(): number;  // 抽象方法，子类必须实现
    
    describe(): string {
        return `这是一个${this.constructor.name}，面积是${this.area()}`;
    }
}

class Circle extends Shape {
    radius: number;
    
    constructor(radius: number) {
        super();
        this.radius = radius;
    }
    
    area(): number {
        return Math.PI * this.radius ** 2;
    }
}

class Rectangle extends Shape {
    width: number;
    height: number;
    
    constructor(width: number, height: number) {
        super();
        this.width = width;
        this.height = height;
    }
    
    area(): number {
        return this.width * this.height;
    }
}

const circle = new Circle(5);
const rect = new Rectangle(4, 6);

console.log(circle.area());  // 78.54
console.log(rect.area());   // 24
```

### 1.5 Getter 和 Setter

```typescript
// ===== Getter / Setter =====
class User {
    private _name: string = "";
    
    // Getter
    get name(): string {
        return this._name;
    }
    
    // Setter（可以做验证）
    set name(value: string) {
        if (value.length < 2) {
            throw new Error("名字至少2个字符");
        }
        this._name = value;
    }
}

const user = new User();
user.name = "张三";     // 调用 setter
console.log(user.name); // "张三" 调用 getter
```

---

## 2. 装饰器（Decorators）

装饰器是一种特殊语法，可以类、方法、属性添加元数据或功能。

### 2.1 开启装饰器

需要在 `tsconfig.json` 中开启：

```json
{
    "compilerOptions": {
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true
    }
}
```

### 2.2 类装饰器

```typescript
// ===== 类装饰器 =====
function Logger(constructor: Function) {
    console.log(`创建类: ${constructor.name}`);
}

@Logger
class Person {
    constructor(public name: string) {}
}
// 输出: "创建类: Person"

// ===== 装饰器工厂（带参数）=====
function Logger2(prefix: string) {
    return function(constructor: Function) {
        console.log(`${prefix} 创建类: ${constructor.name}`);
    };
}

@Logger2("[APP]")
class App {}
// 输出: "[APP] 创建类: App"
```

### 2.3 方法装饰器

```typescript
// ===== 方法装饰器 =====
function Readonly(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    descriptor.writable = false;
}

class User {
    name: string = "张三";
    
    @Readonly
    getName(): string {
        return this.name;
    }
}

const user = new User();
// user.getName = () => "李四";  // ❌ Error: 方法不可写
```

### 2.4 属性装饰器

```typescript
// ===== 属性装饰器 =====
function Default(value: any) {
    return function(target: any, propertyKey: string) {
        target[propertyKey] = value;
    };
}

class Config {
    @Default("localhost")
    host: string;
    
    @Default(8080)
    port: number;
}

const config = new Config();
console.log(config.host);  // "localhost"
console.log(config.port);  // 8080
```

### 2.5 实际应用示例

```typescript
// ===== 日志装饰器 =====
function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const original = descriptor.value;
    
    descriptor.value = function(...args: any[]) {
        console.log(`调用 ${propertyKey}，参数:`, args);
        const result = original.apply(this, args);
        console.log(`${propertyKey} 返回:`, result);
        return result;
    };
}

class Calculator {
    @Log
    add(a: number, b: number): number {
        return a + b;
    }
}

const calc = new Calculator();
calc.add(1, 2);
// 输出:
// 调用 add，参数: [1, 2]
// add 返回: 3
```

---

## 3. Vue 3 + TypeScript 实战

### 3.1 定义响应式类

```typescript
// ===== Vue 3 中的类使用 =====
import { ref, reactive } from 'vue'

class User {
    id: number
    name: string
    email: string
    
    constructor(id: number, name: string, email: string) {
        this.id = id
        this.name = name
        this.email = email
    }
}

const user = ref(new User(1, '张三', 'zhangsan@example.com'))
console.log(user.value.name)  // "张三"

// 修改
user.value.name = "李四"
```

### 3.2 组合式函数返回类实例

```typescript
// ===== 创建可复用的类工厂 =====
function useLocalStorage<T>(key: string, defaultValue: T) {
    const data = ref<T>(defaultValue)
    
    // 从 localStorage 读取
    const stored = localStorage.getItem(key)
    if (stored) {
        data.value = JSON.parse(stored)
    }
    
    // 监听变化并保存
    watch(data, (newValue) => {
        localStorage.setItem(key, JSON.stringify(newValue))
    }, { deep: true })
    
    return data
}

// 使用
const settings = useLocalStorage('app-settings', { theme: 'dark' })
```

---

## 📝 课后练习

创建练习文件 `src/day4-exercise.ts`：

```typescript
// ========== 练习 1：类 ==========
// 1. 定义一个 Animal 类，有 name 属性和 speak 方法
// 2. 定义一个 Dog 类继承 Animal，重写 speak 方法
// 3. 给 Dog 添加 private 类型的 age 属性和 public 的 breed 属性

// ========== 练习 2：访问修饰符 ==========
// 1. 定义一个 BankAccount 类
// 2. balance 是 private，只通过 deposit/withdraw 方法修改
// 3. 添加 readonly 的 accountNumber

// ========== 练习 3：装饰器 ==========
// 1. 写一个 @Logged 装饰器，记录函数调用
// 2. 写一个 @Deprecated 装饰器，标记方法为废弃

// ========== 练习 4：Vue 中使用类 ==========
// 1. 定义一个 Todo 类
// 2. 在 Vue 组件中使用 ref<Todo[]> 存储待办事项
```

---

## 🎯 今日自检清单

- [ ] 类的定义和构造函数
- [ ] 访问修饰符 public / private / protected
- [ ] 继承和抽象类
- [ ] Getter / Setter
- [ ] 装饰器基础

---

## 第一周总结

这周我们学习了：
1. ✅ 基础类型（string, number, boolean, array, tuple, enum...）
2. ✅ any / unknown / never 的区别
3. ✅ 接口（Interface）与类型别名（Type Alias）
4. ✅ 函数类型与泛型
5. ✅ 类与装饰器

**下一步**：第二周我们将学习 TypeScript 高级特性！

---

## 下周预告

- Day 5: 高级类型（联合、交叉、条件类型）
- Day 6: 类型推断与守卫
- Day 7: 模块与命名空间
- Day 8: 工具类型（Partial, Pick, Omit...）
- Day 9: 实战项目 - 为项目添加 TypeScript
