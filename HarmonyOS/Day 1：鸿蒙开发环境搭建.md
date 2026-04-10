> 学习日期：2026-05-05（周一）

## 学习目标

1. 掌握 DevEco Studio 安装与配置
2. 理解鸿蒙应用项目结构
3. 能够创建第一个鸿蒙应用

---

## 1. 开发环境搭建

### 1.1 安装 DevEco Studio

```bash
# 下载地址
# https://developer.harmonyos.com/cn/develop/deveco-studio

# Windows 安装
# 1. 下载 .exe 安装包
# 2. 运行安装程序
# 3. 配置 Node.js 和 ohpm 路径

# macOS 安装
# 1. 下载 .dmg 安装包
# 2. 拖拽到 Applications
# 3. 启动并配置
```

### 1.2 环境要求

| 组件 | 最低版本 | 推荐版本 |
|------|----------|----------|
| 操作系统 | Windows 10 / macOS 10.15 | Windows 11 / macOS 12+ |
| 内存 | 8GB | 16GB+ |
| 磁盘 | 100GB | SSD 256GB+ |
| Node.js | 14.x | 18.x+ |
| JDK | JDK 11 | JDK 17 |

### 1.3 配置 SDK

```bash
# 在 DevEco Studio 中
# 1. 打开 Settings -> HarmonyOS SDK
# 2. 下载需要的 SDK 版本
# 3. 配置 SDK 路径

# 命令行工具
ohpm -v  # 查看 ohpm 版本
hdc -v   # 查看 hdc 版本
```

---

## 2. 创建第一个应用

### 2.1 新建项目

```
DevEco Studio -> New Project -> HarmonyOS -> Empty Ability -> Next
```

填写配置：
- **Project name**: HelloWorld
- **Bundle name**: com.example.helloworld
- **Language**: ArkTS
- **API Version**: 10

### 2.2 项目结构

```
HelloWorld/
├── entry/
│   ├── src/
│   │   ├── main/
│   │   │   ├── ets/
│   │   │   │   ├── entryability/
│   │   │   │   │   └── EntryAbility.ts
│   │   │   │   └── pages/
│   │   │   │       └── index.ets
│   │   │   ├── resources/
│   │   │   │   ├── base/
│   │   │   │   │   ├── element/
│   │   │   │   │   └── layout/
│   │   │   │   └── profile/
│   │   │   └── module.json5
│   │   └── ohosTest/
│   └── build-profile.json5
├── ohosSDK/
└── build.gradle.kts
```

### 2.3 入口文件

```typescript
// entry/src/main/ets/entryability/EntryAbility.ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';

export default class EntryAbility extends UIAbility {
    onCreate(want, launchParam) {
        console.log('EntryAbility onCreate');
    }

    onDestroy() {
        console.log('EntryAbility onDestroy');
    }

    onWindowStageCreate(windowStage) {
        // 设置主页面
        windowStage.loadContent('pages/index');
        console.log('EntryAbility onWindowStageCreate');
    }

    onWindowStageDestroy() {
        console.log('EntryAbility onWindowStageDestroy');
    }
}
```

---

## 3. 基础页面

### 3.1 创建页面

```typescript
// entry/src/main/ets/pages/index.ets
@Entry
@Component
struct Index {
    @State message: string = 'Hello HarmonyOS';

    build() {
        Row() {
            Column() {
                Text(this.message)
                    .fontSize(50)
                    .fontWeight(FontWeight.Bold)
                    .margin({ bottom: 20 })
                
                Button('点击我')
                    .onClick(() => {
                        this.message = '你好，鸿蒙！';
                    })
            }
            .width('100%')
        }
        .height('100%')
    }
}
```

### 3.2 配置路由

```json
// entry/src/main/module.json5
{
    "module": {
        "name": "entry",
        "type": "entry",
        "description": "$string:module_desc",
        "mainElement": "EntryAbility",
        "pages": "$profile:main_pages",
        "abilities": [
            {
                "name": "EntryAbility",
                "srcEntry": "./ets/entryability/EntryAbility.ts",
                "description": "$string:entry_ability_desc",
                "icon": "$media:icon",
                "label": "$string:entry_ability_label",
                "startWindowImage": "$media:icon",
                "startWindowBackground": "$color:start_window_background"
            }
        ]
    }
}
```

```json
<!-- entry/src/main/resources/base/profile/main_pages.json -->
{
    "src": [
        "pages/index"
    ]
}
```

---

## 4. 运行与调试

### 4.1 运行应用

```bash
# 方式1：Deveco Studio 界面运行
# 1. 连接真机或模拟器
# 2. 点击 Run 按钮

# 方式2：命令行运行
# 使用 hdc 工具
hdc shell am start -n com.example.helloworld/entryability.EntryAbility
```

### 4.2 模拟器

```bash
# 在 DevEco Studio 中
# 1. Tools -> Device Manager
# 2. 选择 Emulator Tab
# 3. 点击 + 创建模拟器
# 4. 选择设备类型（Phone/Tad）
# 5. 启动模拟器
```

### 4.3 日志调试

```typescript
// 添加日志
import hilog from '@ohos.hilog';

// 打印日志
hilog.info(0x0001, 'testTag', 'Message: %{public}s', 'Hello');

// 调试
console.log('Debug info');
console.error('Error info');
```

---

## 5. 常见问题

### 5.1 SDK 配置问题

```bash
# 查看 SDK 状态
# DevEco Studio -> Settings -> HarmonyOS SDK

# 手动下载 SDK
# https://developer.harmonyos.com/cn/sdk
```

### 5.2 签名问题

```bash
# 自动签名
# 1. DevEco Studio -> Project Structure -> Signing
# 2. 勾选 "Automatically generate signature"
# 3. 点击 OK

# 手动签名
# 1. 生成签名文件
# 2. 配置签名信息
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：环境搭建 ==========
// 1. 安装 DevEco Studio
// 2. 配置 HarmonyOS SDK

// ========== 练习 2：创建应用 ==========
// 1. 创建 HelloWorld 应用
// 2. 运行到模拟器

// ========== 练习 3：页面交互 ==========
// 1. 创建带按钮的页面
// 2. 实现点击事件
```

---

## 🎯 今日自检清单

- [ ] DevEco Studio 安装配置
- [ ] 项目结构理解
- [ ] 创建第一个应用
- [ ] 运行与调试

---

## 下一步

明天我们将学习：**ArkTS 语法基础**
