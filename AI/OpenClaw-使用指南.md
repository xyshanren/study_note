# OpenClaw 从入门到精通

**国内小白用户完全指南**

> 特别适配：无梯子环境 | 国内用户视角 | 详细图文说明

---

## 目录

1. [初识 OpenClaw](#一初识-openclaw)
2. [快速开始](#二快速开始)
3. [基础操作](#三基础操作)
4. [进阶技巧](#四进阶技巧)
5. [实用小技巧](#五实用小技巧)
6. [常见问题解决](#六常见问题解决)
7. [进阶扩展](#七进阶扩展)

---

## 一、初识 OpenClaw

### 1.1 什么是 OpenClaw？

OpenClaw 是一个开源的 **AI Agent**（智能助手）框架。与普通的 ChatGPT 不同，OpenClaw 可以：

- 📁 **操作文件** - 读取、编辑、创建各种文件
- 💻 **执行命令** - 在终端运行各种命令
- 🌐 **访问网页** - 自动浏览网页、提取信息
- 📱 **集成通讯** - 连接飞书、钉钉等通讯工具
- 🧠 **长期记忆** - 记住之前的对话和操作

### 1.2 为什么国内用户更适合？

| 特性 | 说明 |
|------|------|
| 🌍 **国内镜像** | 已配置阿里云、淘宝镜像，下载安装无需梯子 |
| 🏠 **本地部署** | 数据保存在本地，保护隐私安全 |
| 🤖 ** Ollama 支持** | 可使用本地开源模型，无需 API Key |
| 💬 **中文优化** | 对中文用户友好，支持中文对话 |

### 1.3 准备工作

在开始之前，你需要准备：

#### 方案一：使用 Ollama（推荐新手）

完全免费，无需 API Key，在本地运行 AI 模型。

```bash
# 安装 Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 下载模型（推荐使用 qwen 或 llama3）
ollama pull qwen2.5:latest

# 或选择 llama3
ollama pull llama3:latest
```

> 💡 **提示**：模型下载需要一些时间，建议在网络稳定的环境下进行。

#### 方案二：使用 API Key

如果你有 OpenAI 或 Anthropic 的 API Key，可以直接使用。

| 提供商 | 模型 | 获取方式 |
|--------|------|----------|
| OpenAI | GPT-4 / GPT-4o | [官网](https://platform.openai.com/) |
| Anthropic | Claude 3.5 | [官网](https://www.anthropic.com/) |
| 硅基流动 | OpenAI 兼容 API | [官网](https://siliconflow.cn)（国内可访问） |
| 讯飞星火 | 科大讯飞 API | [官网](https://www.xfyun.cn/) |

> ⚠️ **注意**：API Key 需要付费，但体验更稳定。如果你在国内，推荐使用 **硅基流动**，国内访问速度快，价格也有竞争力。

---

## 二、快速开始

### 2.1 一键安装

#### Docker 方式（推荐）

```bash
# 下载安装脚本
curl -O https://raw.githubusercontent.com/your-repo/install-openclaw-docker.sh

# 添加执行权限
chmod +x install-openclaw-docker.sh

# 运行安装
./install-openclaw-docker.sh
```

安装过程中：
1. 选择 `QuickStart` 模式
2. 选择模型提供商（推荐 Ollama）
3. 按提示完成配置

#### npm 方式

```bash
# 下载安装脚本
curl -O https://raw.githubusercontent.com/your-repo/install-openclaw-npm.sh

# 添加执行权限
chmod +x install-openclaw-npm.sh

# 运行安装
./install-openclaw-npm.sh
```

### 2.2 启动服务

```bash
# Docker 方式
cd ~/openclaw
docker compose up -d

# npm 方式
openclaw dev
# 或
openclaw start
```

### 2.3 首次访问

打开浏览器，访问：`http://localhost:3000`

你会看到 OpenClaw 的界面：

```
┌─────────────────────────────────────────┐
│  🤖 OpenClaw                           │
├─────────────────────────────────────────┤
│                                         │
│  你好！我是你的 AI 助手                  │
│  有什么我可以帮你的吗？                   │
│                                         │
├─────────────────────────────────────────┤
│  ┌─────────────────────────────────┐   │
│  │ 输入你的问题...              📤 │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

---

## 三、基础操作

### 3.1 文件操作

#### 读取文件

```
请帮我读取 /home/user/documents/笔记.txt 的内容
```

#### 创建文件

```
请帮我创建一个名为 todo.md 的文件，内容如下：
# 待办事项

- [ ] 完成报告
- [ ] 发送邮件
- [ ] 整理桌面
```

#### 编辑文件

```
请帮我修改 todo.md，把 "完成报告" 改为 "完成项目报告"
```

#### 批量处理

```
请帮我把 downloads 文件夹里的所有 .txt 文件都读取一遍
```

### 3.2 命令执行

#### 基本命令

```
请帮我运行 ls -la 命令，看看当前目录有什么文件
```

#### 复杂操作

```
请帮我创建一个新的文件夹，命名为 myproject，然后在里面初始化一个 git 仓库
```

#### 运行脚本

```
请帮我运行 Python 脚本 /home/user/test.py
```

### 3.3 网页浏览

#### 获取网页内容

```
请帮我打开 https://github.com/openclaw/openclaw，看看这个项目的信息
```

#### 提取特定信息

```
请帮我从刚才的 GitHub 页面提取 Star 数量和最近更新时间
```

### 3.4 对话管理

#### 查看历史

```
请帮我回顾一下我们刚才讨论的内容
```

#### 清除记忆

```
请清除我们的对话历史
```

---

## 四、进阶技巧

### 4.1 编写复杂指令

#### 使用分隔符

当需要 AI 处理特定格式的内容时，使用分隔符可以提高准确性：

```
请帮我分析以下数据：
---
日期,销售额
2024-01-01,1000
2024-01-02,1500
2024-01-03,1200
---
请计算平均销售额
```

#### 分步骤指令

对于复杂任务，将其拆分成步骤：

```
请帮我完成以下任务：
1. 创建一个新目录 called "project"
2. 在目录中创建 README.md 文件
3. 初始化 git 仓库
4. 创建 .gitignore 文件
```

#### 使用参考上下文

```
基于之前创建的文件，请帮我添加更多内容
```

### 4.2 自定义工作流

#### 创建自动化任务

```
请帮我创建一个自动化脚本，每天凌晨 2 点自动备份 ~/documents 文件夹
```

#### 批量重命名文件

```
请帮我把当前目录下的所有 .jpeg 文件重命名为 .jpg
```

### 4.3 代码开发

#### 代码审查

```
请帮我审查 /home/user/project/src/main.js 的代码，找出潜在问题
```

#### 代码补全

```
请帮我补全这个 Python 函数的逻辑：
```python
def calculate_fibonacci(n):
    # 计算斐波那契数列
    pass
```
```

#### 项目初始化

```
请帮我创建一个新的 React 项目，名称为 my-app
```

### 4.4 数据处理

#### 格式转换

```
请帮我把 data.csv 转换为 JSON 格式
```

#### 数据分析

```
请分析 ~/sales/data.xlsx 文件，生成一个销售报告
```

#### 批量处理

```
请帮我压缩当前目录下所有的图片文件
```

---

## 五、实用小技巧

### 5.1 效率提升技巧

#### 1. 使用快捷指令

OpenClaw 支持快捷指令，快速完成常见操作：

```
# 快速文件操作
/open file /path/to/file    # 快速打开文件
/list /path                 # 快速列出目录
/search keyword             # 快速搜索

# 快速命令
/shell command              # 快速执行 shell 命令
/git command               # 快速执行 git 命令
```

#### 2. 保存常用提示词

你可以把常用的提示词保存为模板：

```
📝 我的常用模板：

【代码审查】
请帮我审查以下代码的 bugs 和性能问题：
[粘贴代码]

【周报生成】
请帮我根据以下工作内容生成周报：
[粘贴工作内容]

【翻译】
请帮我翻译以下内容为英文/中文：
[粘贴内容]
```

#### 3. 使用剪贴板

直接让 AI 读取或写入剪贴板：

```
请读取我剪贴板的内容
请把这段文字写入剪贴板
```

### 5.2 国内特色技巧

#### 1. 解决网络问题

```
# 让 AI 帮你下载无法访问的资源
请帮我下载 https://github.com/... 这个资源

# 使用镜像源
请帮我从 gitee 克隆这个项目
```

#### 2. 中文优化

```
# 使用中文提示词
请用中文回复
请使用简体中文

# 避免英文专业术语
请用通俗易懂的中文解释这个概念
```

#### 3. 本地模型加速

使用 Ollama 时，选择小模型提升响应速度：

```
# 推荐模型
ollama pull qwen2.5:7b      # 7B 参数，速度快
ollama pull llama3:8b        # 8B 参数，效果好
ollama pull qwen2.5:14b      # 14B 参数，效果更好
```

### 5.3 调试技巧

#### 1. 查看执行日志

```
请告诉我你刚才做了什么
```

#### 2. 分步执行

对于复杂任务，让 AI 一步步来：

```
请先执行第一步，完成后告诉我
```

#### 3. 验证结果

```
请验证一下刚才的操作是否成功
```

---

## 六、常见问题解决

### 6.1 启动问题

#### 问题：端口被占用

```bash
# 查找占用端口的进程
lsof -i :3000

# 杀掉进程
kill -9 <PID>

# 或使用其他端口启动
docker compose up -d -p 3001:3000
```

#### 问题：Docker 权限不足

```bash
# 将用户加入 docker 组
sudo usermod -aG docker $USER

# 重新登录或运行
newgrp docker
```

### 6.2 模型问题

#### 问题：Ollama 模型加载慢

```bash
# 预先加载模型到内存
ollama run qwen2.5:latest "你好"

# 或使用更小的模型
ollama pull qwen2.5:7b
```

#### 问题：API Key 无效

```
请检查你的 API Key 配置是否正确
```

### 6.3 性能问题

#### 问题：响应速度慢

1. 使用本地模型（Ollama）
2. 减少对话历史长度
3. 使用更小的模型
4. 关闭不必要的插件

#### 问题：内存不足

```bash
# 查看内存使用
free -h

# 关闭其他占用内存的程序
# 或使用更小的模型
```

---

## 七、进阶扩展

### 7.1 集成飞书/钉钉

OpenClaw 支持连接企业通讯工具：

```
请帮我配置飞书机器人
```

配置步骤：
1. 在飞书开放平台创建应用
2. 获取 App ID 和 App Secret
3. 在 OpenClaw 中配置
4. 即可在飞书中使用

### 7.2 自定义插件

开发者可以编写自定义插件：

```javascript
// 示例：自定义天气插件
module.exports = {
  name: 'weather',
  description: '查询天气',
  async execute(params) {
    // 获取天气信息
    return weatherData;
  }
};
```

### 7.3 API 接入

通过 API 让其他应用调用 OpenClaw：

```bash
# 启动 API 服务
openclaw api

# 调用示例
curl -X POST http://localhost:3000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "你好"}
```

### 7.4 生产环境部署

#### 使用 PM2 管理

```bash
# 安装 PM2
npm install -g pm2

# 启动
pm2 start openclaw

# 设置开机自启
pm2 startup
pm2 save
```

#### 使用 Nginx 反向代理

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
    }
}
```

---

## 快速命令参考

### 常用命令

| 命令 | 说明 |
|------|------|
| `openclaw init` | 初始化项目 |
| `openclaw dev` | 开发模式启动 |
| `openclaw start` | 生产模式启动 |
| `openclaw stop` | 停止服务 |
| `openclaw --help` | 查看帮助 |

### Docker 命令

| 命令 | 说明 |
|------|------|
| `docker compose up -d` | 启动服务 |
| `docker compose down` | 停止服务 |
| `docker compose logs -f` | 查看日志 |
| `docker compose restart` | 重启服务 |

### Ollama 命令

| 命令 | 说明 |
|------|------|
| `ollama list` | 列出已安装模型 |
| `ollama run qwen2.5` | 运行模型 |
| `ollama pull llama3` | 下载模型 |

---

## 总结

OpenClaw 是一个强大的 AI Agent 工具，通过本教程，你应该已经掌握了：

- ✅ 基础的文件操作和命令执行
- ✅ 复杂的任务处理能力
- ✅ 实用的小技巧和效率提升方法
- ✅ 常见问题的解决方法
- ✅ 进阶扩展的能力

**下一步建议**：

1. 多尝试不同的任务类型
2. 探索 Ollama 本地模型的强大功能
3. 学习编写自定义提示词
4. 尝试接入飞书、钉钉等工具

祝你在 OpenClaw 的使用过程中收获愉快的体验！

> 📚 更多资源：
> - 官方文档：https://github.com/openclaw/openclaw
> - 社区论坛：https://discord.gg/openclaw
> - 国内镜像：https://gitee.com/openclaw
