> 学习日期：2026-04-16（周三）

## 学习目标

1. 掌握 Vue 3 组件的 TypeScript 类型定义
2. 理解 Props 和 Emit 的类型化
3. 能够创建类型安全的 Vue 组件

---

## 1. Props 类型定义

### 1.1 基础 Props

```vue
<script setup lang="ts">
// 方式1：类型注解（推荐）
interface Props {
    title: string
    count?: number      // 可选属性
    items: string[]
}

const props = defineProps<Props>()
</script>
```

### 1.2 带默认值的 Props

```typescript
// 方式1：withDefaults（推荐）
interface Props {
    title: string
    size?: 'small' | 'medium' | 'large'
    disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
    size: 'medium',
    disabled: false
})
```

```vue
<!-- 使用 -->
<MyComponent title="Hello" size="large" />
<MyComponent title="Hello" />  <!-- 使用默认值 -->
```

### 1.3 复杂类型 Props

```typescript
interface User {
    id: number
    name: string
    email: string
}

interface Props {
    user: User
    tags: string[]
    metadata: Record<string, any>
}

const props = defineProps<Props>()

// 访问
console.log(props.user.name)
console.log(props.tags[0])
```

---

## 2. Emit 类型定义

### 2.1 基础 Emit

```vue
<script setup lang="ts">
// 方式1：使用类型化 emit（推荐）
const emit = defineEmits<{
    (e: 'update', value: string): void
    (e: 'delete', id: number): void
}>()

function handleClick() {
    emit('update', 'new value')
    emit('delete', 1)
}
</script>

<template>
    <button @click="handleClick">操作</button>
</template>
```

### 2.2 使用接口简化

```typescript
// 定义 emit 类型接口
interface Emits {
    (e: 'change', id: number, value: string): void
    (e: 'update', value: boolean): void
    (e: 'delete'): void
}

const emit = defineEmits<Emits>()
```

### 2.3 v3.3+ 简化语法

```typescript
// Vue 3.3+ 支持更简洁的语法
const emit = defineEmits<{
    change: [id: number, value: string]      // 具名元组
    update: [value: boolean]
    delete: []
}>()
```

---

## 3. 组件类型化实践

### 3.1 完整示例

```vue
<script setup lang="ts">
// ===== Props 定义 =====
interface Props {
    title: string
    type?: 'primary' | 'success' | 'danger'
    loading?: boolean
    items: {
        id: number
        label: string
    }[]
}

const props = withDefaults(defineProps<Props>(), {
    type: 'primary',
    loading: false
})

// ===== Emit 定义 =====
const emit = defineEmits<{
    (e: 'click', id: number): void
    (e: 'update:title', value: string): void
    (e: 'change', payload: { id: number; value: string }): void
}>()

// ===== 方法 =====
function handleItemClick(id: number) {
    emit('click', id)
}
</script>

<template>
    <div class="list">
        <h2>{{ title }}</h2>
        <div v-if="loading">加载中...</div>
        <ul v-else>
            <li 
                v-for="item in items" 
                :key="item.id"
                @click="handleItemClick(item.id)"
            >
                {{ item.label }}
            </li>
        </ul>
    </div>
</template>
```

### 3.2 父组件使用

```vue
<script setup lang="ts">
import { ref } from 'vue'
import List from './List.vue'

interface Item {
    id: number
    label: string
}

const items = ref<Item[]>([
    { id: 1, label: '项目一' },
    { id: 2, label: '项目二' }
])

function handleClick(id: number) {
    console.log('点击了:', id)
}
</script>

<template>
    <List
        title="我的列表"
        type="success"
        :items="items"
        @click="handleClick"
    />
</template>
```

---

## 4. 组件实例类型

### 4.1 获取子组件实例

```vue
<!-- Parent.vue -->
<script setup lang="ts">
import { ref } from 'vue'
import Child from './Child.vue'

// 定义子组件实例类型
import type ChildComponent from './Child.vue'

const childRef = ref<InstanceType<typeof ChildComponent> | null>(null)

function callChildMethod() {
    childRef.value?.childMethod()
}
</script>

<template>
    <Child ref="childRef" />
</template>
```

### 4.2 Expose 暴露方法

```vue
<!-- Child.vue -->
<script setup lang="ts">
function childMethod() {
    console.log('子组件方法被调用')
}

// 暴露给父组件
defineExpose({
    childMethod
})
</script>
```

---

## 5. 插槽类型

### 5.1 基础插槽

```vue
<!-- Card.vue -->
<script setup lang="ts">
interface Props {
    title: string
}

defineProps<Props>()
</script>

<template>
    <div class="card">
        <header>{{ title }}</header>
        <main>
            <slot />
        </main>
    </div>
</template>
```

### 5.2 具名插槽

```vue
<!-- Modal.vue -->
<script setup lang="ts">
defineProps<{
    visible: boolean
}>()

defineEmits<{
    (e: 'close'): void
}>()
</script>

<template>
    <div v-if="visible" class="modal">
        <header>
            <slot name="header">默认标题</slot>
        </header>
        <main>
            <slot />
        </main>
        <footer>
            <slot name="footer">
                <button @click="$emit('close')">关闭</button>
            </slot>
        </footer>
    </div>
</template>
```

**使用：**

```vue
<Modal :visible="showModal" @close="showModal = false">
    <template #header>自定义标题</template>
    <p>内容</p>
    <template #footer>
        <button>确定</button>
    </template>
</Modal>
```

### 5.3 作用域插槽

```vue
<!-- List.vue -->
<script setup lang="ts">
interface Props {
    items: string[]
}

defineProps<Props>()

defineEmits<{
    (e: 'select', item: string): void
}>()
</script>

<template>
    <ul>
        <li 
            v-for="(item, index) in items" 
            :key="item"
        >
            <slot :item="item" :index="index" />
        </li>
    </ul>
</template>
```

**使用：**

```vue
<List :items="['a', 'b', 'c']" #="{ item, index }">
    {{ index + 1 }}. {{ item }}
</List>
```

---

## 6. 泛型组件

### 6.1 v3.3+ 泛型组件

```vue
<script setup lang="ts" generic="T extends { id: number; name: string }">
import { defineProps } from 'vue'

interface Props<T> {
    items: T[]
    selectedId?: number
}

const props = defineProps<Props<T>>()

defineEmits<{
    (e: 'select', item: T): void
}>()
</script>

<template>
    <div>
        <div 
            v-for="item in items" 
            :key="item.id"
            @click="$emit('select', item)"
        >
            {{ item.name }}
        </div>
    </div>
</template>
```

**使用：**

```vue
<script setup lang="ts">
interface User {
    id: number
    name: string
    email: string
}

const users = ref<User[]>([
    { id: 1, name: '张三', email: 'zhangsan@example.com' }
])
</script>

<template>
    <UserList :items="users" @select="handleSelect" />
</template>
```

---

## 📝 课后练习

```vue
<!-- ========== 练习 1：Props ========== -->
<!-- 创建一个 Button 组件 -->
<!-- - variant: 'primary' | 'secondary' | 'danger' -->
<!-- - disabled: boolean -->
<!-- - text: string -->

<!-- ========== 练习 2：Emit ========== -->
<!-- 创建一个 Input 组件 -->
<!-- - modelValue: string -->
<!-- - @update:modelValue -->
<!-- - @change -->

<!-- ========== 练习 3：插槽 ========== -->
<!-- 创建一个 Table 组件，支持作用域插槽 -->

<!-- ========== 练习 4：泛型组件 ========== -->
<!-- 创建一个 List 组件，支持泛型 T -->
```

---

## 🎯 今日自检清单

- [ ] Props 类型定义
- [ ] Emit 类型定义
- [ ] 组件实例类型
- [ ] 插槽类型
- [ ] 泛型组件（Vue 3.3+）

---

## 下一步

明天我们将学习：**Vue Router + TypeScript**
