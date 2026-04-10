> 学习日期：2026-04-18（周五）

## 学习目标

1. 掌握 Pinia 安装与配置
2. 理解 Store 的 TypeScript 类型定义
3. 能够创建类型安全的 Pinia Store

---

## 1. Pinia 简介与安装

### 1.1 安装

```bash
npm install pinia
```

### 1.2 main.ts 配置

```typescript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)
const pinia = createPinia()

app.use(pinia)
app.mount('#app')
```

---

## 2. Pinia Store 基础

### 2.1 定义 Store

```typescript
// src/stores/counter.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useCounterStore = defineStore('counter', () => {
    // 状态
    const count = ref(0)
    const name = ref('Counter')

    // 计算属性
    const doubleCount = computed(() => count.value * 2)
    const isPositive = computed(() => count.value > 0)

    // 方法
    function increment() {
        count.value++
    }

    function decrement() {
        count.value--
    }

    function reset() {
        count.value = 0
    }

    return {
        count,
        name,
        doubleCount,
        isPositive,
        increment,
        decrement,
        reset
    }
})
```

### 2.2 Option Store 风格

```typescript
// src/stores/user.ts
import { defineStore } from 'pinia'

interface UserState {
    id: number | null
    name: string
    email: string
    avatar: string
    isLoggedIn: boolean
}

export const useUserStore = defineStore('user', {
    // 状态
    state: (): UserState => ({
        id: null,
        name: '',
        email: '',
        avatar: '',
        isLoggedIn: false
    }),

    // 计算属性
    getters: {
        displayName: (state) => state.name || '未登录',
        isAdmin: (state) => state.email.endsWith('@admin.com')
    },

    // 方法
    actions: {
        async login(email: string, password: string) {
            // 模拟登录
            const user = await fetch('/api/login', {
                method: 'POST',
                body: JSON.stringify({ email, password })
            }).then(r => r.json())

            this.id = user.id
            this.name = user.name
            this.email = user.email
            this.avatar = user.avatar
            this.isLoggedIn = true
        },

        logout() {
            this.$reset()
        }
    },

    // 持久化
    persist: true
})
```

---

## 3. TypeScript Store 类型

### 3.1 定义 State 类型

```typescript
// src/stores/types.ts
export interface User {
    id: number
    name: string
    email: string
    avatar?: string
    role: 'admin' | 'user' | 'guest'
}

export interface Todo {
    id: number
    title: string
    done: boolean
    createdAt: string
}
```

### 3.2 完整 Store 示例

```typescript
// src/stores/todo.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import type { Todo } from './types'

export const useTodoStore = defineStore('todo', () => {
    // ===== State =====
    const todos = ref<Todo[]>([])
    const filter = ref<'all' | 'active' | 'completed'>('all')
    const loading = ref(false)

    // ===== Getters =====
    const activeTodos = computed(() => 
        todos.value.filter(t => !t.done)
    )

    const completedTodos = computed(() => 
        todos.value.filter(t => t.done)
    )

    const filteredTodos = computed(() => {
        switch (filter.value) {
            case 'active':
                return activeTodos.value
            case 'completed':
                return completedTodos.value
            default:
                return todos.value
        }
    })

    const totalCount = computed(() => todos.value.length)
    const activeCount = computed(() => activeTodos.value.length)
    const completedCount = computed(() => completedTodos.value.length)

    // ===== Actions =====
    async function fetchTodos() {
        loading.value = true
        try {
            const response = await fetch('/api/todos')
            todos.value = await response.json()
        } finally {
            loading.value = false
        }
    }

    function addTodo(title: string): void {
        const newTodo: Todo = {
            id: Date.now(),
            title,
            done: false,
            createdAt: new Date().toISOString()
        }
        todos.value.push(newTodo)
    }

    function toggleTodo(id: number): void {
        const todo = todos.value.find(t => t.id === id)
        if (todo) {
            todo.done = !todo.done
        }
    }

    function deleteTodo(id: number): void {
        const index = todos.value.findIndex(t => t.id === id)
        if (index !== -1) {
            todos.value.splice(index, 1)
        }
    }

    function clearCompleted(): void {
        todos.value = todos.value.filter(t => !t.done)
    }

    function setFilter(newFilter: 'all' | 'active' | 'completed'): void {
        filter.value = newFilter
    }

    return {
        // State
        todos,
        filter,
        loading,
        // Getters
        activeTodos,
        completedTodos,
        filteredTodos,
        totalCount,
        activeCount,
        completedCount,
        // Actions
        fetchTodos,
        addTodo,
        toggleTodo,
        deleteTodo,
        clearCompleted,
        setFilter
    }
})
```

---

## 4. Store 组合式写法

### 4.1 基础写法

```typescript
// src/stores/auth.ts
import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export const useAuthStore = defineStore('auth', () => {
    // 状态
    const token = ref<string | null>(localStorage.getItem('token'))
    const user = ref<{ id: number; name: string; email: string } | null>(null)

    // 计算属性
    const isLoggedIn = computed(() => !!token.value)
    const userName = computed(() => user.value?.name ?? '')

    // 方法
    async function login(email: string, password: string) {
        const response = await fetch('/api/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password })
        })
        
        const data = await response.json()
        
        token.value = data.token
        user.value = data.user
        
        localStorage.setItem('token', data.token)
    }

    function logout() {
        token.value = null
        user.value = null
        localStorage.removeItem('token')
    }

    return {
        token,
        user,
        isLoggedIn,
        userName,
        login,
        logout
    }
})
```

### 4.2 带泛型的 Store

```typescript
// src/stores/createGenericStore.ts
import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export function createGenericStore<T>(name: string, defaultValue: T) {
    return defineStore(name, () => {
        const data = ref<T>(defaultValue)
        
        const hasData = computed(() => {
            if (typeof data.value === 'object') {
                return data.value !== null && Object.keys(data.value).length > 0
            }
            return data.value !== null && data.value !== undefined
        })

        function setData(newData: T) {
            data.value = newData
        }

        function reset() {
            data.value = defaultValue
        }

        return {
            data,
            hasData,
            setData,
            reset
        }
    })
}

// 使用
export const useConfigStore = createGenericStore('config', {
    theme: 'light',
    language: 'zh-CN'
})
```

---

## 5. Pinia 插件

### 5.1 持久化插件

```typescript
// 安装
npm install pinia-plugin-persistedstate
```

```typescript
// main.ts
import { createPinia } from 'pinia'
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)

// store 使用
export const useUserStore = defineStore('user', () => {
    // ...
}, {
    persist: true  // 开启持久化
})
```

### 5.2 自定义插件

```typescript
// src/plugins/pinia/logger.ts
import type { Pinia } from 'pinia'

export function loggerPlugin(pinia: Pinia) {
    pinia.use(({ store }) => {
        // 监听 store 变化
        store.$subscribe((mutation, state) => {
            console.log(`[${store.$id}] 变化:`, mutation)
            console.log('新状态:', state)
        })
    })
}
```

---

## 6. 组件中使用 Store

### 6.1 基础使用

```vue
<script setup lang="ts">
import { storeToRefs } from 'pinia'
import { useTodoStore } from '@/stores/todo'

// 获取 store
const todoStore = useTodoStore()

// 使用 storeToRefs 保持响应式（解构）
const { todos, filter, loading } = storeToRefs(todoStore)

// 方法直接解构
const { addTodo, toggleTodo, deleteTodo } = todoStore

function handleAdd() {
    addTodo('新任务')
}
</script>

<template>
    <div>
        <div v-if="loading">加载中...</div>
        <ul v-else>
            <li 
                v-for="todo in todos" 
                :key="todo.id"
                @click="toggleTodo(todo.id)"
            >
                {{ todo.title }} - {{ todo.done ? '完成' : '待办' }}
            </li>
        </ul>
    </div>
</template>
```

### 6.2 组合式写法 Store

```vue
<script setup lang="ts">
import { useAuthStore } from '@/stores/auth'

// 直接解构（组合式写法支持）
const { isLoggedIn, user, login, logout } = useAuthStore()
</script>
```

---

## 7. Store 间交互

### 7.1 在 Store 中使用其他 Store

```typescript
// src/stores/cart.ts
import { defineStore } from 'pinia'
import { useUserStore } from './user'
import { useProductStore } from './product'

export const useCartStore = defineStore('cart', () => {
    const items = ref<{ productId: number; quantity: number }[]>([])

    function addToCart(productId: number) {
        // 使用其他 store
        const userStore = useUserStore()
        
        if (!userStore.isLoggedIn) {
            throw new Error('请先登录')
        }

        const existing = items.value.find(i => i.productId === productId)
        if (existing) {
            existing.quantity++
        } else {
            items.value.push({ productId, quantity: 1 })
        }
    }

    async function checkout() {
        const userStore = useUserStore()
        const productStore = useProductStore()
        
        // 计算总价
        const total = items.value.reduce((sum, item) => {
            const product = productStore.getProduct(item.productId)
            return sum + (product?.price ?? 0) * item.quantity
        }, 0)

        // 提交订单...
    }

    return {
        items,
        addToCart,
        checkout
    }
})
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：基础 Store ==========
// 1. 创建一个用户 Store
// 2. 实现登录、登出功能

// ========== 练习 2：TypeScript 类型 ==========
// 1. 定义 State、Getter、Action 类型
// 2. 使用泛型创建可复用的 Store

// ========== 练习 3：持久化 ==========
// 1. 安装并配置 pinia-plugin-persistedstate
// 2. 实现用户信息持久化

// ========== 练习 4：Store 交互 ==========
// 1. 创建购物车 Store
// 2. 在购物车中使用用户 Store
```

---

## 🎯 今日自检清单

- [ ] Pinia 安装配置
- [ ] defineStore 两种写法
- [ ] TypeScript Store 类型定义
- [ ] Store 持久化
- [ ] Store 间交互

---

## 第一周 Vue 总结

这周我们学习了：
1. ✅ Vue 3 + TypeScript 项目搭建
2. ✅ Composition API 响应式系统
3. ✅ 组件 Props/Emit 类型定义
4. ✅ Vue Router 4 + TypeScript
5. ✅ Pinia 状态管理 + TypeScript

---

## 下周预告

- Day 6: Element Plus + TypeScript
- Day 7: Vitest 单元测试
- Day 8: 实战项目 - Todo List
