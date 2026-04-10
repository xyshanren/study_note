> 学习日期：2026-05-07（周三）

## 学习目标

1. 掌握常用基础组件
2. 理解布局系统（Flex、Grid）
3. 能够创建复杂界面

---

## 1. 基础组件

### 1.1 Text 文本

```typescript
@Entry
@Component
struct TextExample {
    build() {
        Column({ space: 10 }) {
            // 普通文本
            Text('普通文本')
            
            // 带样式
            Text('大号加粗')
                .fontSize(24)
                .fontWeight(FontWeight.Bold)
            
            // 带Span的多样式文本
            Text() {
                Span('红色')
                    .fontColor('#ff0000')
                Span(' 和 ')
                Span('蓝色')
                    .fontColor('#0000ff')
            }
            
            // 渐变色
            Text('渐变文字')
                .fontSize(30)
                .fontWeight(FontWeight.Bold)
                .foregroundGradient({
                    colors: ['#ff0000', '#00ff00', '#0000ff']
                })
        }
        .padding(20);
    }
}
```

### 1.2 Button 按钮

```typescript
@Entry
@Component
struct ButtonExample {
    build() {
        Column({ space: 20 }) {
            // 普通按钮
            Button('普通按钮')
                .onClick(() => {
                    console.log('点击了');
                })
            
            // 图标按钮
            Button({ type: ButtonType.Circle, stateEffect: true }) {
                Text('+')
                    .fontSize(30)
            }
            .width(60)
            .height(60)
            .backgroundColor('#007dff')
            
            // 文本按钮
            Button('文本按钮', { type: ButtonType.Text })
                .backgroundColor(Color.Transparent)
                .fontColor('#007dff')
        }
        .padding(20);
    }
}
```

### 1.3 Image 图片

```typescript
@Entry
@Component
struct ImageExample {
    build() {
        Column({ space: 10 }) {
            // 本地图片
            Image($r('app.media.icon'))
                .width(100)
                .height(100)
                .borderRadius(10)
            
            // 网络图片
            Image('https://example.com/image.png')
                .width(200)
                .height(150)
                .objectFit(ImageFit.Cover)
            
            // 圆角图片
            Image($r('app.media.avatar'))
                .width(80)
                .height(80)
                .borderRadius(40)
        }
        .padding(20);
    }
}
```

### 1.4 输入组件

```typescript
@Entry
@Component
struct InputExample {
    @State text: string = '';
    @State password: string = '';
    
    build() {
        Column({ space: 20 }) {
            // 普通输入框
            TextInput({ placeholder: '请输入用户名' })
                .onChange((value: string) => {
                    this.text = value;
                })
            
            // 密码输入框
            TextInput({ placeholder: '请输入密码' })
                .type(InputType.Password)
                .showEyeIcon(true)
                .onChange((value: string) => {
                    this.password = value;
                })
            
            // 多行输入
            TextInput({ placeholder: '请输入内容' })
                .type(InputType.Multiline)
                .height(100)
        }
        .padding(20);
    }
}
```

---

## 2. 布局组件

### 2.1 Column / Row 线性布局

```typescript
@Entry
@Component
struct LayoutExample {
    build() {
        // 垂直布局
        Column({ space: 10 }) {
            Text('第一行');
            Text('第二行');
            Text('第三行');
        }
        .width('100%')
        .backgroundColor('#f5f5f5')
        
        // 水平布局
        Row({ space: 10 }) {
            Text('左侧');
            Text('中间');
            Text('右侧');
        }
        .width('100%')
        .height(50)
        .backgroundColor('#e0e0e0')
    }
}
```

### 2.2 Flex 弹性布局

```typescript
@Entry
@Component
struct FlexExample {
    build() {
        Column({ space: 20 }) {
            // 主轴对齐
            Flex({ justifyContent: FlexAlign.SpaceBetween }) {
                Text('左').width('30%').backgroundColor('#ffcccc');
                Text('中').width('30%').backgroundColor('#ccffcc');
                Text('右').width('30%').backgroundColor('#ccccff');
            }
            .height(60)
            
            // 交叉轴对齐
            Flex({ alignItems: ItemAlign.Center }) {
                Text('垂直居中')
                    .width('50%')
                    .height(30)
                    .backgroundColor('#ffcccc');
            }
            .height(60)
            .backgroundColor('#f5f5f5')
            
            // 换行
            Flex({ wrap: FlexWrap.Wrap }) {
                Text('1').width('40%').height(40).backgroundColor('#ffcccc');
                Text('2').width('40%').height(40).backgroundColor('#ccffcc');
                Text('3').width('40%').height(40).backgroundColor('#ccccff');
            }
        }
        .padding(20);
    }
}
```

### 2.3 Grid 网格布局

```typescript
@Entry
@Component
struct GridExample {
    // 网格数据
    private gridItems: string[] = [];
    
    aboutToAppear() {
        for (let i = 0; i < 12; i++) {
            this.gridItems.push(`Item ${i + 1}`);
        }
    }
    
    build() {
        Grid() {
            ForEach(this.gridItems, (item: string) => {
                GridItem() {
                    Text(item)
                        .fontSize(16)
                        .fontColor('#ffffff')
                }
                .backgroundColor('#007dff')
                .borderRadius(8)
            })
        }
        .columnsTemplate('1fr 1fr 1fr')  // 3列
        .rowsGap(10)
        .columnsGap(10)
        .padding(10)
    }
}
```

### 2.4 List 列表

```typescript
@Entry
@Component
struct ListExample {
    private dataSource: string[] = [];
    
    aboutToAppear() {
        for (let i = 0; i < 10; i++) {
            this.dataSource.push(`数据项 ${i + 1}`);
        }
    }
    
    build() {
        List({ space: 10 }) {
            ForEach(this.dataSource, (item: string) => {
                ListItem() {
                    Row() {
                        Text(item)
                            .fontSize(18)
                            .margin({ left: 20 });
                    }
                    .width('100%')
                    .height(60)
                    .backgroundColor('#ffffff')
                    .borderRadius(10);
                }
            })
        }
        .width('100%')
        .height('100%')
        .padding(10)
        .backgroundColor('#f5f5f5');
    }
}
```

---

## 3. 弹窗组件

### 3.1 AlertDialog 警告对话框

```typescript
@Entry
@Component
struct DialogExample {
    build() {
        Column({ space: 20 }) {
            Button('显示对话框')
                .onClick(() => {
                    AlertDialog.show({
                        title: '提示',
                        message: '确定要执行此操作吗？',
                        primaryButton: {
                            value: '取消',
                            action: () => {
                                console.log('点击了取消');
                            }
                        },
                        secondaryButton: {
                            value: '确定',
                            action: () => {
                                console.log('点击了确定');
                            }
                        }
                    });
                })
        }
        .width('100%')
        .height('100%')
        .justifyContent(FlexAlign.Center);
    }
}
```

### 3.2 ActionSheet 动作面板

```typescript
ActionSheet.show({
    title: '请选择操作',
    message: '选择后执行相应操作',
    sheets: [
        {
            title: '分享',
            action: () => {
                console.log('选择分享');
            }
        },
        {
            title: '收藏',
            action: () => {
                console.log('选择收藏');
            }
        },
        {
            title: '删除',
            action: () => {
                console.log('选择删除');
            }
        }
    ]
});
```

---

## 4. 路由导航

### 4.1 页面跳转

```typescript
// pages/home.ets
import router from '@ohos.router';

@Entry
@Component
struct Home {
    build() {
        Column() {
            Button('跳转到详情')
                .onClick(() => {
                    router.pushUrl({
                        url: 'pages/detail',
                        params: {
                            id: 123,
                            name: '张三'
                        }
                    });
                })
        }
    }
}

// pages/detail.ets
import router from '@ohos.router';

@Entry
@Component
struct Detail {
    onPageShow() {
        // 获取传递的参数
        const params = router.getParams();
        console.log('接收参数:', JSON.stringify(params));
    }
    
    build() {
        Column() {
            Text('详情页')
            Button('返回')
                .onClick(() => {
                    router.back();
                })
        }
    }
}
```

### 4.2 路由配置

```json
// module.json5
{
    "module": {
        "pages": {
            "src": [
                "pages/home",
                "pages/detail",
                "pages/profile"
            ]
        }
    }
}
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：基础组件 ==========
// 1. 创建登录页面，包含输入框和按钮
// 2. 实现图片展示组件

// ========== 练习 2：布局 ==========
// 1. 使用 Grid 实现九宫格
// 2. 使用 List 实现通讯录

// ========== 练习 3：弹窗 ==========
// 1. 实现确认对话框
// 2. 实现底部动作面板

// ========== 练习 4：路由 ==========
// 1. 实现页面跳转
// 2. 传递参数
```

---

## 🎯 今日自检清单

- [ ] Text / Button / Image 组件
- [ ] 输入组件 TextInput
- [ ] Column / Row / Flex 布局
- [ ] Grid / List 布局
- [ ] 弹窗组件
- [ ] 路由导航

---

## 下一步

明天我们将学习：**ArkTS 网络与存储**
