> 学习日期：2026-04-17（周四）

## 学习目标

1. 掌握 Vue Router 4 安装与配置
2. 理解路由守卫与权限控制
3. 能够使用 TypeScript 定义路由

---

## 1. Vue Router 4 安装

### 1.1 安装

```bash
npm install vue-router@4
```

### 1.2 项目结构

```
src/
├── router/
│   ├── index.ts          # 路由配置
│   └── routes/           # 路由模块
│       ├── index.ts
│       ├── user.ts
│       └── admin.ts
├── views/                # 页面组件
│   ├── HomePage.vue
│   ├── AboutPage.vue
│   └── UserPage.vue
└── App.vue
```

---

## 2. 路由配置

### 2.1 基础配置

```typescript
// src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'

// 路由配置
const routes: RouteRecordRaw[] = [
    {
        path: '/',
        name: 'Home',
        component: () => import('@/views/HomePage.vue')
    },
    {
        path: '/about',
        name: 'About',
        component: () => import('@/views/AboutPage.vue')
    },
    {
        path: '/user/:id',
        name: 'User',
        component: () => import('@/views/UserPage.vue'),
        props: true
    }
]

// 创建路由实例
const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router
```

### 2.2 路由模块化

```typescript
// src/router/routes/home.ts
import type { RouteRecordRaw } from 'vue-router'

export default {
    path: '/',
    name: 'Home',
    component: () => import('@/views/HomePage.vue'),
    meta: {
        title: '首页'
    }
} as RouteRecordRaw
```

```typescript
// src/router/routes/user.ts
import type { RouteRecordRaw } from 'vue-router'

export default {
    path: '/user',
    name: 'User',
    component: () => import('@/views/UserPage.vue'),
    children: [
        {
            path: 'profile',
            name: 'UserProfile',
            component: () => import('@/views/user/Profile.vue'),
            meta: { title: '个人资料' }
        },
        {
            path: 'settings',
            name: 'UserSettings',
            component: () => import('@/views/user/Settings.vue'),
            meta: { title: '设置' }
        }
    ]
} as RouteRecordRaw
```

```typescript
// src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'

// 导入所有路由模块
import homeRoutes from './routes/home'
import userRoutes from './routes/user'

const routes: RouteRecordRaw[] = [
    homeRoutes,
    userRoutes,
    // 404 路由
    {
        path: '/:pathMatch(.*)*',
        name: 'NotFound',
        component: () => import('@/views/NotFound.vue')
    }
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router
```

---

## 3. 路由守卫

### 3.1 全局守卫

```typescript
// src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'
import { useUserStore } from '@/stores/user'

const router = createRouter({
    history: createWebHistory(),
    routes: []
})

// 全局前置守卫
router.beforeEach((to, from, next) => {
    // 设置页面标题
    document.title = (to.meta.title as string) || 'Vue App'
    
    // 检查是否需要登录
    const requiresAuth = to.meta.requiresAuth
    
    if (requiresAuth) {
        const userStore = useUserStore()
        if (userStore.isLoggedIn) {
            next()
        } else {
            next({ name: 'Login', query: { redirect: to.fullPath } })
        }
    } else {
        next()
    }
})

// 全局后置守卫
router.afterEach((to, from) => {
    console.log(`导航到: ${to.path}`)
})

export default router
```

### 3.2 路由独享守卫

```typescript
{
    path: '/admin',
    name: 'Admin',
    component: () => import('@/views/Admin.vue'),
    meta: { requiresAuth: true, role: 'admin' },
    beforeEnter: (to, from, next) => {
        // 权限检查
        const userStore = useUserStore()
        if (userStore.role === 'admin') {
            next()
        } else {
            next({ name: 'Home' })
        }
    }
}
```

### 3.3 组件内守卫

```vue
<script setup lang="ts">
import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'

// 进入路由前
onBeforeRouteUpdate((to, from) => {
    console.log(`从 ${from.path} 更新到 ${to.path}`)
})

// 离开路由前
onBeforeRouteLeave((to, from) => {
    const answer = window.confirm('确定要离开吗？')
    if (answer) {
        return true
    } else {
        return false
    }
})
</script>
```

---

## 4. TypeScript 路由类型

### 4.1 路由记录类型

```typescript
import type { 
    RouteRecordRaw, 
    RouteLocationNormalized 
} from 'vue-router'

// 自定义路由元数据
interface CustomMeta {
    title?: string
    requiresAuth?: boolean
    role?: 'admin' | 'user' | 'guest'
    keepAlive?: boolean
}

// 扩展路由记录
interface CustomRouteRecord extends RouteRecordRaw {
    meta: CustomMeta
}

const routes: CustomRouteRecord[] = [
    {
        path: '/admin',
        name: 'Admin',
        component: () => import('@/views/Admin.vue'),
        meta: {
            requiresAuth: true,
            role: 'admin'
        }
    }
]
```

### 4.2 获取路由信息类型

```typescript
import { useRoute, useRouter } from 'vue-router'
import type { 
    RouteLocationNormalized, 
    Router 
} from 'vue-router'

// 在组件中使用
const route = useRoute()

// 路由参数类型化
const userId = route.params.id as string
const query = route.query.page as string

// 路由元数据
const title = route.meta.title as string

// 编程式导航
const router = useRouter()

function navigateToUser(id: number) {
    router.push({
        name: 'User',
        params: { id: id.toString() }
    })
}
```

---

## 5. 动态路由

### 5.1 动态添加路由

```typescript
// 动态添加路由
function addRoute() {
    router.addRoute({
        path: '/dynamic',
        name: 'Dynamic',
        component: () => import('@/views/Dynamic.vue')
    })
}

// 动态添加子路由
function addChildRoute() {
    router.addRoute('parent', {
        path: 'child',
        name: 'Child',
        component: () => import('@/views/Child.vue')
    })
}

// 移除路由
function removeRoute(name: string) {
    router.removeRoute(name)
}
```

### 5.2 权限路由示例

```typescript
// src/router/permission.ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'
import { useUserStore } from '@/stores/user'

// 静态路由
const constantRoutes: RouteRecordRaw[] = [
    {
        path: '/login',
        name: 'Login',
        component: () => import('@/views/Login.vue')
    },
    {
        path: '/',
        name: 'Home',
        component: () => import('@/views/Home.vue')
    }
]

// 动态路由（需要权限）
const asyncRoutes: RouteRecordRaw[] = [
    {
        path: '/admin',
        name: 'Admin',
        component: () => import('@/views/Admin.vue'),
        meta: { requiresAuth: true, role: 'admin' }
    }
]

export function createPermissionRouter() {
    const router = createRouter({
        history: createWebHistory(),
        routes: constantRoutes
    })

    // 动态添加路由
    router.beforeEach(async (to, from, next) => {
        const userStore = useUserStore()
        
        // 首次加载时获取用户信息
        if (userStore.isLoggedIn && !userStore.routesLoaded) {
            const accessedRoutes = filterAsyncRoutes(asyncRoutes, userStore.role)
            accessedRoutes.forEach(route => {
                router.addRoute(route)
            })
            userStore.routesLoaded = true
            next({ ...to, replace: true })
        } else {
            next()
        }
    })

    return router
}

// 过滤路由
function filterAsyncRoutes(routes: RouteRecordRaw[], role: string) {
    return routes.filter(route => {
        if (route.meta?.role === role) {
            return true
        }
        return false
    })
}
```

---

## 6. 路由状态管理

### 6.1 Pinia 中管理路由权限

```typescript
// src/stores/permission.ts
import { defineStore } from 'pinia'
import { ref } from 'vue'
import { 
    staticRoutes, 
    asyncRoutes 
} from '@/router/routes'
import type { RouteRecordRaw } from 'vue-router'

export const usePermissionStore = defineStore('permission', () => {
    const routes = ref<RouteRecordRaw[]>([])
    const dynamicRoutesAdded = ref(false)

    function setRoutes(newRoutes: RouteRecordRaw[]) {
        routes.value = newRoutes
    }

    function addRoutes(asyncRts: RouteRecordRaw[]) {
        routes.value = [...staticRoutes, ...asyncRts]
    }

    function reset() {
        routes.value = []
        dynamicRoutesAdded.value = false
    }

    return {
        routes,
        dynamicRoutesAdded,
        setRoutes,
        addRoutes,
        reset
    }
})
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：基础路由 ==========
// 1. 创建首页、关于页、用户页路由
// 2. 配置路径别名

// ========== 练习 2：路由守卫 ==========
// 1. 实现登录验证守卫
// 2. 实现角色权限守卫

// ========== 练习 3：动态路由 ==========
// 1. 实现动态添加路由
// 2. 实现权限控制

// ========== 练习 4：TypeScript 类型 ==========
// 1. 定义带 meta 的路由类型
// 2. 在组件中使用类型化的路由
```

---

## 🎯 今日自检清单

- [ ] Vue Router 4 安装配置
- [ ] 路由模块化
- [ ] 全局/路由/组件守卫
- [ ] TypeScript 路由类型
- [ ] 动态路由与权限

---

## 下一步

明天我们将学习：**Pinia 状态管理 + TypeScript**
