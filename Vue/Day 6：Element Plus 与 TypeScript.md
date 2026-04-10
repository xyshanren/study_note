> 学习日期：2026-04-21（周一）

## 学习目标

1. 掌握 Element Plus 安装与配置
2. 理解 Element Plus 组件的 TypeScript 使用
3. 能够创建类型安全的表单

---

## 1. Element Plus 安装

### 1.1 安装

```bash
npm install element-plus
```

### 1.2 main.ts 配置

```typescript
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```

### 1.3 按需导入（推荐）

```bash
# 安装自动导入插件
npm install -D unplugin-vue-components unplugin-auto-import
```

**vite.config.ts：**

```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  plugins: [
    vue(),
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

---

## 2. 基础组件使用

### 2.1 Button 按钮

```vue
<template>
  <el-button>默认按钮</el-button>
  <el-button type="primary">主要按钮</el-button>
  <el-button type="success">成功按钮</el-button>
  <el-button type="danger">危险按钮</el-button>
  <el-button :loading="true">加载中</el-button>
  <el-button icon="Search">搜索</el-button>
</template>
```

### 2.2 Icon 图标

```bash
# 安装图标库
npm install @element-plus/icons-vue
```

```typescript
import { Search, Plus, Edit } from '@element-plus/icons-vue'

// 注册全局图标
import * as icons from '@element-plus/icons-vue'

const app = createApp(App)
for (const [key, component] of Object.entries(icons)) {
  app.component(key, component)
}
```

```vue
<template>
  <el-icon><Search /></el-icon>
  <el-button :icon="Plus">添加</el-button>
</template>

<script setup lang="ts">
import { Search, Plus } from '@element-plus/icons-vue'
</script>
```

---

## 3. 表单组件 + TypeScript

### 3.1 基础表单

```vue
<script setup lang="ts">
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'

// 表单引用
const formRef = ref<FormInstance>()

// 表单数据
interface LoginForm {
    username: string
    password: string
    remember: boolean
}

const form = reactive<LoginForm>({
    username: '',
    password: '',
    remember: false
})

// 验证规则
const rules = reactive<FormRules<LoginForm>>({
    username: [
        { required: true, message: '请输入用户名', trigger: 'blur' },
        { min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur' }
    ],
    password: [
        { required: true, message: '请输入密码', trigger: 'blur' },
        { min: 6, message: '密码至少 6 位', trigger: 'blur' }
    ]
})

// 提交表单
async function submitForm() {
    if (!formRef.value) return
    
    await formRef.value.validate((valid) => {
        if (valid) {
            console.log('提交成功:', form)
        } else {
            console.log('验证失败')
        }
    })
}

// 重置表单
function resetForm() {
    formRef.value?.resetFields()
}
</script>

<template>
    <el-form
        ref="formRef"
        :model="form"
        :rules="rules"
        label-width="80px"
    >
        <el-form-item label="用户名" prop="username">
            <el-input v-model="form.username" placeholder="请输入用户名" />
        </el-form-item>
        
        <el-form-item label="密码" prop="password">
            <el-input v-model="form.password" type="password" show-password />
        </el-form-item>
        
        <el-form-item>
            <el-checkbox v-model="form.remember">记住我</el-checkbox>
        </el-form-item>
        
        <el-form-item>
            <el-button type="primary" @click="submitForm">登录</el-button>
            <el-button @click="resetForm">重置</el-button>
        </el-form-item>
    </el-form>
</template>
```

### 3.2 复杂表单 + 自定义验证

```vue
<script setup lang="ts">
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { ElMessage } from 'element-plus'

interface UserForm {
    name: string
    email: string
    phone: string
    role: string
    hobbies: string[]
    description: string
}

const formRef = ref<FormInstance>()

const form = reactive<UserForm>({
    name: '',
    email: '',
    phone: '',
    role: '',
    hobbies: [],
    description: ''
})

const roleOptions = [
    { label: '管理员', value: 'admin' },
    { label: '普通用户', value: 'user' },
    { label: '访客', value: 'guest' }
]

const hobbyOptions = [
    { label: '编程', value: 'coding' },
    { label: '阅读', value: 'reading' },
    { label: '运动', value: 'sports' }
]

// 自定义验证规则
const validatePhone = (rule: any, value: any, callback: any) => {
    if (value && !/^1[3-9]\d{9}$/.test(value)) {
        callback(new Error('请输入正确的手机号'))
    } else {
        callback()
    }
}

const rules = reactive<FormRules<UserForm>>({
    name: [
        { required: true, message: '请输入姓名', trigger: 'blur' }
    ],
    email: [
        { required: true, message: '请输入邮箱', trigger: 'blur' },
        { type: 'email', message: '请输入正确的邮箱格式', trigger: 'blur' }
    ],
    phone: [
        { validator: validatePhone, trigger: 'blur' }
    ],
    role: [
        { required: true, message: '请选择角色', trigger: 'change' }
    ]
})

async function submit() {
    try {
        await formRef.value?.validate()
        ElMessage.success('提交成功')
    } catch (err) {
        ElMessage.error('请检查表单')
    }
}
</script>
```

---

## 4. 表格组件 + TypeScript

### 4.1 基础表格

```vue
<script setup lang="ts">
import { ref, reactive } from 'vue'
import { Plus, Edit, Delete } from '@element-plus/icons-vue'
import { ElMessage, ElMessageBox } from 'element-plus'
import type { TableColumnCtx } from 'element-plus'

// 用户数据类型
interface User {
    id: number
    name: string
    email: string
    role: string
    status: boolean
    createTime: string
}

// 表格数据
const tableData = ref<User[]>([
    {
        id: 1,
        name: '张三',
        email: 'zhangsan@example.com',
        role: 'admin',
        status: true,
        createTime: '2024-01-01'
    }
])

// 表格列配置
const columns: TableColumnCtx<User>[] = [
    { prop: 'id', label: 'ID', width: 80 },
    { prop: 'name', label: '姓名' },
    { prop: 'email', label: '邮箱' },
    { prop: 'role', label: '角色' },
    { 
        prop: 'status', 
        label: '状态',
        formatter: (row: User) => row.status ? '启用' : '禁用'
    },
    { 
        prop: 'createTime', 
        label: '创建时间',
        width: 180
    }
]

// 选择
const selectedRows = ref<User[]>([])

function handleSelectionChange(rows: User[]) {
    selectedRows.value = rows
}

// 操作
function handleEdit(row: User) {
    ElMessage.info(`编辑: ${row.name}`)
}

async function handleDelete(row: User) {
    try {
        await ElMessageBox.confirm('确定删除该用户吗？', '提示', {
            type: 'warning'
        })
        // 删除逻辑
        ElMessage.success('删除成功')
    } catch {
        // 取消
    }
}
</script>

<template>
    <el-table
        :data="tableData"
        @selection-change="handleSelectionChange"
        stripe
    >
        <el-table-column type="selection" width="55" />
        <el-table-column 
            v-for="col in columns" 
            :key="col.prop"
            :prop="col.prop"
            :label="col.label"
            :width="col.width"
            :formatter="col.formatter"
        />
        <el-table-column label="操作" width="180" fixed="right">
            <template #default="{ row }">
                <el-button type="primary" :icon="Edit" @click="handleEdit(row)">编辑</el-button>
                <el-button type="danger" :icon="Delete" @click="handleDelete(row)">删除</el-button>
            </template>
        </el-table-column>
    </el-table>
</template>
```

### 4.2 带分页的表格

```typescript
import { ref, reactive } from 'vue'
import type { User } from './types'

// 分页配置
const pagination = reactive({
    current: 1,
    pageSize: 10,
    total: 0
})

// 加载数据
async function loadData() {
    const res = await fetch(`/api/users?page=${pagination.current}&size=${pagination.pageSize}`)
    const data = await res.json()
    
    tableData.value = data.list
    pagination.total = data.total
}

function handlePageChange(page: number) {
    pagination.current = page
    loadData()
}

function handleSizeChange(size: number) {
    pagination.pageSize = size
    loadData()
}
```

```vue
<template>
    <el-table :data="tableData">
        <!-- 列配置... -->
    </el-table>
    
    <el-pagination
        v-model:current-page="pagination.current"
        v-model:page-size="pagination.pageSize"
        :total="pagination.total"
        :page-sizes="[10, 20, 50, 100]"
        layout="total, sizes, prev, pager, next, jumper"
        @current-change="handlePageChange"
        @size-change="handleSizeChange"
    />
</template>
```

---

## 5. 弹窗组件 + TypeScript

### 5.1 基础弹窗表单

```vue
<script setup lang="ts">
import { ref, reactive } from 'vue'
import type { FormInstance } from 'element-plus'

// 弹窗显示状态
const dialogVisible = ref(false)

// 表单数据
interface FormData {
    id?: number
    name: string
    email: string
}

const form = reactive<FormData>({
    name: '',
    email: ''
})

const formRef = ref<FormInstance>()

// 编辑/新增模式
const isEdit = ref(false)

// 打开弹窗
function open(row?: FormData) {
    if (row) {
        // 编辑模式
        isEdit.value = true
        Object.assign(form, row)
    } else {
        // 新增模式
        isEdit.value = false
        Object.assign(form, { name: '', email: '' })
    }
    dialogVisible.value = true
}

// 提交
async function submit() {
    await formRef.value?.validate()
    
    if (isEdit.value) {
        // 更新
    } else {
        // 新增
    }
    
    dialogVisible.value = false
}

// 暴露方法给父组件
defineExpose({
    open
})
</script>

<template>
    <el-dialog
        v-model="dialogVisible"
        :title="isEdit ? '编辑用户' : '新增用户'"
        width="500px"
    >
        <el-form ref="formRef" :model="form" label-width="80px">
            <el-form-item label="姓名" prop="name">
                <el-input v-model="form.name" />
            </el-form-item>
            <el-form-item label="邮箱" prop="email">
                <el-input v-model="form.email" />
            </el-form-item>
        </el-form>
        
        <template #footer>
            <el-button @click="dialogVisible = false">取消</el-button>
            <el-button type="primary" @click="submit">确定</el-button>
        </template>
    </el-dialog>
</template>
```

---

## 6. 消息提示 TypeScript 类型

### 6.1 全局消息

```typescript
import { ElMessage, ElMessageBox, ElNotification } from 'element-plus'

// 成功消息
ElMessage.success('操作成功')

// 错误消息
ElMessage.error('操作失败')

// 确认框
await ElMessageBox.confirm('确定执行此操作吗？', '提示', {
    type: 'warning'
})

// 通知
ElNotification({
    title: '提示',
    message: '更新成功',
    type: 'success'
})
```

### 6.2 扩展 ElMessage 类型

```typescript
// 自定义消息类型
declare module 'element-plus' {
    interface ElMessageOptions {
        customClass?: string
        duration?: number
    }
}
```

---

## 📝 课后练习

```vue
<!-- ========== 练习 1：登录表单 ========== -->
<!-- 1. 创建登录表单，包含用户名、密码、验证码 -->
<!-- 2. 添加表单验证 -->
<!-- 3. 实现记住我功能 -->

<!-- ========== 练习 2：用户表格 ========== -->
<!-- 1. 创建用户列表表格 -->
<!-- 2. 实现分页功能 -->
<!-- 3. 实现多选和批量删除 -->

<!-- ========== 练习 3：CRUD 弹窗 ========== -->
<!-- 1. 创建用户编辑弹窗 -->
<!-- 2. 支持新增和编辑两种模式 -->
<!-- 3. 表单验证 -->

<!-- ========== 练习 4：TypeScript 类型 ========== -->
<!-- 1. 定义 Table、Form 的 TypeScript 类型 -->
<!-- 2. 使用泛型创建可复用的表格组件 -->
```

---

## 🎯 今日自检清单

- [ ] Element Plus 安装配置
- [ ] 按需导入配置
- [ ] 表单组件 + TypeScript
- [ ] 表格组件 + TypeScript
- [ ] 弹窗组件 + TypeScript
- [ ] 消息提示

---

## 下一步

明天我们将学习：**Vitest 单元测试**
