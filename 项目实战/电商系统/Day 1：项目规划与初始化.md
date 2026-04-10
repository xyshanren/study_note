> 学习日期：2026-05-12（周一）

## 项目概述

### 1.1 项目目标

创建一个前后端分离的电商系统，包含：
- **前端**: Vue 3 + TypeScript + Element Plus
- **后端**: Python FastAPI + PostgreSQL + Redis
- **移动端**: 鸿蒙 ArkTS

### 1.2 功能模块

```
电商系统功能模块
├── 用户模块
│   ├── 用户注册/登录
│   ├── 个人资料管理
│   ├── 地址管理
│   └── 订单历史
├── 商品模块
│   ├── 商品列表
│   ├── 商品详情
│   ├── 搜索与筛选
│   └── 分类管理
├── 购物车模块
│   ├── 添加/删除商品
│   ├── 数量修改
│   └── 价格计算
├── 订单模块
│   ├── 创建订单
│   ├── 订单列表
│   ├── 订单详情
│   └── 订单状态跟踪
└── 支付模块
    ├── 支付接口集成
    ├── 支付回调
    └── 退款处理
```

---

## 2. 技术架构

### 2.1 系统架构图

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   用户端    │     │   移动端    │     │   管理后台  │
│  (Vue Web)  │     │  (ArkTS)   │     │  (Vue Web)  │
└──────┬──────┘     └──────┬──────┘     └──────┬──────┘
       │                  │                  │
       └──────────────────┼──────────────────┘
                          │
                    ┌─────▼─────┐
                    │  API 网关  │
                    │  (Nginx)   │
                    └─────┬─────┘
                          │
         ┌────────────────┼────────────────┐
         │                │                │
   ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
   │  Vue SSR  │    │  FastAPI  │    │  FastAPI  │
   │  (前端)   │    │  (用户)   │    │  (管理)   │
   └─────┬─────┘    └─────┬─────┘    └─────┬─────┘
         │                │                │
         └────────────────┼────────────────┘
                          │
              ┌───────────┼───────────┐
              │           │           │
        ┌─────▼─────┐┌────▼────┐┌────▼────┐
        │ PostgreSQL ││  Redis  ││  MongoDB │
        │   (主库)   ││ (缓存)  ││ (日志)  │
        └───────────┘└─────────┘└─────────┘
```

### 2.2 项目结构

```
ecommerce/
├── frontend/                 # Vue 3 前端
│   ├── src/
│   │   ├── api/           # API 接口
│   │   ├── components/    # 公共组件
│   │   ├── views/         # 页面
│   │   ├── stores/        # Pinia Store
│   │   ├── router/        # 路由
│   │   └── utils/         # 工具
│   └── package.json
│
├── backend/                 # FastAPI 后端
│   ├── app/
│   │   ├── api/           # API 路由
│   │   ├── core/          # 核心配置
│   │   ├── models/        # 数据模型
│   │   ├── schemas/       # Pydantic 模型
│   │   ├── services/      # 业务逻辑
│   │   └── utils/         # 工具
│   ├── alembic/           # 数据库迁移
│   └── requirements.txt
│
└── mobile/                 # 鸿蒙移动端
    ├── src/
    │   ├── pages/         # 页面
    │   ├── components/    # 组件
    │   ├── services/      # 服务
    │   └── utils/         # 工具
    └── package.json
```

---

## 3. 数据库设计

### 3.1 用户表 (users)

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    hashed_password VARCHAR(255) NOT NULL,
    full_name VARCHAR(100),
    phone VARCHAR(20),
    avatar VARCHAR(255),
    role VARCHAR(20) DEFAULT 'user',
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
```

### 3.2 地址表 (addresses)

```sql
CREATE TABLE addresses (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    receiver_name VARCHAR(50) NOT NULL,
    phone VARCHAR(20) NOT NULL,
    province VARCHAR(50) NOT NULL,
    city VARCHAR(50) NOT NULL,
    district VARCHAR(50),
    detail_address VARCHAR(255) NOT NULL,
    is_default BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_addresses_user ON addresses(user_id);
```

### 3.3 商品分类表 (categories)

```sql
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    parent_id INTEGER REFERENCES categories(id),
    icon VARCHAR(100),
    sort_order INTEGER DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 3.4 商品表 (products)

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    category_id INTEGER REFERENCES categories(id),
    price DECIMAL(10, 2) NOT NULL,
    original_price DECIMAL(10, 2),
    stock INTEGER DEFAULT 0,
    images TEXT[],  -- JSON 数组
    status VARCHAR(20) DEFAULT 'active',
    sales_count INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_products_category ON products(category_id);
CREATE INDEX idx_products_status ON products(status);
CREATE INDEX idx_products_price ON products(price);
```

### 3.5 购物车表 (cart_items)

```sql
CREATE TABLE cart_items (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(id) ON DELETE CASCADE,
    quantity INTEGER NOT NULL DEFAULT 1,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    UNIQUE(user_id, product_id)
);

CREATE INDEX idx_cart_items_user ON cart_items(user_id);
```

### 3.6 订单表 (orders)

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    order_number VARCHAR(50) UNIQUE NOT NULL,
    user_id INTEGER REFERENCES users(id),
    total_amount DECIMAL(10, 2) NOT NULL,
    discount_amount DECIMAL(10, 2) DEFAULT 0,
    actual_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) DEFAULT 'pending',
    payment_method VARCHAR(20),
    payment_time TIMESTAMP,
    receiver_name VARCHAR(50),
    receiver_phone VARCHAR(20),
    receiver_address VARCHAR(255),
    remark TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_orders_user ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_number ON orders(order_number);
```

### 3.7 订单详情表 (order_items)

```sql
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INTEGER REFERENCES orders(id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(id),
    product_name VARCHAR(200) NOT NULL,
    product_image VARCHAR(255),
    price DECIMAL(10, 2) NOT NULL,
    quantity INTEGER NOT NULL,
    subtotal DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_order_items_order ON order_items(order_id);
```

---

## 4. API 设计

### 4.1 用户 API

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | /api/users/register | 用户注册 |
| POST | /api/users/login | 用户登录 |
| GET | /api/users/me | 获取当前用户信息 |
| PUT | /api/users/me | 更新用户信息 |
| GET | /api/users/addresses | 获取收货地址列表 |
| POST | /api/users/addresses | 添加收货地址 |
| PUT | /api/users/addresses/{id} | 更新收货地址 |
| DELETE | /api/users/addresses/{id} | 删除收货地址 |

### 4.2 商品 API

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | /api/products | 商品列表 |
| GET | /api/products/{id} | 商品详情 |
| GET | /api/categories | 商品分类 |
| GET | /api/products/search | 搜索商品 |

### 4.3 购物车 API

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | /api/cart | 获取购物车 |
| POST | /api/cart/items | 添加商品到购物车 |
| PUT | /api/cart/items/{id} | 更新商品数量 |
| DELETE | /api/cart/items/{id} | 删除购物车商品 |
| DELETE | /api/cart/clear | 清空购物车 |

### 4.4 订单 API

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | /api/orders | 创建订单 |
| GET | /api/orders | 订单列表 |
| GET | /api/orders/{id} | 订单详情 |
| PUT | /api/orders/{id}/cancel | 取消订单 |
| PUT | /api/orders/{id}/confirm | 确认收货 |

---

## 5. 前端项目初始化

### 5.1 创建 Vue 项目

```bash
# 创建项目
npm create vite@latest ecommerce-web -- --template vue-ts

cd ecommerce-web

# 安装依赖
npm install vue-router@4 pinia element-plus @element-plus/icons-vue axios

# 安装开发依赖
npm install -D sass unplugin-vue-components unplugin-auto-import @types/node
```

### 5.2 配置 Element Plus

```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
import { resolve } from 'path'

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
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src')
    }
  }
})
```

---

## 📝 课后练习

```bash
# 1. 初始化前端项目
npm create vite@latest ecommerce-web -- --template vue-ts

# 2. 安装依赖
cd ecommerce-web && npm install

# 3. 创建数据库表
# 参考上面的 SQL 创建表

# 4. 了解项目结构
```

---

## 🎯 今日自检清单

- [ ] 了解项目功能模块
- [ ] 理解系统架构
- [ ] 掌握数据库设计
- [ ] 了解 API 设计
- [ ] 初始化前端项目

---

## 下一步

明天我们将开发：**电商系统前端 - 商品模块**
