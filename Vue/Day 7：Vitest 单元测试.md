> 学习日期：2026-04-22（周二）

## 学习目标

1. 掌握 Vitest 安装与配置
2. 理解测试基础概念
3. 能够编写 Vue 组件测试

---

## 1. Vitest 简介

Vitest 是一个由 Vite 驱动的单元测试框架，与 Vue 3 完美集成。

### 1.1 安装

```bash
npm install -D vitest @vue/test-utils jsdom
```

### 1.2 vite.config.ts 配置

```typescript
/// <reference types="vitest" />
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  test: {
    environment: 'jsdom',  // 模拟浏览器环境
    globals: true,         // 全局 API (describe, it, expect)
    include: ['src/**/*.test.ts', 'src/**/*.spec.ts']
  }
})
```

### 1.3 package.json 配置

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run",  // 单次运行
    "test:coverage": "vitest run --coverage"
  }
}
```

---

## 2. 测试基础

### 2.1 基础结构

```typescript
// 导入测试API
import { describe, it, expect, beforeEach, afterEach } from 'vitest'

// 测试组
describe('计算器', () => {
    // 测试用例
    it('应该正确计算加法', () => {
        expect(1 + 1).toBe(2)
    })
    
    it('应该正确计算减法', () => {
        expect(5 - 3).toBe(2)
    })
})
```

### 2.2 常用匹配器

```typescript
// 基础匹配
expect(value).toBe(expected)          // 严格相等 ===
expect(value).toEqual(expected)        // 深度相等
expect(value).toBeNull()               // null
expect(value).toBeUndefined()          // undefined
expect(value).toBeTruthy()            // 真值
expect(value).toBeFalsy()              // 假值

// 数值匹配
expect(value).toBeGreaterThan(3)      // 大于
expect(value).toBeLessThan(3)         // 小于
expect(value).toBeCloseTo(3.14, 2)    // 近似相等

// 字符串匹配
expect(str).toContain('hello')         // 包含
expect(str).toMatch(/hello/)           // 正则匹配

// 数组匹配
expect(arr).toContain(item)            // 包含元素
expect(arr).toHaveLength(3)           // 长度
expect(arr).toEqual(expect.arrayContaining(['a', 'b']))

// 对象匹配
expect(obj).toHaveProperty('name')    // 属性存在
expect(obj).toMatchObject({ name: '张三' }) // 部分匹配
```

---

## 3. 函数测试

### 3.1 基础函数测试

```typescript
// src/utils/calculator.ts
export function add(a: number, b: number): number {
    return a + b
}

export function multiply(a: number, b: number): number {
    return a * b
}

export function divide(a: number, b: number): number {
    if (b === 0) {
        throw new Error('不能除以零')
    }
    return a / b
}
```

```typescript
// src/utils/calculator.test.ts
import { describe, it, expect } from 'vitest'
import { add, multiply, divide } from './calculator'

describe('计算器', () => {
    describe('add', () => {
        it('应该正确计算两个正数相加', () => {
            expect(add(1, 2)).toBe(3)
        })
        
        it('应该正确计算负数相加', () => {
            expect(add(-1, -2)).toBe(-3)
        })
        
        it('应该正确计算小数相加', () => {
            expect(add(0.1, 0.2)).toBeCloseTo(0.3)
        })
    })
    
    describe('multiply', () => {
        it('应该正确计算乘法', () => {
            expect(multiply(3, 4)).toBe(12)
        })
        
        it('乘以零应该返回零', () => {
            expect(multiply(100, 0)).toBe(0)
        })
    })
    
    describe('divide', () => {
        it('应该正确计算除法', () => {
            expect(divide(10, 2)).toBe(5)
        })
        
        it('除以零应该抛出错误', () => {
            expect(() => divide(10, 0)).toThrow('不能除以零')
        })
    })
})
```

### 3.2 异步函数测试

```typescript
// src/utils/api.ts
export async function fetchUser(id: number): Promise<{ id: number; name: string }> {
    const response = await fetch(`/api/users/${id}`)
    return response.json()
}

export async function fetchUsers(): Promise<{ id: number; name: string }[]> {
    const response = await fetch('/api/users')
    return response.json()
}
```

```typescript
// src/utils/api.test.ts
import { describe, it, expect, vi } from 'vitest'
import { fetchUser, fetchUsers } from './api'

// Mock fetch
global.fetch = vi.fn()

describe('API', () => {
    beforeEach(() => {
        vi.clearAllMocks()
    })
    
    describe('fetchUser', () => {
        it('应该返回用户数据', async () => {
            const mockUser = { id: 1, name: '张三' }
            ;(fetch as any).mockResolvedValueOnce({
                json: () => Promise.resolve(mockUser)
            })
            
            const user = await fetchUser(1)
            expect(user).toEqual(mockUser)
        })
    })
})
```

---

## 4. Vue 组件测试

### 4.1 基础组件测试

```vue
<!-- src/components/HelloWorld.vue -->
<script setup lang="ts">
defineProps<{
    msg: string
}>()

const count = ref(0)
</script>

<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <button @click="count++">点击次数: {{ count }}</button>
    </div>
</template>
```

```typescript
// src/components/HelloWorld.test.ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import { createTestingPinia } from '@pinia/testing'
import HelloWorld from './HelloWorld.vue'

describe('HelloWorld', () => {
    it('应该正确渲染消息', () => {
        const wrapper = mount(HelloWorld, {
            props: { msg: '你好' }
        })
        
        expect(wrapper.text()).toContain('你好')
    })
    
    it('点击按钮应该增加计数', async () => {
        const wrapper = mount(HelloWorld, {
            props: { msg: '测试' }
        })
        
        const button = wrapper.find('button')
        expect(button.text()).toContain('点击次数: 0')
        
        await button.trigger('click')
        expect(button.text()).toContain('点击次数: 1')
    })
})
```

### 4.2 组件Props测试

```vue
<!-- src/components/UserCard.vue -->
<script setup lang="ts">
interface Props {
    name: string
    email: string
    avatar?: string
    status?: 'online' | 'offline'
}

const props = withDefaults(defineProps<Props>(), {
    status: 'offline'
})
</script>

<template>
    <div class="user-card">
        <img v-if="avatar" :src="avatar" :alt="name" />
        <h3>{{ name }}</h3>
        <p>{{ email }}</p>
        <span :class="['status', status]">{{ status }}</span>
    </div>
</template>
```

```typescript
// src/components/UserCard.test.ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import UserCard from './UserCard.vue'

describe('UserCard', () => {
    it('应该渲染用户信息', () => {
        const wrapper = mount(UserCard, {
            props: {
                name: '张三',
                email: 'zhangsan@example.com'
            }
        })
        
        expect(wrapper.text()).toContain('张三')
        expect(wrapper.text()).toContain('zhangsan@example.com')
    })
    
    it('应该使用默认状态', () => {
        const wrapper = mount(UserCard, {
            props: {
                name: '张三',
                email: 'test@example.com'
            }
        })
        
        expect(wrapper.find('.status.offline').exists()).toBe(true)
    })
    
    it('应该显示自定义头像', () => {
        const wrapper = mount(UserCard, {
            props: {
                name: '张三',
                email: 'test@example.com',
                avatar: '/avatar.png'
            }
        })
        
        expect(wrapper.find('img').exists()).toBe(true)
        expect(wrapper.find('img').attributes('src')).toBe('/avatar.png')
    })
})
```

### 4.3 组件交互测试

```vue
<!-- src/components/Counter.vue -->
<script setup lang="ts">
const props = defineProps<{ initial: number }>()
const emit = defineEmits<{
    (e: 'update', value: number): void
}>()

const count = ref(props.initial)

function increment() {
    count.value++
    emit('update', count.value)
}

function decrement() {
    count.value--
    emit('update', count.value)
}
</script>

<template>
    <div class="counter">
        <button @click="decrement">-</button>
        <span>{{ count }}</span>
        <button @click="increment">+</button>
    </div>
</template>
```

```typescript
// src/components/Counter.test.ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import Counter from './Counter.vue'

describe('Counter', () => {
    it('应该使用初始值', () => {
        const wrapper = mount(Counter, {
            props: { initial: 10 }
        })
        
        expect(wrapper.text()).toContain('10')
    })
    
    it('点击加号应该增加计数', async () => {
        const wrapper = mount(Counter, {
            props: { initial: 0 }
        })
        
        await wrapper.find('button:last-child').trigger('click')
        expect(wrapper.text()).toContain('1')
    })
    
    it('点击减号应该减少计数', async () => {
        const wrapper = mount(Counter, {
            props: { initial: 5 }
        })
        
        await wrapper.find('button:first-child').trigger('click')
        expect(wrapper.text()).toContain('4')
    })
    
    it('应该触发 update 事件', async () => {
        const wrapper = mount(Counter, {
            props: { initial: 0 }
        })
        
        await wrapper.find('button:last-child').trigger('click')
        
        expect(wrapper.emitted('update')).toBeTruthy()
        expect(wrapper.emitted('update')?.[0]).toEqual([1])
    })
})
```

---

## 5. Pinia Store 测试

### 5.1 Store 测试

```typescript
// src/stores/counter.ts
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useCounterStore = defineStore('counter', () => {
    const count = ref(0)
    
    function increment() {
        count.value++
    }
    
    function decrement() {
        count.value--
    }
    
    return { count, increment, decrement }
})
```

```typescript
// src/stores/counter.test.ts
import { describe, it, expect, beforeEach } from 'vitest'
import { setActivePinia, createPinia } from 'pinia'
import { useCounterStore } from './counter'

describe('Counter Store', () => {
    beforeEach(() => {
        setActivePinia(createPinia())
    })
    
    it('初始值应该是0', () => {
        const store = useCounterStore()
        expect(store.count).toBe(0)
    })
    
    it('increment 应该增加计数', () => {
        const store = useCounterStore()
        store.increment()
        expect(store.count).toBe(1)
    })
    
    it('decrement 应该减少计数', () => {
        const store = useCounterStore()
        store.count.value = 10
        store.decrement()
        expect(store.count).toBe(9)
    })
})
```

---

## 6. Mock 技术

### 6.1 vi.fn() 函数Mock

```typescript
import { describe, it, expect, vi } from 'vitest'

describe('Mock 函数', () => {
    it('应该捕获调用信息', () => {
        const fn = vi.fn()
        
        fn('a', 'b')
        fn('c')
        
        expect(fn).toHaveBeenCalledTimes(2)
        expect(fn).toHaveBeenCalledWith('a', 'b')
        expect(fn).toHaveBeenCalledWith('c')
    })
    
    it('应该返回预设值', () => {
        const fn = vi.fn().mockReturnValue('mocked')
        
        expect(fn()).toBe('mocked')
    })
    
    it('应该实现异步函数', async () => {
        const fn = vi.fn().mockResolvedValue('async result')
        
        const result = await fn()
        expect(result).toBe('async result')
    })
})
```

### 6.2 vi.mock() 模块Mock

```typescript
// Mock 模块
vi.mock('@/utils/api', () => ({
    fetchUser: vi.fn().mockResolvedValue({ id: 1, name: 'Mock 用户' }),
    fetchUsers: vi.fn().mockResolvedValue([
        { id: 1, name: '用户1' },
        { id: 2, name: '用户2' }
    ])
}))

import { fetchUser, fetchUsers } from '@/utils/api'

describe('Mock 模块', () => {
    it('fetchUser 应该返回mock数据', async () => {
        const user = await fetchUser(1)
        expect(user.name).toBe('Mock 用户')
    })
})
```

---

## 7. 常用测试技巧

### 7.1 等待异步操作

```typescript
import { describe, it, expect, vi } from 'vitest'

// 方式1: await wrapper.vm.$nextTick()
await wrapper.vm.$nextTick()

// 方式2: await nextTick()
import { nextTick } from 'vue'
await nextTick()

// 方式3: 使用 fake timers（定时器）
vi.useFakeTimers()

// 方式4: 等待指定时间
await vi.waitFor(() => {
    expect(wrapper.text()).toContain('更新后的内容')
}, { timeout: 1000 })
```

### 7.2 错误边界测试

```typescript
it('应该捕获组件错误', async () => {
    const errorHandler = vi.fn()
    
    const wrapper = mount(Component, {
        global: {
            config: {
                errorHandler
            }
        }
    })
    
    // 触发错误...
    
    expect(errorHandler).toHaveBeenCalled()
})
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：工具函数测试 ==========
// 1. 编写一个日期格式化函数的测试
// 2. 编写一个表单验证函数的测试

// ========== 练习 2：组件测试 ==========
// 1. 测试一个 Button 组件
// 2. 测试一个 Input 组件的 v-model

// ========== 练习 3：Store 测试 ==========
// 1. 测试 Todo Store 的增删改查
// 2. 测试计算属性

// ========== 练习 4：Mock 测试 ==========
// 1. Mock fetch API
// 2. Mock localStorage
```

---

## 🎯 今日自检清单

- [ ] Vitest 安装配置
- [ ] 基础测试语法
- [ ] 函数测试
- [ ] Vue 组件测试
- [ ] Pinia Store 测试
- [ ] Mock 技术

---

## 下一步

明天我们将学习：**实战项目 - Todo List**
