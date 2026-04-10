> 学习日期：2026-04-14（周一）

## 学习目标

1. 掌握 Vite + Vue 3 + TypeScript 项目创建
2. 理解项目目录结构
3. 掌握 TypeScript 配置

---

## 1. 创建 Vue 3 + TypeScript 项目

### 1.1 使用 Vite 创建项目

```bash
# 创建项目（选择 Vue + TypeScript）
npm create vite@latest my-vue-app -- --template vue-ts

# 进入项目目录
cd my-vue-app

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

### 1.2 项目结构

```
my-vue-app/
├── public/                 # 静态资源（不被构建）
│   └── favicon.ico
├── src/                    # 源代码
│   ├── assets/            # 资源（图片、样式等）
│   │   └── main.css
│   ├── components/        # 组件
│   │   └── HelloWorld.vue
│   ├── App.vue            # 根组件
│   ├── main.ts            # 入口文件
│   └── vite-env.d.ts      # Vite 类型声明
├── index.html             # HTML 模板
├── package.json           # 依赖配置
├── tsconfig.json          # TypeScript 配置
├── tsconfig.node.json     # Node 环境 TypeScript 配置
└── vite.config.ts         # Vite 配置
```

---

## 2. TypeScript 配置详解

### 2.1 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",              // 编译目标版本
    "useDefineForClassFields": true, // 类字段使用设计模式
    "module": "ESNext",              // 模块系统
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,            // 跳过库检查

    /* 模块解析 */
    "moduleResolution": "bundler",   // 模块解析方式
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,       // 允许导入 JSON
    "isolatedModules": true,         // 每个文件独立转译

    /* 路径别名 */
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },

    /* 严格模式 */
    "strict": true,                  // 开启所有严格检查
    "noUnusedLocals": true,          // 未使用变量报错
    "noUnusedParameters": true,      // 未使用参数报错
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### 2.2 路径别名配置

**1. tsconfig.json 配置：**

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"]
    }
  }
}
```

**2. vite.config.ts 配置：**

```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components')
    }
  }
})
```

**使用：**

```typescript
import HelloWorld from '@/components/HelloWorld.vue'
import { useApi } from '@/composables/useApi'
```

---

## 3. Vue 3 入口文件类型

### 3.1 main.ts

```typescript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

// 创建应用实例
const app = createApp(App)

// 全局错误处理
app.config.errorHandler = (err, instance, info) => {
  console.error('全局错误:', err)
  console.error('组件:', instance)
  console.error('信息:', info)
}

// 挂载应用
app.mount('#app')
```

### 3.2 类型声明文件

**vite-env.d.ts：**

```typescript
/// <reference types="vite/client" />

// 导入 .vue 文件需要声明
declare module '*.vue' {
  import type { DefineComponent } from 'vue'
  const component: DefineComponent<{}, {}, any>
  export default component
}
```

---

## 4. Vue 文件中的 TypeScript

### 4.1 script setup 语法

```vue
<template>
  <div class="greeting">
    <h1>{{ message }}</h1>
    <button @click="handleClick">点击</button>
  </div>
</template>

<script setup lang="ts">
// 定义响应式数据
const message: string = '你好，Vue 3 + TypeScript!'

// 定义方法
function handleClick(): void {
  console.log('按钮被点击')
}
</script>

<style scoped>
.greeting {
  text-align: center;
  margin-top: 50px;
}
</style>
```

### 4.2 组件 Props 定义

```vue
<script setup lang="ts">
// 方式1：使用 defineProps（推荐）
interface Props {
  title: string
  count?: number
  items: string[]
}

const props = withDefaults(defineProps<Props>(), {
  count: 0
})

// 方式2：使用泛型
// const props = defineProps<Props>()
</script>
```

---

## 5. 开发工具配置

### 5.1 VS Code 插件推荐

| 插件 | 作用 |
|------|------|
| Vue - Official | Vue 3 官方支持 |
| TypeScript Vue Plugin | TypeScript 支持 |
| ESLint | 代码检查 |
| Prettier | 代码格式化 |
| Volar | Vue SFC 支持 |

### 5.2 设置.json 推荐配置

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[vue]": {
    "editor.defaultFormatter": "Vue.volar"
  },
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

---

## 📝 课后练习

```bash
# 1. 使用 Vite 创建 Vue 3 + TypeScript 项目
npm create vite@latest vue-project -- --template vue-ts

# 2. 配置路径别名
# 在 tsconfig.json 和 vite.config.ts 中配置 @ 别名

# 3. 创建一个组件，定义 props 和方法
```

---

## 🎯 今日自检清单

- [ ] 创建 Vue 3 + TypeScript 项目
- [ ] 理解项目目录结构
- [ ] 配置路径别名
- [ ] 在组件中使用 TypeScript

---

## 下一步

明天我们将学习：**Vue 3 Composition API 基础**
