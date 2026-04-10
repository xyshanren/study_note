> 学习日期：2026-05-06（周二）

## 学习目标

1. 掌握 ArkTS 基本语法
2. 理解状态管理装饰器
3. 能够创建响应式界面

---

## 1. 基础语法

### 1.1 数据类型

```typescript
// 基础类型
let name: string = '张三';
let age: number = 25;
let isStudent: boolean = true;

// 数组
let numbers: number[] = [1, 2, 3, 4, 5];
let names: Array<string> = ['Alice', 'Bob'];

// 元组
let person: [string, number] = ['Alice', 30];

// 枚举
enum Color {
    Red = 'red',
    Green = 'green',
    Blue = 'blue'
}
let favoriteColor: Color = Color.Blue;

// 对象
let user: { name: string; age: number } = {
    name: '张三',
    age: 25
};
```

### 1.2 函数

```typescript
// 函数声明
function greet(name: string): string {
    return `你好, ${name}!`;
}

// 箭头函数
const add = (a: number, b: number): number => {
    return a + b;
};

// 可选参数
function createUser(name: string, age?: number): string {
    if (age !== undefined) {
        return `${name}, ${age}岁`;
    }
    return name;
}

// 默认参数
function greet(name: string, greeting: string = '你好'): string {
    return `${greeting}, ${name}!`;
}

// 剩余参数
function sum(...numbers: number[]): number {
    return numbers.reduce((a, b) => a + b, 0);
}
```

### 1.3 类

```typescript
class Person {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    
    greet(): string {
        return `你好, 我是${this.name}`;
    }
}

const person = new Person('张三', 25);
console.log(person.greet());
```

---

## 2. 状态管理

### 2.1 @State - 组件状态

```typescript
@Entry
@Component
struct Counter {
    @State count: number = 0;
    
    build() {
        Column() {
            Text(`计数: ${this.count}`)
                .fontSize(30)
            
            Button('增加')
                .onClick(() => {
                    this.count++;  // 修改状态，UI 自动更新
                })
        }
    }
}
```

### 2.2 @Prop - 父子传值

```typescript
// 子组件
@Component
struct ChildComponent {
    @Prop title: string;
    @Prop count: number = 0;
    
    build() {
        Column() {
            Text(this.title)
            Text(`Count: ${this.count}`)
        }
    }
}

// 父组件
@Entry
@Component
struct Parent {
    @State parentTitle: string = '父组件标题';
    @State parentCount: number = 10;
    
    build() {
        ChildComponent({
            title: this.parentTitle,
            count: this.parentCount
        })
    }
}
```

### 2.3 @Link - 双向绑定

```typescript
// 子组件
@Component
struct ChildComponent {
    @Link count: number;
    
    build() {
        Column() {
            Text(`子组件: ${this.count}`)
            Button('增加')
                .onClick(() => {
                    this.count++;
                })
        }
    }
}

// 父组件
@Entry
@Component
struct Parent {
    @State parentCount: number = 0;
    
    build() {
        Column() {
            Text(`父组件: ${this.parentCount}`)
            ChildComponent({ count: $parentCount })  // 使用 $ 传递引用
        }
    }
}
```

### 2.4 @Provide / @Consume - 跨层级传值

```typescript
// 祖先组件
@Entry
@Component
struct GrandParent {
    @Provide theme: string = 'dark';
    
    build() {
        Column() {
            Text(`主题: ${this.theme}`)
            Child()
        }
    }
}

// 后代组件
@Component
struct Child {
    @Consume theme: string;
    
    build() {
        Text(`使用主题: ${this.theme}`)
    }
}
```

### 2.5 @Observed / @ObjectLink - 对象观测

```typescript
@Observed
class User {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

@Component
struct ChildComponent {
    @ObjectLink user: User;
    
    build() {
        Column() {
            Text(`Name: ${this.user.name}`)
            Text(`Age: ${this.user.age}`)
            Button('修改年龄')
                .onClick(() => {
                    this.user.age++;  // 对象属性变化可被观测
                })
        }
    }
}

@Entry
@Component
struct Parent {
    @State user: User = new User('张三', 25);
    
    build() {
        ChildComponent({ user: this.user })
    }
}
```

---

## 3. 条件渲染

### 3.1 if / else

```typescript
@Entry
@Component
struct ConditionalRender {
    @State showContent: boolean = true;
    
    build() {
        Column() {
            if (this.showContent) {
                Text('显示内容')
                    .fontSize(20)
            } else {
                Text('隐藏内容')
                    .fontSize(20)
            }
            
            Button('切换')
                .onClick(() => {
                    this.showContent = !this.showContent;
                })
        }
    }
}
```

### 3.2 switch

```typescript
@Entry
@Component
struct SwitchExample {
    @State status: string = 'loading';
    
    build() {
        Column() {
            switch (this.status) {
                case 'success':
                    Text('成功');
                    break;
                case 'error':
                    Text('错误');
                    break;
                case 'loading':
                    Text('加载中...');
                    break;
                default:
                    Text('未知状态');
            }
        }
    }
}
```

---

## 4. 循环渲染

### 4.1 ForEach

```typescript
@Entry
@Component
struct ListExample {
    @State fruits: string[] = ['苹果', '香蕉', '橙子', '葡萄'];
    
    build() {
        Column() {
            ForEach(this.fruits, (item: string, index: number) => {
                Text(`${index + 1}. ${item}`)
                    .fontSize(20)
                    .margin(10)
            })
        }
    }
}
```

### 4.2 复杂列表

```typescript
interface Item {
    id: number;
    title: string;
    subtitle: string;
}

@Entry
@Component
struct ComplexList {
    @State items: Item[] = [
        { id: 1, title: '标题1', subtitle: '描述1' },
        { id: 2, title: '标题2', subtitle: '描述2' },
        { id: 3, title: '标题3', subtitle: '描述3' }
    ];
    
    build() {
        Column() {
            ForEach(this.items, (item: Item) => {
                Row() {
                    Column() {
                        Text(item.title)
                            .fontSize(18)
                            .fontWeight(FontWeight.Bold);
                        Text(item.subtitle)
                            .fontSize(14)
                            .fontColor('#666666');
                    }
                    .alignItems(HorizontalAlign.Start);
                }
                .padding(10)
                .borderWidth(1)
                .borderColor('#eeeeee')
                .margin(5);
            })
        }
    }
}
```

---

## 5. 样式

### 5.1 基础样式

```typescript
@Entry
@Component
struct StyleExample {
    build() {
        Column() {
            Text('Hello')
                .fontSize(24)
                .fontColor('#333333')
                .fontWeight(FontWeight.Bold)
                .textAlign(TextAlign.Center)
                .margin({ top: 20, bottom: 20 })
                .padding(10)
                .backgroundColor('#f5f5f5')
                .borderRadius(10)
        }
        .width('100%')
        .padding(20);
    }
}
```

### 5.2 通用样式属性

```typescript
Text('样式示例')
    .width(200)           // 宽度
    .height(100)          // 高度
    .padding(10)          // 内边距
    .margin(10)           // 外边距
    .backgroundColor('#fff')  // 背景色
    .border({             // 边框
        width: 1,
        color: '#999',
        radius: 10,
        style: BorderStyle.Solid
    })
    .opacity(0.8)         // 透明度
    .visibility(Visibility.Visible)  // 可见性
    .enabled(true)        // 是否可交互
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：基础语法 ==========
// 1. 创建函数实现计算器
// 2. 使用类实现用户管理

// ========== 练习 2：状态管理 ==========
// 1. 实现计数器功能
// 2. 实现父子组件数据双向绑定

// ========== 练习 3：列表渲染 ==========
// 1. 渲染水果列表
// 2. 实现带图片的商品列表
```

---

## 🎯 今日自检清单

- [ ] ArkTS 数据类型
- [ ] 函数与类
- [ ] @State 状态管理
- [ ] @Prop/@Link/@Provide/@Consume
- [ ] 条件与循环渲染
- [ ] 样式

---

## 下一步

明天我们将学习：**ArkTS 组件与布局**
