> 学习日期：2026-03-31（周二）

## 学习目标

1. 掌握 8 种基础类型
2. 理解 `any` vs `unknown` vs `never` 的区别
3. 能够正确声明变量类型

---

## 1. 基础类型一览

```typescript
// ===== 基础类型 =====

// 字符串
let name: string = "张三";
let greeting: string = `你好，${name}！`;  // 模板字符串

// 数字（整数和浮点数都是 number）
let age: number = 25;
let price: number = 19.99;
let negative: number = -10;

// 布尔值
let isStudent: boolean = true;
let hasJob: boolean = false;

// 数组
let numbers: number[] = [1, 2, 3, 4, 5];
let strings: Array<string> = ["a", "b", "c"];  // 泛型写法
let mixed: (number | string)[] = [1, "two", 3];  // 联合类型

// 元组（Tuple）：固定长度、固定类型的数组
let person: [string, number, boolean] = ["李四", 30, true];
let coord: [number, number] = [100, 200];

// 枚举（Enum）
enum Color {
    Red = "red",
    Green = "green", 
    Blue = "blue"
}
let favoriteColor: Color = Color.Blue;

// null 和 undefined
let empty: null = null;
let notDefined: undefined = undefined;

// void：函数没有返回值
function sayHello(): void {
    console.log("Hello!");
}

// symbol
// symbol是一种用于创建唯一且不可变的值的原始数据类型，主要用于对象的属性键，以保证属性名的唯一性，避免冲突。
// Symbol值创建后无法改变，适合用作枚举或常量的值。
let sym: symbol = Symbol("唯一标识");

// 1. 基本类型注解
let sym: symbol = Symbol();
const uniqueKey: symbol = Symbol('description');

// 2. 作为对象属性键（这是最主要的用途）
const ID_KEY = Symbol('id'); // 创建一个唯一的符号

let obj = {
    [ID_KEY]: 123, // 使用符号作为属性名
    name: "元宝"
};

console.log(obj[ID_KEY]); // 123
// 这个属性不会被普通的遍历（如 for...in, Object.keys）发现，也不会被意外覆盖。
```

---

## 2. 重点：`any` vs `unknown` vs `never`

这是面试必考！

```typescript
// ===== any：任意类型，绕过类型检查 =====
let anything: any = "hello";
anything = 123;
anything = true;
anything.foo.bar;  // ❌ 不报错（危险！）

// ===== unknown：安全版 any，使用前必须检查 =====
let maybe: unknown = "world";
maybe = 42;

// maybe.length;  ❌ 报错：Object is of type 'unknown'

// ✅ 必须先类型守卫
if (typeof maybe === "string") {
    console.log(maybe.length);  // OK
}

// ===== never：永不返回的值 ========

// 1. 函数抛出异常，永远不返回
function throwError(msg: string): never {
    throw new Error(msg);
}

// 2. 无限循环，永远不返回
function infiniteLoop(): never {
    while (true) {
        // do something forever
    }
}

// 3. 类型收窄的终极状态
function getValue(value: string | number) {
    if (typeof value === "string") {
        return value.toUpperCase();
    } else if (typeof value === "number") {
        return value.toFixed(2);
    } else {
        // never：这里永远不应该到达
        const _exhaustive: never = value;
        return _exhaustive;
    }
}
```

---

## 3. 类型断言（手动告诉 TS 是什么类型）

```typescript
// ===== 类型断言：告诉 TS "我保证这是 XXX" =====

let someValue: unknown = "Hello TypeScript";

// 写法一：as 语法（推荐）
let strLength1: number = (someValue as string).length;

// 写法二：<> 语法
let strLength2: number = (<string>someValue).length;

// ⚠️ 注意：断言是"骗"编译器，不是真的转换
let num: unknown = "42";
let actualNum: number = +(num as string);  // 运行时还是字符串！
```

---

## 4. 类型推断

```typescript
// ===== TypeScript 会自动推断类型 =====

let x = 3;        // ✅ 推断为 number
let y = "hello";  // ✅ 推断为 string
let z = true;     // ✅ 推断为 boolean

// 数组
let arr = [1, 2, 3];  // ✅ 推断为 number[]

// 函数返回值
function add(a: number, b: number) {
    return a + b;  // ✅ 推断返回 number
}
```

---

## 📝 课后练习

创建练习文件 `src/day1-exercise.ts`：

```typescript
// 练习1：声明以下变量并添加类型
let myName: string = "你的名字";
let myAge: number = 25;
let isLearning: boolean = true;
let skills: string[] = ["TypeScript", "JavaScript", "Vue"];

// 练习2：创建元组表示一本书（标题、作者、页数、是否读完）
let book: [string, string, number, boolean] = ["百年孤独", "马尔克斯", 360, false];

// 练习3：写出 any、unknown、never 的区别（写注释）
// any: 任意类型，绕过类型检查
// unknown: 未知类型，使用前必须检查
// never: 永不返回的值

// 练习4：写一个返回 never 的函数
function infinite(): never {
    while (true) {}
}

// 练习5：使用类型断言
let value: unknown = "test";
console.log((value as string).toUpperCase());
```

---

## 编译运行
如果只在**项目本地**安装了编译器 TypeScript 和 ts-node，而没有安装到**全局**。在 Windows 系统中，局部安装的包不会自动添加到系统 PATH 环境变量中，所以无法直接在命令行中使用。

```powershell
# 局部安装命令
npm install --save-dev typescript
npm install --save-dev ts-node typescript
```

### 解决方案

### 方案1：使用 npx 运行（推荐）

`npx`是 npm 自带的工具，可以执行项目本地安装的包。

```
# 查看版本
npx tsc --version
npx ts-node --version

# 编译 TypeScript
# 当指定 `tsconfig.json`时，TypeScript 会编译整个项目
# 当在命令行指定具体文件时，TypeScript 会忽略项目配置
# 两者同时存在会产生歧义，所以 TypeScript 默认会报错
npx tsc yourfile.ts

# 日常开发运行某一个文件，用 `ts-node`
# 直接运行 TypeScript
npx ts-node yourfile.ts
```

### 方案2：通过 package.json 的 scripts 运行

修改 `package.json`，添加 scripts 脚本：

```
{
  "scripts": {
    "tsc": "tsc",
    "ts-node": "ts-node",
    "build": "tsc",
    "start": "ts-node src/index.ts"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "ts-node": "^10.9.0"
  }
}
```

然后通过 npm 运行：

```
# 查看版本
npm run tsc -- --version
# 或
npm exec tsc -- --version

# 编译 TypeScript
npm run build
# 或
npm exec tsc yourfile.ts

# 运行 TypeScript
npm exec ts-node yourfile.ts
```

### 方案3：全局安装（简单但可能不推荐）

全局安装后，`tsc`和 `ts-node`命令会在任何目录下都可用。

```
# 全局安装
npm install -g typescript ts-node

# 现在可以直接使用了
tsc --version
ts-node --version
```

**注意**：全局安装可能导致不同项目的 TypeScript 版本冲突。建议在项目中用 npx 或 scripts。

### 方案4：使用完整路径执行

直接指定 `node_modules/.bin`目录中的可执行文件：

```
# Windows
.\node_modules\.bin\tsc --version
.\node_modules\.bin\ts-node yourfile.ts

# macOS/Linux
./node_modules/.bin/tsc --version
./node_modules/.bin/ts-node yourfile.ts
```

### 方案5：配置 VS Code 任务

如果你使用 VS Code，可以创建任务来运行 TypeScript：

1. 创建 `.vscode/tasks.json`：
    

```
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "TypeScript: 编译当前文件",
      "type": "shell",
      "command": "npx tsc ${file}",
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "TypeScript: 运行当前文件",
      "type": "shell",
      "command": "npx ts-node ${file}",
      "group": "test"
    }
  ]
}
```

1. 在 VS Code 中按 `Ctrl+Shift+P`，输入 "Run Task"，选择相应任务。
    

### 验证安装是否成功

运行以下命令检查是否安装正确：

```
# 检查 package.json 中是否有依赖
cat package.json | grep -A5 "devDependencies"

# 检查 node_modules/.bin 中是否有可执行文件
dir node_modules\.bin  # Windows
ls node_modules/.bin   # macOS/Linux
```

应该能看到 `tsc`和 `ts-node`相关的文件。

### 创建快捷脚本

还可以在项目根目录创建一个批处理文件来简化命令：

**windows-run-ts.bat**​ (Windows):

```
@echo off
npx ts-node %*
```

**run-ts.sh**​ (macOS/Linux):

```
#!/bin/bash
npx ts-node "$@"
```

给权限后使用：

```
# Windows
.\windows-run-ts.bat yourfile.ts

# macOS/Linux
chmod +x run-ts.sh
./run-ts.sh yourfile.ts
```

### 为什么局部安装的包不能直接运行？

这是 npm 的设计行为：

- 局部安装的包保存在 `node_modules/.bin/`目录
    
- Windows 不会自动将该目录加入 PATH
    
- 全局安装的包会放在 npm 的全局目录，这个目录通常在 PATH 中
    
- `npx`会自动查找局部安装的包，优先于全局
    

### 最佳实践建议

1. **开发环境**：使用 `npx`运行局部安装的工具
    
2. **项目脚本**：在 `package.json`的 `scripts`中定义常用命令
    
3. **CI/CD 环境**：确保在脚本中使用 `npx`或 `npm exec`
    
4. **团队协作**：避免全局依赖，让每个成员都通过项目内的版本工作
    

```
# 推荐的工作流程示例
npx tsc --init                    # 初始化 tsconfig.json
npx ts-node src/index.ts         # 运行开发
npx tsc                          # 编译项目
npm run build                    # 通过脚本编译
```

这样项目就可以独立于开发环境的全局配置，确保一致性。

---
## 🎯 今日自检清单

完成了吗？勾选一下：

- [x] 8 种基础类型（string, number, boolean, array, tuple, enum, null/undefined, void）
- [x] `any` vs `unknown` vs `never` 区别
- [x] 写了练习代码
- [x] 在 VS Code 中运行 `tsc` 编译成功

---

## 下一步

明天我们将学习：**接口（Interface）与类型别名**