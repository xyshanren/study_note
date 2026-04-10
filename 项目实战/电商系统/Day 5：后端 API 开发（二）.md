> 学习日期：2026-05-16（周五）

## 学习目标

1. 实现购物车 API
2. 实现订单 API
3. 实现支付集成

---

## 1. 订单模型

### 1.1 订单表

```python
# app/models/order.py
from sqlalchemy import Column, Integer, String, Numeric, Text, DateTime, ForeignKey
from sqlalchemy.orm import relationship
from datetime import datetime
from app.database import Base

class Order(Base):
    __tablename__ = "orders"
    
    id = Column(Integer, primary_key=True, index=True)
    order_number = Column(String(50), unique=True, nullable=False, index=True)
    user_id = Column(Integer, ForeignKey("users.id"), nullable=False, index=True)
    total_amount = Column(Numeric(10, 2), nullable=False)
    discount_amount = Column(Numeric(10, 2), default=0)
    actual_amount = Column(Numeric(10, 2), nullable=False)
    status = Column(String(20), default="pending", index=True)
    payment_method = Column(String(20))
    payment_time = Column(DateTime)
    receiver_name = Column(String(50))
    receiver_phone = Column(String(20))
    receiver_address = Column(String(255))
    remark = Column(Text)
    created_at = Column(DateTime, default=datetime.utcnow, index=True)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
    
    # 关系
    user = relationship("User", back_populates="orders")
    items = relationship("OrderItem", back_populates="order", cascade="all, delete-orphan")


class OrderItem(Base):
    __tablename__ = "order_items"
    
    id = Column(Integer, primary_key=True, index=True)
    order_id = Column(Integer, ForeignKey("orders.id"), nullable=False, index=True)
    product_id = Column(Integer, ForeignKey("products.id"), nullable=False)
    product_name = Column(String(200), nullable=False)
    product_image = Column(String(255))
    price = Column(Numeric(10, 2), nullable=False)
    quantity = Column(Integer, nullable=False)
    subtotal = Column(Numeric(10, 2), nullable=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # 关系
    order = relationship("Order", back_populates="items")
    product = relationship("Product", back_populates="order_items")
```

---

## 2. 订单 Schema

### 2.1 定义

```python
# app/schemas/order.py
from pydantic import BaseModel, Field
from typing import List, Optional
from datetime import datetime

class OrderItemCreate(BaseModel):
    product_id: int
    quantity: int = Field(..., gt=0)


class OrderCreate(BaseModel):
    address_id: int
    items: List[OrderItemCreate]
    remark: Optional[str] = None


class OrderItemResponse(BaseModel):
    id: int
    product_id: int
    product_name: str
    product_image: Optional[str] = None
    price: float
    quantity: int
    subtotal: float
    
    class Config:
        from_attributes = True


class OrderResponse(BaseModel):
    id: int
    order_number: str
    user_id: int
    total_amount: float
    discount_amount: float
    actual_amount: float
    status: str
    payment_method: Optional[str] = None
    payment_time: Optional[datetime] = None
    receiver_name: str
    receiver_phone: str
    receiver_address: str
    remark: Optional[str] = None
    items: List[OrderItemResponse] = []
    created_at: datetime
    
    class Config:
        from_attributes = True


class CartItemResponse(BaseModel):
    id: int
    product_id: int
    product_name: str
    product_image: Optional[str] = None
    price: float
    quantity: int
    stock: int
    
    class Config:
        from_attributes = True
```

---

## 3. 购物车 API

### 3.1 API 实现

```python
# app/api/cart.py
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session
from typing import List
from app.database import get_db
from app.models.user import User, Address
from app.models.product import Product
from app.models.cart import CartItem
from app.schemas.order import CartItemResponse
from app.api.deps import get_current_user

router = APIRouter(prefix="/api/cart", tags=["购物车"])


# 获取购物车
@router.get("", response_model=List[CartItemResponse])
def get_cart(
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    items = db.query(CartItem).filter(
        CartItem.user_id == current_user.id
    ).all()
    
    # 填充商品信息
    result = []
    for item in items:
        product = db.query(Product).filter(Product.id == item.product_id).first()
        if product and product.status == "active":
            result.append(item)
    
    return result


# 添加到购物车
@router.post("/items", status_code=status.HTTP_201_CREATED)
def add_to_cart(
    product_id: int,
    quantity: int = 1,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    # 检查商品
    product = db.query(Product).filter(Product.id == product_id).first()
    if not product:
        raise HTTPException(status_code=404, detail="商品不存在")
    
    if product.status != "active":
        raise HTTPException(status_code=400, detail="商品已下架")
    
    if product.stock < quantity:
        raise HTTPException(status_code=400, detail="库存不足")
    
    # 检查购物车是否已有
    cart_item = db.query(CartItem).filter(
        CartItem.user_id == current_user.id,
        CartItem.product_id == product_id
    ).first()
    
    if cart_item:
        # 更新数量
        new_quantity = cart_item.quantity + quantity
        if product.stock < new_quantity:
            raise HTTPException(status_code=400, detail="库存不足")
        cart_item.quantity = new_quantity
    else:
        # 创建新条目
        cart_item = CartItem(
            user_id=current_user.id,
            product_id=product_id,
            quantity=quantity
        )
        db.add(cart_item)
    
    db.commit()
    return {"message": "添加成功"}


# 更新购物车数量
@router.put("/items/{item_id}")
def update_cart_item(
    item_id: int,
    quantity: int,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    cart_item = db.query(CartItem).filter(
        CartItem.id == item_id,
        CartItem.user_id == current_user.id
    ).first()
    
    if not cart_item:
        raise HTTPException(status_code=404, detail="购物车项不存在")
    
    # 检查库存
    product = db.query(Product).filter(Product.id == cart_item.product_id).first()
    if product.stock < quantity:
        raise HTTPException(status_code=400, detail="库存不足")
    
    cart_item.quantity = quantity
    db.commit()
    
    return {"message": "更新成功"}


# 删除购物车项
@router.delete("/items/{item_id}")
def remove_cart_item(
    item_id: int,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    cart_item = db.query(CartItem).filter(
        CartItem.id == item_id,
        CartItem.user_id == current_user.id
    ).first()
    
    if not cart_item:
        raise HTTPException(status_code=404, detail="购物车项不存在")
    
    db.delete(cart_item)
    db.commit()
    
    return {"message": "删除成功"}


# 清空购物车
@router.delete("/clear")
def clear_cart(
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    db.query(CartItem).filter(
        CartItem.user_id == current_user.id
    ).delete()
    db.commit()
    
    return {"message": "清空成功"}
```

---

## 4. 订单 API

### 4.1 创建订单

```python
# app/api/orders.py
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session
from typing import List
import uuid
from datetime import datetime
from app.database import get_db
from app.models.user import User, Address
from app.models.product import Product
from app.models.cart import CartItem
from app.models.order import Order, OrderItem
from app.schemas.order import OrderCreate, OrderResponse
from app.api.deps import get_current_user

router = APIRouter(prefix="/api/orders", tags=["订单"])


def generate_order_number():
    """生成订单号"""
    import random
    return f"ORD{datetime.now().strftime('%Y%m%d%H%M%S')}{random.randint(1000, 9999)}"


# 创建订单
@router.post("", response_model=OrderResponse, status_code=status.HTTP_201_CREATED)
def create_order(
    order_data: OrderCreate,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    # 获取地址
    address = db.query(Address).filter(
        Address.id == order_data.address_id,
        Address.user_id == current_user.id
    ).first()
    
    if not address:
        raise HTTPException(status_code=400, detail="收货地址不存在")
    
    # 获取商品并计算价格
    order_items = []
    total_amount = 0
    
    for item in order_data.items:
        product = db.query(Product).filter(Product.id == item.product_id).first()
        
        if not product:
            raise HTTPException(status_code=400, detail=f"商品 {item.product_id} 不存在")
        
        if product.status != "active":
            raise HTTPException(status_code=400, detail=f"商品 {product.name} 已下架")
        
        if product.stock < item.quantity:
            raise HTTPException(status_code=400, detail=f"商品 {product.name} 库存不足")
        
        subtotal = float(product.price) * item.quantity
        total_amount += subtotal
        
        order_items.append({
            "product_id": product.id,
            "product_name": product.name,
            "product_image": product.images[0] if product.images else None,
            "price": product.price,
            "quantity": item.quantity,
            "subtotal": subtotal
        })
    
    # 创建订单
    order = Order(
        order_number=generate_order_number(),
        user_id=current_user.id,
        total_amount=total_amount,
        discount_amount=0,
        actual_amount=total_amount,
        status="pending",
        receiver_name=address.receiver_name,
        receiver_phone=address.phone,
        receiver_address=f"{address.province} {address.city} {address.district} {address.detail_address}",
        remark=order_data.remark
    )
    db.add(order)
    db.flush()  # 获取 order.id
    
    # 创建订单项
    for item in order_items:
        order_item = OrderItem(order_id=order.id, **item)
        db.add(order_item)
        
        # 扣减库存
        product = db.query(Product).filter(Product.id == item["product_id"]).first()
        product.stock -= item["quantity"]
        product.sales_count += item["quantity"]
    
    # 清空购物车
    for item in order_data.items:
        db.query(CartItem).filter(
            CartItem.user_id == current_user.id,
            CartItem.product_id == item.product_id
        ).delete()
    
    db.commit()
    db.refresh(order)
    
    return order


# 订单列表
@router.get("")
def get_orders(
    status: str = None,
    page: int = 1,
    page_size: int = 10,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    query = db.query(Order).filter(Order.user_id == current_user.id)
    
    if status:
        query = query.filter(Order.status == status)
    
    total = query.count()
    orders = query.order_by(Order.created_at.desc()).offset((page - 1) * page_size).limit(page_size).all()
    
    return {
        "list": orders,
        "total": total,
        "page": page,
        "page_size": page_size
    }


# 订单详情
@router.get("/{order_id}", response_model=OrderResponse)
def get_order(
    order_id: int,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    order = db.query(Order).filter(
        Order.id == order_id,
        Order.user_id == current_user.id
    ).first()
    
    if not order:
        raise HTTPException(status_code=404, detail="订单不存在")
    
    return order


# 取消订单
@router.put("/{order_id}/cancel")
def cancel_order(
    order_id: int,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    order = db.query(Order).filter(
        Order.id == order_id,
        Order.user_id == current_user.id
    ).first()
    
    if not order:
        raise HTTPException(status_code=404, detail="订单不存在")
    
    if order.status not in ["pending"]:
        raise HTTPException(status_code=400, detail="该订单无法取消")
    
    # 恢复库存
    for item in order.items:
        product = db.query(Product).filter(Product.id == item.product_id).first()
        if product:
            product.stock += item.quantity
            product.sales_count -= item.quantity
    
    order.status = "cancelled"
    db.commit()
    
    return {"message": "取消成功"}


# 确认收货
@router.put("/{order_id}/confirm")
def confirm_receipt(
    order_id: int,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    order = db.query(Order).filter(
        Order.id == order_id,
        Order.user_id == current_user.id
    ).first()
    
    if not order:
        raise HTTPException(status_code=404, detail="订单不存在")
    
    if order.status != "shipping":
        raise HTTPException(status_code=400, detail="该订单无法确认收货")
    
    order.status = "completed"
    db.commit()
    
    return {"message": "确认收货成功"}
```

---

## 5. 支付集成

### 5.1 支付服务

```python
# app/services/payment.py
from abc import ABC, abstractmethod
from datetime import datetime
from typing import Optional

class PaymentService(ABC):
    @abstractmethod
    def create_payment(self, order_number: str, amount: float) -> dict:
        pass
    
    @abstractmethod
    def query_payment(self, payment_id: str) -> dict:
        pass
    
    @abstractmethod
    def handle_callback(self, callback_data: dict) -> dict:
        pass


class AlipayService(PaymentService):
    def __init__(self):
        self.app_id = "your_app_id"
        self.private_key = "your_private_key"
        self.alipay_public_key = "alipay_public_key"
        self.gateway = "https://openapi.alipay.com/gateway.do"
    
    def create_payment(self, order_number: str, amount: float) -> dict:
        # 构建支付参数
        biz_content = {
            "out_trade_no": order_number,
            "total_amount": str(amount),
            "subject": f"订单 {order_number}",
            "product_code": "FAST_INSTANT_TRADE_PAY"
        }
        
        # TODO: 实际调用支付宝 API
        # 返回支付信息（二维码链接等）
        return {
            "payment_id": f"PAY{order_number}",
            "qr_code": f"https://qr.alipay.com/{order_number}",
            "status": "pending"
        }
    
    def query_payment(self, payment_id: str) -> dict:
        # TODO: 查询支付状态
        return {"status": "paid", "paid_at": datetime.now()}
    
    def handle_callback(self, callback_data: dict) -> dict:
        # TODO: 处理支付宝回调
        return {"success": True}


class WechatPayService(PaymentService):
    def __init__(self):
        self.mch_id = "your_mch_id"
        self.api_key = "your_api_key"
    
    def create_payment(self, order_number: str, amount: float) -> dict:
        # TODO: 调用微信支付 API
        return {
            "payment_id": f"PAY{order_number}",
            "code_url": f"weixin://wxpay/bizpayurl?pr={order_number}",
            "status": "pending"
        }
    
    def query_payment(self, payment_id: str) -> dict:
        return {"status": "paid", "paid_at": datetime.now()}
    
    def handle_callback(self, callback_data: dict) -> dict:
        return {"success": True}


# 支付工厂
class PaymentFactory:
    @staticmethod
    def get_payment_service(payment_method: str) -> PaymentService:
        services = {
            "alipay": AlipayService(),
            "wechat": WechatPayService()
        }
        
        if payment_method not in services:
            raise ValueError(f"不支持的支付方式: {payment_method}")
        
        return services[payment_method]
```

### 5.2 支付 API

```python
# app/api/payment.py
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from pydantic import BaseModel
from app.database import get_db
from app.models.user import User
from app.models.order import Order
from app.services.payment import PaymentFactory
from app.api.deps import get_current_user

router = APIRouter(prefix="/api/payment", tags=["支付"])


class PaymentRequest(BaseModel):
    order_id: int
    payment_method: str


class PaymentResponse(BaseModel):
    payment_id: str
    qr_code: str
    status: str


# 创建支付
@router.post("/create", response_model=PaymentResponse)
def create_payment(
    request: PaymentRequest,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    # 获取订单
    order = db.query(Order).filter(
        Order.id == request.order_id,
        Order.user_id == current_user.id
    ).first()
    
    if not order:
        raise HTTPException(status_code=404, detail="订单不存在")
    
    if order.status != "pending":
        raise HTTPException(status_code=400, detail="订单状态不正确")
    
    # 创建支付
    try:
        payment_service = PaymentFactory.get_payment_service(request.payment_method)
        payment_result = payment_service.create_payment(
            order.order_number,
            float(order.actual_amount)
        )
        
        # 更新订单支付方式
        order.payment_method = request.payment_method
        db.commit()
        
        return payment_result
    except ValueError as e:
        raise HTTPException(status_code=400, detail=str(e))


# 支付回调（由支付平台调用）
@router.post("/callback/{payment_method}")
def payment_callback(
    payment_method: str,
    callback_data: dict,
    db: Session = Depends(get_db)
):
    try:
        payment_service = PaymentFactory.get_payment_service(payment_method)
        result = payment_service.handle_callback(callback_data)
        
        if result.get("success"):
            # 更新订单状态
            order_number = callback_data.get("out_trade_no")
            order = db.query(Order).filter(Order.order_number == order_number).first()
            
            if order:
                order.status = "paid"
                order.payment_time = datetime.now()
                db.commit()
        
        return {"success": True}
    except Exception as e:
        return {"success": False, "error": str(e)}
```

---

## 📝 课后练习

```python
# ========== 练习 1：完善购物车 ==========
# 1. 实现购物车商品选择
# 2. 计算运费

# ========== 练习 2：完善订单 ==========
# 1. 实现订单列表筛选
# 2. 实现订单状态跟踪

# ========== 练习 3：支付集成 ==========
# 1. 实现支付宝支付
# 2. 实现支付回调
```

---

## 🎯 今日自检清单

- [ ] 订单模型
- [ ] 购物车 API
- [ ] 创建订单 API
- [ ] 订单列表 API
- [ ] 支付集成
- [ ] 支付回调

---

## 电商系统项目总结

这周我们完成了电商系统的核心功能：

1. ✅ Day 1: 项目规划与初始化
2. ✅ Day 2: 商品模块开发
3. ✅ Day 3: 购物车与订单模块
4. ✅ Day 4: 后端 API 开发（一）
5. ✅ Day 5: 后端 API 开发（二）

---

**📊 全栈学习进度：**

| 阶段 | 内容 | 状态 |
|------|------|------|
| TypeScript | 8天 | ✅ |
| Vue 3 | 7天 | ✅ |
| FastAPI | 5天 | ✅ |
| 鸿蒙 ArkTS | 5天 | ✅ |
| 电商项目 | 5天 | ✅ |
| Docker | 待完成 | ⏳ |

接下来是 **Docker 部署** 和 **CI/CD**，要继续吗？
