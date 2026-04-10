> 学习日期：2026-04-09（周三）

## 学习目标

1. 理解 ES Modules
2. 掌握模块导出导入
3. 了解命名空间（Namespace）

---

## 1. ES Modules（ES 模块）

### 1.1 导出（Export）

```typescript
// ===== 命名导出 =====

// 导出变量
export const PI = 3.14159;
export const VERSION = "1.0.0";

// 导出函数
export function add(a: number, b: number): number {
    return a + b;
}

// 导出类
export class User {
    constructor(public name: string) {}
}

// 导出接口（接口本身就是类型，不需要具体实现）
export interface Person {
    name: string;
    age: number;
}

// 批量导出
function subtract(a: number, b: number): number {
    return a - b;
}

class Animal {
    constructor(public species: string) {}
}

export { subtract, Animal };

// 重命名导出
export { subtract as minus, Animal as Pet };
```

### 1.2 导入（Import）

```typescript
// ===== 命名导入 =====

// 导入具体内容
import { PI, add } from "./math";
console.log(add(PI, 2));

// 导入并重命名
import { add as sum, PI as pi } from "./math";

// 导入所有内容为命名空间
import * as MathUtils from "./math";
console.log(MathUtils.PI);

// ===== 默认导出 =====

// 定义默认导出
export default function multiply(a: number, b: number): number {
    return a * b;
}

// 导入默认导出
import multiply from "./math";

// 可以同时导入默认和命名导出
import multiply, { PI } from "./math";
```

### 1.3 类型导出导入

```typescript
// ===== 类型导出导入 =====

// 导出类型
export type ID = string | number;
export interface User {
    id: ID;
    name: string;
}

// 导入类型（编译时会被删除）
import type { ID, User } from "./types";

// 两者混合
import { type ID, User } from "./types";
```

---

## 2. 模块解析

### 2.1 相对 vs 非相对导入

```typescript
// ===== 相对导入 =====
// 以 ./ 或 ../ 开头

import { User } from "./models/User";
import { utils } from "../utils/helper";

// ===== 非相对导入 =====
// 不以 ./ 或 ../ 开头，从 tsconfig 的 paths 或 node_modules 查找

import express from "express";
import { useState } from "vue";
```

### 2.2 路径别名

在 `tsconfig.json` 中配置：

```json
{
    "compilerOptions": {
        "baseUrl": "./src",
        "paths": {
            "@/*": ["./*"],
            "@components/*": ["./components/*"],
            "@utils/*": ["./utils/*"]
        }
    }
}
```

使用：

```typescript
import Button from "@components/Button";
import { formatDate } from "@utils/date";
```

---

## 3. 命名空间（Namespace）

命名空间用于组织代码，避免全局命名冲突。

### 3.1 定义命名空间

```typescript
// ===== 定义命名空间 =====
namespace Validation {
    export interface Rules {
        required: boolean;
        minLength?: number;
        maxLength?: number;
    }
    
    export class Validator {
        validate(value: string, rules: Rules): boolean {
            if (rules.required && !value) {
                return false;
            }
            if (rules.minLength && value.length < rules.minLength) {
                return false;
            }
            if (rules.maxLength && value.length > rules.maxLength) {
                return false;
            }
            return true;
        }
    }
}

// 使用
const validator = new Validation.Validator();
validator.validate("hello", { required: true, minLength: 3 });
```

### 3.2 多文件命名空间

```typescript
// ===== 文件1: Validation.ts =====
namespace Validation {
    export interface Rules {
        required: boolean;
    }
}

// ===== 文件2: Validator.ts =====
/// <reference path="./Validation.ts" />

namespace Validation {
    export class Validator {
        validate(value: string, rules: Rules): boolean {
            return rules.required ? value.length > 0 : true;
        }
    }
}
```

### 3.3 模块 vs 命名空间

| 特性 | ES Modules | 命名空间 |
|------|-----------|----------|
| 编译目标 | 可编译为多种格式 | 通常编译为 IIFE（浏览器） |
| 导入方式 | import / export | 全局对象 |
| 树摇优化 | 支持 | 不支持 |
| 推荐场景 | 现代项目 | 简单浏览器脚本 |

**现代开发推荐使用 ES Modules**。

---

## 4. 声明文件（.d.ts）

声明文件用于为 JavaScript 库提供类型信息。

### 4.1 全局声明

```typescript
// ===== 全局声明 =====

// 声明全局变量
declare const API_BASE: string;

// 声明全局函数
declare function fetchData<T>(url: string): Promise<T>;

// 声明全局接口
interface Window {
    gtag: (command: string, ...args: any[]) => void;
}
```

### 4.2 模块声明

```typescript
// ===== 为 JS 模块声明类型 =====

// my-lib.d.ts
declare module "my-lib" {
    export function init(options: { debug: boolean }): void;
    export class HttpClient {
        get<T>(url: string): Promise<T>;
        post<T>(url: string, data: any): Promise<T>;
    }
}

// 使用
import { init, HttpClient } from "my-lib";
```

### 4.3 扩展现有模块

```typescript
// ===== 扩展 Vue 类型 =====

// vue-shim.d.ts
import "vue";

declare module "vue" {
    export interface ComponentCustomProperties {
        $api: {
            get<T>(url: string): Promise<T>;
            post<T>(url: string, data: any): Promise<T>;
        };
    }
}
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：模块导出导入 ==========
// 1. 创建 math.ts，导出加减乘除函数
// 2. 创建 index.ts，导入并使用这些函数

// ========== 练习 2：类型导入 ==========
// 1. 创建 types.ts，定义 User 接口
// 2. 使用 type 关键字导入

// ========== 练习 3：路径别名 ==========
// 1. 在项目中配置路径别名
// 2. 使用别名导入模块

// ========== 练习 4：声明文件 ==========
// 1. 创建一个简单的声明文件
// 2. 为一个假设的库声明类型
```

---

## 🎯 今日自检清单

- [ ] ES Modules 导出（export）
- [ ] ES Modules 导入（import）
- [ ] 类型导入（import type）
- [ ] 路径别名配置
- [ ] 命名空间基础
- [ ] 声明文件（.d.ts）

---

## 下一步

明天我们将学习：**工具类型（Utility Types）**
