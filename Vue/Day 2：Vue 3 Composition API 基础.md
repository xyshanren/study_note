> 学习日期：2026-04-15（周二）

## 学习目标

1. 掌握 ref 和 reactive 响应式系统
2. 理解 computed 和 watch
3. 能够使用组合式函数

---

## 1. 响应式系统

### 1.1 ref - 基础类型响应式

```typescript
import { ref } from 'vue'

// 定义响应式变量
const count = ref(0)
const name = ref('张三')
const isActive = ref(true)

// 读取和修改
console.log(count.value)  // 0
count.value++

// 在模板中自动解包，不需要 .value
// <template>{{ count }}</template>
```

**泛型指定类型：**

```typescript
const count = ref<number>(0)
const list = ref<string[]>([])
const user = ref<{ name: string; age: number }>()
```

### 1.2 reactive - 对象响应式

```typescript
import { reactive } from 'vue'

// 定义响应式对象
const state = reactive({
    user: {
        name: '张三',
        age: 25
    },
    loading: false
})

// 直接修改属性
state.user.name = '李四'
state.loading = true

// 不需要 .value（已经解包）
```

**reactive vs ref：**

| 特性 | ref | reactive |
|------|-----|----------|
| 支持类型 | 任意类型 | 对象/数组 |
| 修改方式 | `.value` | 直接修改 |
| 替换整个对象 | `obj.value = newObj` | `Object.assign(obj, newObj)` |
| 返回值 | Ref 对象 | 原始对象 |

**选择建议：**
- 基础类型（string, number, boolean）用 `ref`
- 对象/数组用 `reactive`
- 复杂状态可以用 `ref` 包裹对象

### 1.3 readonly - 只读响应式

```typescript
import { ref, readonly } from 'vue'

const original = ref({ count: 0 })
const copy = readonly(original)

// 修改 original 会触发警告
original.value.count++
// Vue警告: Set operation on key "count" failed: target is readonly.

// 读取 copy 正常
console.log(copy.count)  // 1
```

---

## 2. computed - 计算属性

### 2.1 基础用法

```typescript
import { ref, computed } from 'vue'

const firstName = ref('张')
const lastName = ref('三')

// 计算属性（自动缓存）
const fullName = computed(() => {
    return firstName.value + lastName.value
})

console.log(fullName.value)  // "张三"
```

### 2.2 Getter 和 Setter

```typescript
const count = ref(0)

// 可写的计算属性
const doubleCount = computed({
    get() {
        return count.value * 2
    },
    set(val: number) {
        count.value = val / 2
    }
})

doubleCount.value = 100  // count 变成 50
console.log(count.value)  // 50
```

### 2.3 复杂示例

```typescript
import { ref, computed } from 'vue'

interface Todo {
    id: number
    title: string
    done: boolean
}

const todos = ref<Todo[]>([
    { id: 1, title: '学习 Vue', done: true },
    { id: 2, title: '学习 TypeScript', done: false },
    { id: 3, title: '完成项目', done: false }
])

// 未完成数量
const remainingCount = computed(
    () => todos.value.filter(t => !t.done).length
)

// 完成进度
const progress = computed(() => {
    if (todos.value.length === 0) return 0
    const done = todos.value.filter(t => t.done).length
    return Math.round((done / todos.value.length) * 100)
})

// 是否全部完成
const allDone = computed({
    get: () => remainingCount.value === 0,
    set: (val: boolean) => {
        todos.value.forEach(t => t.done = val)
    }
})
```

---

## 3. watch - 监听器

### 3.1 基础用法

```typescript
import { ref, watch } from 'vue'

const count = ref(0)

// 监听单个 ref
watch(count, (newVal, oldVal) => {
    console.log(`count 从 ${oldVal} 变成 ${newVal}`)
})

count.value++  // 输出: count 从 0 变成 1
```

### 3.2 监听 reactive 对象

```typescript
import { reactive, watch } from 'vue'

const state = reactive({
    user: { name: '张三' },
    loading: false
})

// 监听整个 reactive 对象
watch(state, (newVal, oldVal) => {
    console.log('state 变化了', newVal)
})

// 监听单个属性
watch(
    () => state.user.name,
    (newVal, oldVal) => {
        console.log(`名字从 ${oldVal} 变成 ${newVal}`)
    }
)
```

### 3.3 监听多个源

```typescript
import { ref, watch } from 'vue'

const firstName = ref('张')
const lastName = ref('三')

// 监听多个源
watch([firstName, lastName], ([newFirst, newLast], [oldFirst, oldLast]) => {
    console.log(`${oldFirst}${oldLast} -> ${newFirst}${newLast}`)
})

firstName.value = '李'  // 输出: 张三 -> 李三
```

### 3.4 立即执行和深度监听

```typescript
import { ref, watch } from 'vue'

const state = ref({ user: { name: '张三' } })

// immediate: 立即执行一次
watch(state, () => {
    console.log('state 变化了')
}, { immediate: true })

// deep: 深度监听（reactive 默认深度监听）
watch(
    () => state.value.user,
    () => {
        console.log('user 对象变化了')
    },
    { deep: true }
)
```

---

## 4. watchEffect - 自动监听

### 4.1 基础用法

```typescript
import { ref, watchEffect } from 'vue'

const count = ref(0)
const message = ref('')

// watchEffect 自动收集依赖
watchEffect(() => {
    message.value = `当前计数: ${count.value}`
    console.log(`计数变为: ${count.value}`)
})

count.value++  // 自动触发
```

### 4.2 watch vs watchEffect

| 特性 | watch | watchEffect |
|------|-------|-------------|
| 依赖声明 | 手动指定 | 自动收集 |
| 首次执行 | 需要 immediate | 自动执行 |
| 旧值 | 能访问 | 不能访问 |
| 场景 | 精确控制 | 简单响应 |

---

## 5. 组合式函数（Composables）

### 5.1 基础组合式函数

```typescript
// src/composables/useCounter.ts
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
    const count = ref(initialValue)
    
    const double = computed(() => count.value * 2)
    
    function increment() {
        count.value++
    }
    
    function decrement() {
        count.value--
    }
    
    function reset() {
        count.value = initialValue
    }
    
    return {
        count,
        double,
        increment,
        decrement,
        reset
    }
}
```

**使用：**

```typescript
import { useCounter } from '@/composables/useCounter'

const { count, double, increment, decrement } = useCounter(10)
```

### 5.2 异步组合式函数

```typescript
// src/composables/useFetch.ts
import { ref, type Ref } from 'vue'

interface FetchOptions {
    method?: 'GET' | 'POST'
    body?: any
}

export function useFetch<T>(url: Ref<string> | string) {
    const data = ref<T | null>(null)
    const loading = ref(false)
    const error = ref<string | null>(null)
    
    async function fetchData(options?: FetchOptions) {
        loading.value = true
        error.value = null
        
        try {
            const response = await fetch(url.value, {
                method: options?.method || 'GET',
                body: options?.body ? JSON.stringify(options.body) : undefined
            })
            data.value = await response.json()
        } catch (e) {
            error.value = e instanceof Error ? e.message : 'Unknown error'
        } finally {
            loading.value = false
        }
    }
    
    return {
        data,
        loading,
        error,
        fetchData
    }
}
```

### 5.3 组合式函数最佳实践

```typescript
// 1. 保持函数纯净
// ✅ 好的：返回响应式状态
export function useUser(userId: number) {
    const user = ref(null)
    // ...
    return { user }
}

// 2. 依赖注入
// 使用 provide/inject 在组件间共享
import { provide, inject } from 'vue'

const ThemeKey = Symbol('theme')

// 父组件
provide(ThemeKey, 'dark')

// 子组件
const theme = inject(ThemeKey)
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：响应式 ==========
// 1. 用 ref 定义一个计数器
// 2. 用 reactive 定义一个用户对象
// 3. 用 readonly 包装一个响应式对象

// ========== 练习 2：计算属性 ==========
// 1. 根据购物车商品列表，计算总金额
// 2. 根据完成状态显示不同文字

// ========== 练习 3：监听器 ==========
// 1. 监听用户名变化，自动保存到 localStorage
// 2. 使用 watchEffect 实现同样功能

// ========== 练习 4：组合式函数 ==========
// 1. 创建一个 useLocalStorage 组合式函数
// 2. 创建一个 useMousePosition 组合式函数
```

---

## 🎯 今日自检清单

- [ ] ref 和 reactive 响应式
- [ ] computed 计算属性
- [ ] watch 和 watchEffect
- [ ] 组合式函数（Composables）
- [ ] 组件间通信（provide/inject）

---

## 下一步

明天我们将学习：**Vue 3 组件开发与 TypeScript**
