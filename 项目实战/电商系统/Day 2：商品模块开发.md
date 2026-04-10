> 学习日期：2026-05-13（周二）

## 学习目标

1. 实现商品列表页面
2. 实现商品搜索与筛选
3. 实现商品详情页面

---

## 1. API 接口定义

### 1.1 API 服务封装

```typescript
// src/api/request.ts
import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios'
import { ElMessage } from 'element-plus'

const service: AxiosInstance = axios.create({
    baseURL: import.meta.env.VITE_API_BASE_URL || '/api',
    timeout: 10000
})

// 请求拦截器
service.interceptors.request.use(
    config => {
        const token = localStorage.getItem('token')
        if (token) {
            config.headers.Authorization = `Bearer ${token}`
        }
        return config
    },
    error => {
        return Promise.reject(error)
    }
)

// 响应拦截器
service.interceptors.response.use(
    response => {
        const res = response.data
        if (res.code !== 200) {
            ElMessage.error(res.message || '请求失败')
            return Promise.reject(new Error(res.message))
        }
        return res.data
    },
    error => {
        ElMessage.error(error.message || '网络错误')
        return Promise.reject(error)
    }
)

export default service
```

### 1.2 商品 API

```typescript
// src/api/product.ts
import request from './request'
import type { 
    Product, 
    ProductListParams, 
    ProductListResponse,
    Category 
} from './types'

// 获取商品列表
export function getProductList(params: ProductListParams) {
    return request.get<ProductListResponse>('/products', { params })
}

// 获取商品详情
export function getProductDetail(id: number) {
    return request.get<Product>(`/products/${id}`)
}

// 获取商品分类
export function getCategories() {
    return request.get<Category[]>('/categories')
}

// 搜索商品
export function searchProducts(keyword: string) {
    return request.get<ProductListResponse>('/products/search', { 
        params: { keyword } 
    })
}
```

### 1.3 类型定义

```typescript
// src/api/types.ts

// 商品
export interface Product {
    id: number
    name: string
    description: string
    price: number
    originalPrice?: number
    images: string[]
    stock: number
    salesCount: number
    categoryId: number
    categoryName?: string
    status: 'active' | 'inactive'
    createdAt: string
}

// 商品列表参数
export interface ProductListParams {
    page?: number
    pageSize?: number
    categoryId?: number
    keyword?: string
    minPrice?: number
    maxPrice?: number
    sortBy?: 'price' | 'sales' | 'created'
    sortOrder?: 'asc' | 'desc'
}

// 商品列表响应
export interface ProductListResponse {
    list: Product[]
    total: number
    page: number
    pageSize: number
}

// 分类
export interface Category {
    id: number
    name: string
    parentId?: number
    children?: Category[]
}
```

---

## 2. 商品列表页面

### 2.1 页面结构

```vue
<!-- src/views/product/ProductList.vue -->
<template>
    <div class="product-list">
        <!-- 筛选栏 -->
        <div class="filter-bar">
            <el-row :gutter="20">
                <el-col :span="4">
                    <el-select 
                        v-model="filters.categoryId" 
                        placeholder="选择分类"
                        clearable
                        @change="handleFilterChange"
                    >
                        <el-option
                            v-for="cat in categories"
                            :key="cat.id"
                            :label="cat.name"
                            :value="cat.id"
                        />
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <el-input
                        v-model="filters.keyword"
                        placeholder="搜索商品"
                        clearable
                        @keyup.enter="handleSearch"
                    >
                        <template #append>
                            <el-button :icon="Search" @click="handleSearch" />
                        </template>
                    </el-input>
                </el-col>
                <el-col :span="4">
                    <el-input
                        v-model.number="filters.minPrice"
                        placeholder="最低价"
                        type="number"
                        @change="handleFilterChange"
                    />
                </el-col>
                <el-col :span="4">
                    <el-input
                        v-model.number="filters.maxPrice"
                        placeholder="最高价"
                        type="number"
                        @change="handleFilterChange"
                    />
                </el-col>
            </el-row>
            
            <!-- 排序 -->
            <div class="sort-bar">
                <span>排序：</span>
                <el-radio-group 
                    v-model="filters.sortBy" 
                    @change="handleFilterChange"
                >
                    <el-radio label="created">最新</el-radio>
                    <el-radio label="sales">销量</el-radio>
                    <el-radio label="price">价格</el-radio>
                </el-radio-group>
                <el-button-group>
                    <el-button 
                        :type="filters.sortOrder === 'asc' ? 'primary' : ''"
                        @click="handleSortOrder('asc')"
                    >
                        ↑
                    </el-button>
                    <el-button 
                        :type="filters.sortOrder === 'desc' ? 'primary' : ''"
                        @click="handleSortOrder('desc')"
                    >
                        ↓
                    </el-button>
                </el-button-group>
            </div>
        </div>
        
        <!-- 商品列表 -->
        <div class="product-grid" v-loading="loading">
            <el-row :gutter="20">
                <el-col 
                    v-for="product in products" 
                    :key="product.id"
                    :xs="12" :sm="8" :md="6" :lg="4"
                >
                    <ProductCard :product="product" @click="goToDetail(product.id)" />
                </el-col>
            </el-row>
            
            <!-- 空状态 -->
            <el-empty v-if="!loading && products.length === 0" description="暂无商品" />
        </div>
        
        <!-- 分页 -->
        <div class="pagination">
            <el-pagination
                v-model:current-page="pagination.page"
                v-model:page-size="pagination.pageSize"
                :total="pagination.total"
                :page-sizes="[12, 24, 48]"
                layout="total, sizes, prev, pager, next, jumper"
                @size-change="handleSizeChange"
                @current-change="handlePageChange"
            />
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { Search } from '@element-plus/icons-vue'
import { getProductList, getCategories } from '@/api/product'
import type { Product, ProductListParams, Category } from '@/api/types'
import ProductCard from '@/components/ProductCard.vue'

const router = useRouter()

// 筛选条件
const filters = reactive<ProductListParams>({
    page: 1,
    pageSize: 12,
    keyword: '',
    categoryId: undefined,
    minPrice: undefined,
    maxPrice: undefined,
    sortBy: 'created',
    sortOrder: 'desc'
})

// 分页
const pagination = reactive({
    page: 1,
    pageSize: 12,
    total: 0
})

// 数据
const products = ref<Product[]>([])
const categories = ref<Category[]>([])
const loading = ref(false)

// 获取商品列表
async function fetchProducts() {
    loading.value = true
    try {
        const data = await getProductList({
            ...filters,
            page: pagination.page,
            pageSize: pagination.pageSize
        })
        products.value = data.list
        pagination.total = data.total
    } finally {
        loading.value = false
    }
}

// 获取分类
async function fetchCategories() {
    categories.value = await getCategories()
}

// 处理筛选变化
function handleFilterChange() {
    pagination.page = 1
    fetchProducts()
}

// 搜索
function handleSearch() {
    pagination.page = 1
    fetchProducts()
}

// 排序
function handleSortOrder(order: 'asc' | 'desc') {
    filters.sortOrder = order
    handleFilterChange()
}

// 分页
function handlePageChange(page: number) {
    pagination.page = page
    fetchProducts()
}

function handleSizeChange(size: number) {
    pagination.pageSize = size
    pagination.page = 1
    fetchProducts()
}

// 跳转到详情
function goToDetail(id: number) {
    router.push(`/product/${id}`)
}

onMounted(() => {
    fetchCategories()
    fetchProducts()
})
</script>

<style scoped lang="scss">
.product-list {
    padding: 20px;
}

.filter-bar {
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.sort-bar {
    margin-top: 15px;
    display: flex;
    align-items: center;
    gap: 15px;
}

.product-grid {
    min-height: 400px;
}

.pagination {
    margin-top: 20px;
    display: flex;
    justify-content: center;
}
</style>
```

---

## 3. 商品卡片组件

### 3.1 组件代码

```vue
<!-- src/components/ProductCard.vue -->
<template>
    <div class="product-card" @click="handleClick">
        <!-- 图片 -->
        <div class="image-wrapper">
            <img 
                :src="product.images[0] || '/placeholder.png'" 
                :alt="product.name"
                class="product-image"
            />
            <div class="tags">
                <span v-if="product.stock === 0" class="tag sold-out">缺货</span>
                <span v-else-if="product.originalPrice" class="tag discount">
                    {{ discount }}% OFF
                </span>
            </div>
        </div>
        
        <!-- 信息 -->
        <div class="product-info">
            <h3 class="name">{{ product.name }}</h3>
            <p class="description">{{ product.description }}</p>
            
            <div class="price-row">
                <span class="price">¥{{ product.price }}</span>
                <span v-if="product.originalPrice" class="original-price">
                    ¥{{ product.originalPrice }}
                </span>
            </div>
            
            <div class="meta">
                <span class="sales">{{ product.salesCount }} 人付款</span>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import type { Product } from '@/api/types'

const props = defineProps<{
    product: Product
}>()

const emit = defineEmits<{
    (e: 'click'): void
}>()

// 计算折扣
const discount = computed(() => {
    if (!props.product.originalPrice) return 0
    return Math.round(
        (1 - props.product.price / props.product.originalPrice) * 100
    )
})

function handleClick() {
    emit('click')
}
</script>

<style scoped lang="scss">
.product-card {
    background: #fff;
    border-radius: 8px;
    overflow: hidden;
    cursor: pointer;
    transition: all 0.3s;
    
    &:hover {
        transform: translateY(-5px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    }
}

.image-wrapper {
    position: relative;
    width: 100%;
    padding-top: 100%; // 1:1 比例
    overflow: hidden;
    
    .product-image {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    
    .tags {
        position: absolute;
        top: 10px;
        left: 10px;
        
        .tag {
            display: inline-block;
            padding: 2px 8px;
            font-size: 12px;
            color: #fff;
            border-radius: 4px;
            margin-right: 5px;
            
            &.sold-out {
                background: #999;
            }
            
            &.discount {
                background: #ff4d4f;
            }
        }
    }
}

.product-info {
    padding: 12px;
}

.name {
    font-size: 14px;
    font-weight: 500;
    margin: 0 0 8px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.description {
    font-size: 12px;
    color: #999;
    margin: 0 0 8px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.price-row {
    display: flex;
    align-items: baseline;
    gap: 8px;
    margin-bottom: 8px;
    
    .price {
        font-size: 18px;
        font-weight: 600;
        color: #ff4d4f;
    }
    
    .original-price {
        font-size: 12px;
        color: #999;
        text-decoration: line-through;
    }
}

.meta {
    font-size: 12px;
    color: #999;
}
</style>
```

---

## 4. 商品详情页面

### 4.1 页面代码

```vue
<!-- src/views/product/ProductDetail.vue -->
<template>
    <div class="product-detail" v-loading="loading">
        <!-- 面包屑 -->
        <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>{{ product.categoryName }}</el-breadcrumb-item>
            <el-breadcrumb-item>{{ product.name }}</el-breadcrumb-item>
        </el-breadcrumb>
        
        <!-- 商品主体 -->
        <el-row :gutter="40" class="product-main">
            <!-- 图片 -->
            <el-col :span="10">
                <div class="image-preview">
                    <img 
                        :src="currentImage" 
                        :alt="product.name"
                        class="main-image"
                    />
                    <div class="thumbnail-list">
                        <div
                            v-for="(img, index) in product.images"
                            :key="index"
                            :class="['thumbnail', { active: currentImageIndex === index }]"
                            @click="currentImageIndex = index"
                        >
                            <img :src="img" />
                        </div>
                    </div>
                </div>
            </el-col>
            
            <!-- 信息 -->
            <el-col :span="14">
                <div class="product-info">
                    <h1 class="title">{{ product.name }}</h1>
                    <p class="description">{{ product.description }}</p>
                    
                    <div class="price-section">
                        <span class="price">¥{{ product.price }}</span>
                        <span v-if="product.originalPrice" class="original-price">
                            ¥{{ product.originalPrice }}
                        </span>
                    </div>
                    
                    <el-divider />
                    
                    <div class="specs">
                        <div class="spec-item">
                            <span class="label">配送至：</span>
                            <el-select v-model="addressId" placeholder="请选择">
                                <el-option
                                    v-for="addr in addresses"
                                    :key="addr.id"
                                    :label="addr.province + addr.city + addr.detailAddress"
                                    :value="addr.id"
                                />
                            </el-select>
                        </div>
                        
                        <div class="spec-item">
                            <span class="label">数量：</span>
                            <el-input-number 
                                v-model="quantity" 
                                :min="1" 
                                :max="product.stock"
                            />
                            <span class="stock">库存 {{ product.stock }} 件</span>
                        </div>
                    </div>
                    
                    <div class="actions">
                        <el-button 
                            type="primary" 
                            size="large"
                            :disabled="product.stock === 0"
                            @click="handleAddToCart"
                        >
                            加入购物车
                        </el-button>
                        <el-button 
                            size="large"
                            :disabled="product.stock === 0"
                            @click="handleBuyNow"
                        >
                            立即购买
                        </el-button>
                    </div>
                </div>
            </el-col>
        </el-row>
        
        <!-- 商品详情 -->
        <div class="product-detail-section">
            <h2>商品详情</h2>
            <div class="detail-content" v-html="product.detailHtml"></div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'
import { getProductDetail } from '@/api/product'
import { addToCart } from '@/api/cart'
import type { Product } from '@/api/types'

const route = useRoute()
const router = useRouter()

const loading = ref(false)
const product = ref<Product>({
    id: 0,
    name: '',
    description: '',
    price: 0,
    originalPrice: 0,
    images: [],
    stock: 0,
    salesCount: 0,
    categoryId: 0,
    categoryName: '',
    status: 'active',
    createdAt: ''
})

const currentImageIndex = ref(0)
const currentImage = ref('')
const quantity = ref(1)
const addressId = ref<number | null>(null)
const addresses = ref<any[]>([])

// 获取商品详情
async function fetchProduct() {
    loading.value = true
    try {
        const id = Number(route.params.id)
        product.value = await getProductDetail(id)
        currentImage.value = product.value.images[0] || ''
    } finally {
        loading.value = false
    }
}

// 加入购物车
async function handleAddToCart() {
    try {
        await addToCart({
            productId: product.value.id,
            quantity: quantity.value
        })
        ElMessage.success('已加入购物车')
    } catch (e) {
        ElMessage.error('加入购物车失败')
    }
}

// 立即购买
function handleBuyNow() {
    router.push({
        path: '/order/confirm',
        query: {
            productId: product.value.id,
            quantity: quantity.value.toString()
        }
    })
}

onMounted(() => {
    fetchProduct()
})
</script>

<style scoped lang="scss">
.product-detail {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.product-main {
    margin-top: 20px;
}

.image-preview {
    .main-image {
        width: 100%;
        border-radius: 8px;
    }
    
    .thumbnail-list {
        display: flex;
        gap: 10px;
        margin-top: 10px;
        
        .thumbnail {
            width: 60px;
            height: 60px;
            border: 2px solid transparent;
            border-radius: 4px;
            cursor: pointer;
            overflow: hidden;
            
            &.active {
                border-color: #409eff;
            }
            
            img {
                width: 100%;
                height: 100%;
                object-fit: cover;
            }
        }
    }
}

.product-info {
    .title {
        font-size: 24px;
        font-weight: 600;
        margin: 0 0 10px;
    }
    
    .description {
        color: #666;
        margin: 0 0 20px;
    }
    
    .price-section {
        .price {
            font-size: 32px;
            font-weight: 600;
            color: #ff4d4f;
        }
        
        .original-price {
            font-size: 16px;
            color: #999;
            text-decoration: line-through;
            margin-left: 10px;
        }
    }
    
    .specs {
        .spec-item {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
            
            .label {
                width: 80px;
                color: #666;
            }
            
            .stock {
                color: #999;
            }
        }
    }
    
    .actions {
        margin-top: 30px;
    }
}

.product-detail-section {
    margin-top: 40px;
    
    h2 {
        font-size: 20px;
        margin-bottom: 20px;
    }
    
    .detail-content {
        line-height: 2;
    }
}
</style>
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：完善商品列表 ==========
// 1. 实现分类筛选
// 2. 实现价格区间筛选

// ========== 练习 2：完善商品详情 ==========
// 1. 实现图片切换
// 2. 实现 SKU 选择

// ========== 练习 3：实现收藏功能 ==========
// 1. 添加收藏按钮
// 2. 收藏列表页面
```

---

## 🎯 今日自检清单

- [ ] 封装 API 请求
- [ ] 实现商品列表
- [ ] 实现筛选和排序
- [ ] 实现商品详情页
- [ ] 实现加入购物车

---

## 下一步

明天我们将学习：**购物车与订单模块**
