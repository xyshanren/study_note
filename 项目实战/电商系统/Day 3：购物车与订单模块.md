> 学习日期：2026-05-14（周三）

## 学习目标

1. 实现购物车功能
2. 实现订单创建与管理
3. 掌握状态管理

---

## 1. 购物车 API

### 1.1 接口定义

```typescript
// src/api/cart.ts
import request from './request'
import type { CartItem, CartItemParams } from './types'

// 获取购物车
export function getCart() {
    return request.get<CartItem[]>('/cart')
}

// 添加到购物车
export function addToCart(params: CartItemParams) {
    return request.post<CartItem>('/cart/items', params)
}

// 更新数量
export function updateCartItem(id: number, quantity: number) {
    return request.put<CartItem>(`/cart/items/${id}`, { quantity })
}

// 删除商品
export function removeCartItem(id: number) {
    return request.delete(`/cart/items/${id}`)
}

// 清空购物车
export function clearCart() {
    return request.delete('/cart/clear')
}
```

### 1.2 类型定义

```typescript
// src/api/types.ts 扩展

export interface CartItem {
    id: number
    productId: number
    productName: string
    productImage: string
    price: number
    quantity: number
    stock: number
    checked: boolean
}

export interface CartItemParams {
    productId: number
    quantity: number
}

export interface Cart {
    items: CartItem[]
    totalAmount: number
    totalCount: number
}
```

---

## 2. 购物车 Store

### 2.1 Pinia Store

```typescript
// src/stores/cart.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import { 
    getCart, 
    addToCart, 
    updateCartItem, 
    removeCartItem,
    clearCart 
} from '@/api/cart'
import type { CartItem, CartItemParams } from '@/api/types'

export const useCartStore = defineStore('cart', () => {
    // 状态
    const items = ref<CartItem[]>([])
    const loading = ref(false)

    // 计算属性
    const totalCount = computed(() => {
        return items.value.reduce((sum, item) => sum + item.quantity, 0)
    })

    const totalAmount = computed(() => {
        return items.value
            .filter(item => item.checked)
            .reduce((sum, item) => sum + item.price * item.quantity, 0)
    })

    const checkedCount = computed(() => {
        return items.value.filter(item => item.checked).length
    })

    // 获取购物车
    async function fetchCart() {
        loading.value = true
        try {
            items.value = await getCart()
        } finally {
            loading.value = false
        }
    }

    // 添加商品
    async function addItem(params: CartItemParams) {
        await addToCart(params)
        await fetchCart()
    }

    // 更新数量
    async function updateQuantity(id: number, quantity: number) {
        await updateCartItem(id, quantity)
        const item = items.value.find(i => i.id === id)
        if (item) {
            item.quantity = quantity
        }
    }

    // 删除商品
    async function removeItem(id: number) {
        await removeCartItem(id)
        items.value = items.value.filter(i => i.id !== id)
    }

    // 全选/取消全选
    function checkAll(checked: boolean) {
        items.value.forEach(item => {
            item.checked = checked
        })
    }

    // 切换选中
    function toggleCheck(id: number) {
        const item = items.value.find(i => i.id === id)
        if (item) {
            item.checked = !item.checked
        }
    }

    // 清空购物车
    async function clearCartItems() {
        await clearCart()
        items.value = []
    }

    return {
        items,
        loading,
        totalCount,
        totalAmount,
        checkedCount,
        fetchCart,
        addItem,
        updateQuantity,
        removeItem,
        checkAll,
        toggleCheck,
        clearCartItems
    }
})
```

---

## 3. 购物车页面

### 3.1 页面结构

```vue
<!-- src/views/cart/Cart.vue -->
<template>
    <div class="cart-page">
        <h1>购物车</h1>
        
        <!-- 购物车列表 -->
        <div class="cart-list" v-loading="cartStore.loading">
            <!-- 表头 -->
            <div class="cart-header">
                <el-checkbox 
                    :model-value="isAllChecked" 
                    @change="handleCheckAll"
                >
                    全选
                </el-checkbox>
                <span class="col-product">商品信息</span>
                <span class="col-price">单价</span>
                <span class="col-quantity">数量</span>
                <span class="col-subtotal">小计</span>
                <span class="col-action">操作</span>
            </div>
            
            <!-- 购物车项 -->
            <div v-if="cartStore.items.length > 0" class="cart-items">
                <div 
                    v-for="item in cartStore.items" 
                    :key="item.id"
                    class="cart-item"
                >
                    <el-checkbox 
                        :model-value="item.checked"
                        @change="cartStore.toggleCheck(item.id)"
                    />
                    
                    <div class="product-info" @click="goToProduct(item.productId)">
                        <img :src="item.productImage" class="product-image" />
                        <div class="product-detail">
                            <h4>{{ item.productName }}</h4>
                        </div>
                    </div>
                    
                    <div class="price">¥{{ item.price }}</div>
                    
                    <div class="quantity">
                        <el-input-number 
                            :model-value="item.quantity"
                            :min="1"
                            :max="item.stock"
                            @change="(val) => handleQuantityChange(item.id, val)"
                        />
                    </div>
                    
                    <div class="subtotal">
                        ¥{{ (item.price * item.quantity).toFixed(2) }}
                    </div>
                    
                    <div class="action">
                        <el-button 
                            type="danger" 
                            link
                            @click="handleRemove(item.id)"
                        >
                            删除
                        </el-button>
                    </div>
                </div>
            </div>
            
            <!-- 空状态 -->
            <el-empty v-else description="购物车是空的" />
        </div>
        
        <!-- 底部操作栏 -->
        <div class="cart-footer" v-if="cartStore.items.length > 0">
            <div class="footer-left">
                <el-checkbox 
                    :model-value="isAllChecked" 
                    @change="handleCheckAll"
                >
                    全选
                </el-checkbox>
                <el-button 
                    type="danger" 
                    link
                    @click="handleClear"
                >
                    清空购物车
                </el-button>
            </div>
            
            <div class="footer-right">
                <div class="total-info">
                    <span>已选 {{ cartStore.checkedCount }} 件商品，合计：</span>
                    <span class="total-amount">¥{{ cartStore.totalAmount.toFixed(2) }}</span>
                </div>
                <el-button 
                    type="primary" 
                    size="large"
                    :disabled="cartStore.checkedCount === 0"
                    @click="handleSettle"
                >
                    去结算
                </el-button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { ElMessage, ElMessageBox } from 'element-plus'
import { useCartStore } from '@/stores/cart'

const router = useRouter()
const cartStore = useCartStore()

// 是否全选
const isAllChecked = computed(() => {
    return cartStore.items.length > 0 && 
           cartStore.items.every(item => item.checked)
})

// 全选/取消全选
function handleCheckAll(checked: boolean) {
    cartStore.checkAll(checked)
}

// 修改数量
async function handleQuantityChange(id: number, quantity: number) {
    await cartStore.updateQuantity(id, quantity)
}

// 删除商品
async function handleRemove(id: number) {
    try {
        await ElMessageBox.confirm('确定要删除该商品吗？', '提示', {
            type: 'warning'
        })
        await cartStore.removeItem(id)
        ElMessage.success('删除成功')
    } catch {
        // 取消
    }
}

// 清空购物车
async function handleClear() {
    try {
        await ElMessageBox.confirm('确定要清空购物车吗？', '提示', {
            type: 'warning'
        })
        await cartStore.clearCartItems()
        ElMessage.success('已清空')
    } catch {
        // 取消
    }
}

// 去结算
function handleSettle() {
    router.push('/order/confirm')
}

// 跳转到商品详情
function goToProduct(id: number) {
    router.push(`/product/${id}`)
}

onMounted(() => {
    cartStore.fetchCart()
})
</script>

<style scoped lang="scss">
.cart-page {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.cart-header {
    display: flex;
    align-items: center;
    padding: 15px 20px;
    background: #f5f5f5;
    border-radius: 8px 8px 0 0;
    
    .col-product { flex: 1; margin-left: 15px; }
    .col-price { width: 120px; text-align: center; }
    .col-quantity { width: 150px; text-align: center; }
    .col-subtotal { width: 120px; text-align: center; }
    .col-action { width: 80px; text-align: center; }
}

.cart-item {
    display: flex;
    align-items: center;
    padding: 20px;
    border-bottom: 1px solid #eee;
    
    .product-info {
        flex: 1;
        display: flex;
        align-items: center;
        margin-left: 15px;
        cursor: pointer;
        
        .product-image {
            width: 80px;
            height: 80px;
            object-fit: cover;
            border-radius: 4px;
        }
        
        .product-detail {
            margin-left: 15px;
            
            h4 {
                margin: 0;
                font-size: 14px;
            }
        }
    }
    
    .price {
        width: 120px;
        text-align: center;
        font-weight: 500;
    }
    
    .quantity {
        width: 150px;
        text-align: center;
    }
    
    .subtotal {
        width: 120px;
        text-align: center;
        font-weight: 600;
        color: #ff4d4f;
    }
    
    .action {
        width: 80px;
        text-align: center;
    }
}

.cart-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    background: #fff;
    border-radius: 0 0 8px 8px;
    box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.05);
    
    .footer-left {
        display: flex;
        align-items: center;
        gap: 20px;
    }
    
    .footer-right {
        display: flex;
        align-items: center;
        gap: 20px;
        
        .total-info {
            .total-amount {
                font-size: 24px;
                font-weight: 600;
                color: #ff4d4f;
            }
        }
    }
}
</style>
```

---

## 4. 订单 API

### 4.1 接口定义

```typescript
// src/api/order.ts
import request from './request'
import type { Order, OrderCreateParams, OrderConfirmParams } from './types'

// 创建订单
export function createOrder(params: OrderCreateParams) {
    return request.post<Order>('/orders', params)
}

// 订单列表
export function getOrderList(params?: { status?: string; page?: number; pageSize?: number }) {
    return request.get<{ list: Order[]; total: number }>('/orders', { params })
}

// 订单详情
export function getOrderDetail(id: number) {
    return request.get<Order>(`/orders/${id}`)
}

// 取消订单
export function cancelOrder(id: number) {
    return request.put(`/orders/${id}/cancel`)
}

// 确认收货
export function confirmReceipt(id: number) {
    return request.put(`/orders/${id}/confirm`)
}

// 删除订单
export function deleteOrder(id: number) {
    return request.delete(`/orders/${id}`)
}
```

### 4.2 类型定义

```typescript
// src/api/types.ts 扩展

export type OrderStatus = 
    | 'pending'      // 待支付
    | 'paid'         // 已支付
    | 'shipping'     // 配送中
    | 'received'     // 已收货
    | 'completed'    // 已完成
    | 'cancelled'    // 已取消
    | 'refunding'   // 退款中
    | 'refunded'     // 已退款

export interface Order {
    id: number
    orderNumber: string
    userId: number
    totalAmount: number
    discountAmount: number
    actualAmount: number
    status: OrderStatus
    paymentMethod?: string
    paymentTime?: string
    receiverName: string
    receiverPhone: string
    receiverAddress: string
    remark?: string
    items: OrderItem[]
    createdAt: string
    updatedAt: string
}

export interface OrderItem {
    id: number
    productId: number
    productName: string
    productImage: string
    price: number
    quantity: number
    subtotal: number
}

export interface OrderCreateParams {
    addressId: number
    items: { productId: number; quantity: number }[]
    remark?: string
}

export interface OrderConfirmParams {
    orderId: number
    paymentMethod: string
}
```

---

## 5. 订单确认页

### 5.1 页面结构

```vue
<!-- src/views/order/OrderConfirm.vue -->
<template>
    <div class="order-confirm">
        <h1>确认订单</h1>
        
        <!-- 收货地址 -->
        <div class="section">
            <h3>收货地址</h3>
            <div class="address-list" v-if="addresses.length > 0">
                <div 
                    v-for="addr in addresses"
                    :key="addr.id"
                    :class="['address-card', { selected: selectedAddressId === addr.id }]"
                    @click="selectedAddressId = addr.id"
                >
                    <div class="receiver">{{ addr.receiverName }} {{ addr.phone }}</div>
                    <div class="address">{{ addr.province }} {{ addr.city }} {{ addr.district }} {{ addr.detailAddress }}</div>
                    <div v-if="addr.isDefault" class="default-tag">默认</div>
                </div>
            </div>
            <el-empty v-else description="暂无收货地址">
                <el-button type="primary" @click="goToAddress">添加地址</el-button>
            </el-empty>
        </div>
        
        <!-- 商品列表 -->
        <div class="section">
            <h3>商品清单</h3>
            <el-table :data="orderItems">
                <el-table-column label="商品信息">
                    <template #default="{ row }">
                        <div class="product-cell">
                            <img :src="row.productImage" class="product-image" />
                            <span>{{ row.productName }}</span>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column label="单价" width="150">
                    <template #default="{ row }">
                        ¥{{ row.price }}
                    </template>
                </el-table-column>
                <el-table-column prop="quantity" label="数量" width="100" />
                <el-table-column label="小计" width="150">
                    <template #default="{ row }">
                        <span class="subtotal">¥{{ (row.price * row.quantity).toFixed(2) }}</span>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        
        <!-- 备注 -->
        <div class="section">
            <h3>订单备注</h3>
            <el-input
                v-model="remark"
                type="textarea"
                :rows="3"
                placeholder="请输入备注信息"
            />
        </div>
        
        <!-- 结算 -->
        <div class="settlement">
            <div class="settlement-info">
                <div class="info-row">
                    <span>商品总价：</span>
                    <span>¥{{ totalAmount.toFixed(2) }}</span>
                </div>
                <div class="info-row">
                    <span>运费：</span>
                    <span>¥{{ shippingFee.toFixed(2) }}</span>
                </div>
                <div class="info-row total">
                    <span>应付总额：</span>
                    <span class="total-amount">¥{{ finalAmount.toFixed(2) }}</span>
                </div>
            </div>
            
            <el-button 
                type="primary" 
                size="large"
                :disabled="!selectedAddressId || orderItems.length === 0"
                @click="handleSubmit"
            >
                提交订单
            </el-button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'
import { createOrder } from '@/api/order'
import { getCart } from '@/api/cart'
import type { Address } from '@/api/types'

const route = useRoute()
const router = useRouter()

const addresses = ref<Address[]>([])
const selectedAddressId = ref<number | null>(null)
const remark = ref('')
const orderItems = ref<any[]>([])

// 计算金额
const totalAmount = computed(() => {
    return orderItems.value.reduce((sum, item) => sum + item.price * item.quantity, 0)
})

const shippingFee = computed(() => {
    return totalAmount.value >= 99 ? 0 : 10 // 满99免运费
})

const finalAmount = computed(() => {
    return totalAmount.value + shippingFee.value
})

// 提交订单
async function handleSubmit() {
    if (!selectedAddressId.value) {
        ElMessage.warning('请选择收货地址')
        return
    }
    
    try {
        const order = await createOrder({
            addressId: selectedAddressId.value,
            items: orderItems.value.map(item => ({
                productId: item.productId,
                quantity: item.quantity
            })),
            remark: remark.value
        })
        
        ElMessage.success('订单创建成功')
        router.push(`/order/payment/${order.id}`)
    } catch (e) {
        ElMessage.error('订单创建失败')
    }
}

// 跳转添加地址
function goToAddress() {
    router.push('/user/address')
}

onMounted(async () => {
    // 获取地址列表
    // addresses.value = await getAddresses()
    
    // 获取商品（从购物车或直接购买）
    const productId = route.query.productId
    const quantity = route.query.quantity
    
    if (productId) {
        // 直接购买
        // orderItems.value = await getProductDetail(productId)
    } else {
        // 从购物车
        orderItems.value = await getCart()
    }
})
</script>

<style scoped lang="scss">
.order-confirm {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.section {
    margin-bottom: 30px;
    
    h3 {
        margin-bottom: 15px;
    }
}

.address-list {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
}

.address-card {
    width: 280px;
    padding: 15px;
    border: 2px solid #eee;
    border-radius: 8px;
    cursor: pointer;
    position: relative;
    
    &:hover {
        border-color: #409eff;
    }
    
    &.selected {
        border-color: #409eff;
        background: #f0f9ff;
    }
    
    .receiver {
        font-weight: 600;
        margin-bottom: 8px;
    }
    
    .address {
        color: #666;
        font-size: 14px;
    }
    
    .default-tag {
        position: absolute;
        top: 10px;
        right: 10px;
        font-size: 12px;
        color: #409eff;
    }
}

.product-cell {
    display: flex;
    align-items: center;
    gap: 10px;
    
    .product-image {
        width: 60px;
        height: 60px;
        object-fit: cover;
        border-radius: 4px;
    }
}

.subtotal {
    font-weight: 600;
    color: #ff4d4f;
}

.settlement {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 30px;
    padding: 20px;
    background: #f5f5f5;
    border-radius: 8px;
    
    .settlement-info {
        text-align: right;
        
        .info-row {
            margin-bottom: 10px;
            
            &.total {
                font-size: 18px;
                font-weight: 600;
                
                .total-amount {
                    font-size: 24px;
                    color: #ff4d4f;
                }
            }
        }
    }
}
</style>
```

---

## 6. 订单列表页

### 6.1 页面结构

```vue
<!-- src/views/order/OrderList.vue -->
<template>
    <div class="order-list">
        <h1>我的订单</h1>
        
        <!-- 标签页 -->
        <el-tabs v-model="activeTab" @tab-change="handleTabChange">
            <el-tab-pane label="全部" name="all" />
            <el-tab-pane label="待支付" name="pending" />
            <el-tab-pane label="待收货" name="shipping" />
            <el-tab-pane label="已完成" name="completed" />
        </el-tabs>
        
        <!-- 订单列表 -->
        <div class="orders" v-loading="loading">
            <div v-for="order in orders" :key="order.id" class="order-card">
                <!-- 订单头部 -->
                <div class="order-header">
                    <span class="order-number">订单号：{{ order.orderNumber }}</span>
                    <span class="order-time">{{ order.createdAt }}</span>
                    <span :class="['order-status', order.status]">
                        {{ statusText(order.status) }}
                    </span>
                </div>
                
                <!-- 订单商品 -->
                <div class="order-items">
                    <div v-for="item in order.items" :key="item.id" class="order-item">
                        <img :src="item.productImage" class="product-image" />
                        <div class="product-info">
                            <span>{{ item.productName }}</span>
                            <span class="price">¥{{ item.price }} × {{ item.quantity }}</span>
                        </div>
                    </div>
                </div>
                
                <!-- 订单底部 -->
                <div class="order-footer">
                    <div class="total">
                        实付金额：<span class="amount">¥{{ order.actualAmount }}</span>
                    </div>
                    <div class="actions">
                        <el-button 
                            v-if="order.status === 'pending'"
                            type="primary"
                            @click="handlePay(order.id)"
                        >
                            去支付
                        </el-button>
                        <el-button 
                            v-if="order.status === 'shipping'"
                            @click="handleConfirmReceipt(order.id)"
                        >
                            确认收货
                        </el-button>
                        <el-button @click="goToDetail(order.id)">查看详情</el-button>
                    </div>
                </div>
            </div>
            
            <el-empty v-if="!loading && orders.length === 0" description="暂无订单" />
        </div>
        
        <!-- 分页 -->
        <div class="pagination" v-if="total > 0">
            <el-pagination
                v-model:current-page="pagination.page"
                v-model:page-size="pagination.pageSize"
                :total="total"
                layout="total, prev, pager, next"
                @current-change="fetchOrders"
            />
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'
import { getOrderList, cancelOrder, confirmReceipt } from '@/api/order'

const router = useRouter()

const activeTab = ref('all')
const loading = ref(false)
const orders = ref<any[]>([])
const pagination = reactive({
    page: 1,
    pageSize: 10
})
const total = ref(0)

// 状态文本
function statusText(status: string) {
    const map: Record<string, string> = {
        pending: '待支付',
        paid: '已支付',
        shipping: '配送中',
        received: '已收货',
        completed: '已完成',
        cancelled: '已取消',
        refunding: '退款中',
        refunded: '已退款'
    }
    return map[status] || status
}

// 获取订单列表
async function fetchOrders() {
    loading.value = true
    try {
        const params: any = {
            page: pagination.page,
            pageSize: pagination.pageSize
        }
        if (activeTab.value !== 'all') {
            params.status = activeTab.value
        }
        
        const data = await getOrderList(params)
        orders.value = data.list
        total.value = data.total
    } finally {
        loading.value = false
    }
}

// 切换标签
function handleTabChange() {
    pagination.page = 1
    fetchOrders()
}

// 去支付
function handlePay(orderId: number) {
    router.push(`/order/payment/${orderId}`)
}

// 确认收货
async function handleConfirmReceipt(orderId: number) {
    await confirmReceipt(orderId)
    ElMessage.success('确认收货成功')
    fetchOrders()
}

// 查看详情
function goToDetail(orderId: number) {
    router.push(`/order/${orderId}`)
}

onMounted(() => {
    fetchOrders()
})
</script>

<style scoped lang="scss">
.order-list {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.order-card {
    background: #fff;
    border-radius: 8px;
    margin-bottom: 20px;
    overflow: hidden;
}

.order-header {
    display: flex;
    align-items: center;
    padding: 15px 20px;
    background: #f5f5f5;
    
    .order-number { font-weight: 500; }
    .order-time { margin-left: 20px; color: #999; }
    .order-status { margin-left: auto; color: #409eff; }
}

.order-items {
    padding: 20px;
}

.order-item {
    display: flex;
    align-items: center;
    padding: 10px 0;
    
    .product-image {
        width: 80px;
        height: 80px;
        object-fit: cover;
        border-radius: 4px;
    }
    
    .product-info {
        flex: 1;
        display: flex;
        justify-content: space-between;
        margin-left: 15px;
        
        .price { color: #666; }
    }
}

.order-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 20px;
    border-top: 1px solid #eee;
    
    .total .amount {
        font-size: 20px;
        font-weight: 600;
        color: #ff4d4f;
    }
}

.pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}
</style>
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：购物车 ==========
// 1. 实现商品数量修改
// 2. 实现商品删除

// ========== 练习 2：订单 ==========
// 1. 实现订单创建
// 2. 实现订单列表

// ========== 练习 3：状态管理 ==========
// 1. 使用 Pinia 管理购物车状态
// 2. 实现持久化
```

---

## 🎯 今日自检清单

- [ ] 购物车 API 封装
- [ ] 购物车 Store
- [ ] 购物车页面
- [ ] 订单 API
- [ ] 订单确认页
- [ ] 订单列表页

---

## 下一步

明天我们将学习：**后端 API 开发**
