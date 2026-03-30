> 更新时间：2026-03-30 | 版本：DeerFlow 2.0 | 新增：Linux 双路径安装 + 多平台 API 配置 + 复杂任务示例

---

## 目录

1. [DeerFlow 是什么](#1-deerflow-是什么)
2. [安装前准备：先装 Docker Desktop（Windows）](#2-安装前准备先装-docker-desktopwindows)
3. [Linux 虚拟机安装（Ubuntu / AnolisOS）](#3-linux-虚拟机安装ubuntu--anolisos)
   - [3.1 Docker 方式安装](#31-docker-方式安装)
   - [3.2 非 Docker 方式安装（原生部署）](#32-非-docker-方式安装原生部署)
4. [获取 DeerFlow 代码](#4-获取-deerflow-代码)
5. [配置大语言模型 API（多平台汇总）](#5-配置大语言模型-api多平台汇总)
6. [启动 DeerFlow](#6-启动-deerflow)
7. [开始使用：你的第一个任务](#7-开始使用你的第一个任务)
8. [复杂任务示例：解锁 DeerFlow 的真正实力](#8-复杂任务示例解锁-deerflow-的真正实力)
9. [常见问题与解决](#9-常见问题与解决)

---

## 1. DeerFlow 是什么

**DeerFlow**（Deep Exploration and Efficient Research Flow）是字节跳动开源的**超级智能体运行框架**。

简单来说，它能帮你把一个复杂任务交给 AI，让 AI 自己规划步骤、调用工具、执行代码、生成报告——你只需要告诉它要做什么。

**它最擅长：**
- 📊 深度行业研究与报告生成
- 💻 复杂代码编写与调试
- 📈 数据分析与可视化
- 🎨 图像与视频内容创作
- 🌐 多步骤网页与内容创作
- 🔍 竞品调研与技术追踪

**一句话理解：** 你提需求，DeerFlow 的多个 AI 助手协同工作，在沙箱里自动完成任务，全程不需要你介入具体操作。

---

## 2. 安装前准备：先装 Docker Desktop（Windows）

DeerFlow 需要 Docker 来运行沙箱环境。如果你电脑上还没有 Docker Desktop，按以下步骤安装：

### 2.1 下载 Docker Desktop

打开浏览器，访问：

```
https://www.docker.com/products/docker-desktop/
```

点击 **Download for Windows**，下载安装包（约 500MB）。

> ⚠️ 如果你用的是中国大陆网络，Docker 官方下载可能很慢。可以访问阿里云或腾讯云的 Docker 加速器镜像站下载。

### 2.2 安装 Docker Desktop

1. 双击下载好的 `.exe` 安装包
2. 安装过程中勾选 **"Use WSL 2 instead of Hyper-V"**（推荐方式）
3. 安装完成后，点击 **"Close and restart"** 重启电脑

### 2.3 验证 Docker 是否安装成功

重启后，在开始菜单找到 **Docker Desktop** 并打开，等待右下角图标变为绿色。

打开 **PowerShell**（右键开始菜单 → Windows PowerShell），输入：

```powershell
docker --version
```

看到类似下面的输出就说明安装成功：

```
Docker version 26.x.x, build xxxxxx
```

### 2.4 配置 Docker 镜像加速（强烈推荐）

中国大陆访问 Docker Hub 官方镜像非常慢，建议配置国内加速源：

1. 打开 Docker Desktop → 设置（⚙️）→ **Docker Engine**
2. 在右侧编辑框中，将 JSON 内容替换为：

```json
{
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://docker.xuanyuan.me"
  ]
}
```

3. 点击 **Apply & Restart**

---

## 3. Linux 虚拟机安装（Ubuntu / AnolisOS）

### 3.1 Docker 方式安装

#### 步骤 1：安装 Docker

```bash
# Ubuntu
sudo apt update
sudo apt install docker.io docker-compose -y

# AnolisOS（龙蜥 OS，与 CentOS/RHEL 兼容）
sudo yum install docker docker-compose -y

# 启动 Docker 服务
sudo systemctl start docker
sudo systemctl enable docker

# 将当前用户加入 docker 组（避免每次都用 sudo）
sudo usermod -aG docker $USER
# 重新登录后生效
```

#### 步骤 2：克隆并启动

```bash
git clone https://github.com/bytedance/deer-flow.git
cd deer-flow

# 复制配置文件
cp .env.example .env
cp config.example.yaml config.yaml

# 配置 API Key（见 §5）
vim .env

# 一键启动
cd docker
docker compose --env-file ../.env up -d --build

# 检查状态
docker compose ps
```

#### 步骤 3：访问

启动成功后，打开浏览器访问 `http://<虚拟机IP>:2026`

---

### 3.2 非 Docker 方式安装（原生部署）

这种方式适合不想用 Docker、想直接控制环境的用户。

#### 环境要求

| 组件 | 版本要求 | 说明 |
|------|---------|------|
| **Python** | 3.11+ | 推荐用 `uv` 管理环境 |
| **Node.js** | 20+ | 推荐用 `nvm` 管理版本 |
| **pnpm** | 最新版 | 前端依赖管理 |
| **Git** | 任意版本 | 代码下载 |

#### 步骤 1：安装 Python 环境管理器 uv

```bash
# 方法 A：通过官方脚本安装（推荐）
curl -LsSf https://astral.sh/uv/install.sh | sh

# 方法 B：通过 pip 安装
pip install uv

# 安装完成后，重启终端或执行：
source $HOME/.cargo/env
```

#### 步骤 2：安装 Node.js 版本管理器 nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# 安装完成后，重启终端或执行：
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

#### 步骤 3：安装 Node.js 20 和 pnpm

```bash
nvm install 20
nvm use 20

# 安装 pnpm
curl -fsSL https://get.pnpm.io/install.sh | sh -
source ~/.bashrc
```

#### 步骤 4：克隆代码并安装依赖

```bash
git clone https://github.com/bytedance/deer-flow.git
cd deer-flow

# 安装 Python 依赖（uv 会自动创建虚拟环境）
uv sync

# 安装前端依赖
cd frontend
pnpm install
cd ..
```

#### 步骤 5：配置环境变量和模型

```bash
cp .env.example .env
cp config.example.yaml config.yaml

# 编辑 .env，填入 API Key（见 §5）
vim .env

# 编辑 config.yaml，配置模型（见 §5）
vim config.yaml
```

#### 步骤 6：启动

```bash
# 方式一：控制台界面（无 Web 界面，直接在终端交互）
uv run main.py

# 方式二：Web 界面（开发模式）
./bootstrap.sh -d

# 方式三：分别启动前后端
# 终端 1：启动后端
uv run uvicorn backend.app.main:app --reload --port 8000

# 终端 2：启动前端
cd frontend && pnpm dev
```

#### 步骤 7：访问

- Web 界面：http://localhost:3000
- API 接口：http://localhost:8000

> ⚠️ **注意**：非 Docker 方式不包含沙箱隔离环境，代码执行在宿主机上进行。生产环境建议使用 Docker 方式。

---

## 4. 获取 DeerFlow 代码

### 4.1 安装 Git（如果没有）

```powershell
# Windows：下载 https://git-scm.com/download/win 安装包，一路 Next
# Linux：sudo apt install git -y
```

### 4.2 克隆仓库

```powershell
cd F:\work\workspace          # 换成你喜欢的目录
git clone https://github.com/bytedance/deer-flow.git
cd deer-flow
```

> 如果 GitHub 访问缓慢，可以试试 Gitee 镜像：
> `git clone https://gitee.com/ByteDance/deer-flow.git`

### 4.3 项目目录结构

```
deer-flow/
├── backend/          # 后端服务（Agent 运行时）
├── frontend/         # 前端界面
├── docker/           # Docker 编排配置
├── skills/           # 技能模块（可扩展）
├── Makefile          # 一键启动脚本
├── .env.example      # 环境变量模板
└── config.example.yaml  # 模型配置模板
```

---

## 5. 配置大语言模型 API（多平台汇总）

DeerFlow 需要连接大语言模型才能工作。以下是支持的平台及配置方法。

### 5.1 国内主流平台配置

#### 🥇 DeepSeek（推荐）

| 项目 | 说明 |
|------|------|
| **官网** | https://platform.deepseek.com/ |
| **API 地址** | `https://api.deepseek.com` |
| **模型** | deepseek-chat（V3）、deepseek-reasoner（R1） |
| **优点** | 效果好、中文优化、价格便宜、新用户有免费额度 |
| **配置格式** | OpenAI 兼容格式 |

**`.env` 配置：**
```env
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**`config.yaml` 配置：**
```yaml
models:
  - name: deepseek-chat
    display_name: DeepSeek V3
    use: langchain_deepseek:ChatDeepSeek
    model: deepseek-chat
    api_key: $DEEPSEEK_API_KEY
    max_tokens: 8192
    temperature: 0.7

default_model: deepseek-chat
```

---

#### 🥈 硅基流动（SiliconFlow）

| 项目 | 说明 |
|------|------|
| **官网** | https://cloud.siliconflow.cn/ |
| **API 地址** | `https://api.siliconflow.cn/v1` |
| **模型** | DeepSeek-V3、Qwen2.5、FLUX 等开源模型 |
| **优点** | 模型种类多、按量计费、国内直连 |
| **兼容性** | OpenAI API 兼容 |

**`.env` 配置：**
```env
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**`config.yaml` 配置：**
```yaml
models:
  - name: deepseek-ai/DeepSeek-V3
    display_name: SiliconFlow DeepSeek V3
    use: langchain_openai:ChatOpenAI
    model: deepseek-ai/DeepSeek-V3
    api_key: $OPENAI_API_KEY
    base_url: https://api.siliconflow.cn/v1
    max_tokens: 8192
    temperature: 0.7

default_model: deepseek-ai/DeepSeek-V3
```

---

#### 🥉 华为云（盘古大模型）

| 项目 | 说明 |
|------|------|
| **官网** | https://www.huaweicloud.com/ |
| **服务** | 盘古大模型 / ModelArts |
| **认证方式** | AK + SK + Project ID |
| **优点** | 国产大厂、稳定性高、企业友好 |

**`.env` 配置：**
```env
HUAWEI_CLOUD_AK=xxxxxxxxxxxxxxxx
HUAWEI_CLOUD_SK=xxxxxxxxxxxxxxxx
HUAWEI_CLOUD_PROJECT_ID=cn-north-4
HUAWEI_CLOUD_REGION=cn-north-4
```

> ⚠️ 华为云使用自有 SDK，DeerFlow 可能需要额外配置 LangChain 的华为云集成包，或通过 OpenAI 兼容网关转发。

---

#### AtmoGit AI

| 项目 | 说明 |
|------|------|
| **官网** | https://www.atmogit.ai/ |
| **特点** | 开源模型聚合平台 |
| **兼容性** | OpenAI API 兼容 |

**`.env` 配置：**
```env
OPENAI_API_KEY=your-atmogit-api-key
```

**`config.yaml` 配置：**
```yaml
models:
  - name: atmogit-model
    display_name: AtmoGit AI
    use: langchain_openai:ChatOpenAI
    model: atmogit-model-name
    api_key: $OPENAI_API_KEY
    base_url: https://api.atmogit.ai/v1
    max_tokens: 4096

default_model: atmogit-model
```

> 📌 AtmoGit AI 是新兴平台，配置前请确认其最新的 API 地址和支持的模型名称。

---

### 5.2 其他平台快速配置表

| 平台 | Base URL | 格式 | 备注 |
|------|---------|------|------|
| **火山引擎（豆包）** | `https://ark.cn-beijing.volces.com/api/v3` | OpenAI 兼容 | 国内直连，文档：ark.console.volcengine.com |
| **阿里云通义千问** | `https://dashscope.aliyuncs.com/compatible-mode/v1` | OpenAI 兼容 | 需要开通 DashScope 服务 |
| **腾讯混元** | `https://hunyuan.cloud.tencent.com` | 自有 SDK | 需安装腾讯云 SDK |
| **智谱 GLM** | `https://open.bigmodel.cn/api/paas/v4` | OpenAI 兼容 | 新用户有免费额度 |
| **OpenAI（GPT-4）** | `https://api.openai.com/v1` | OpenAI 原生 | 需要翻墙 |

---

### 5.3 配置示例：火山引擎（豆包）

```env
OPENAI_API_KEY=your-ark-api-key
```

```yaml
models:
  - name: doubao-pro-32k
    display_name: 豆包 Pro 32K
    use: langchain_openai:ChatOpenAI
    model: doubao-pro-32k
    api_key: $OPENAI_API_KEY
    base_url: https://ark.cn-beijing.volces.com/api/v3
    max_tokens: 8192

default_model: doubao-pro-32k
```

---

### 5.4 配置示例：智谱 GLM

```env
OPENAI_API_KEY=your-zhipu-api-key
```

```yaml
models:
  - name: glm-4
    display_name: 智谱 GLM-4
    use: langchain_openai:ChatOpenAI
    model: glm-4
    api_key: $OPENAI_API_KEY
    base_url: https://open.bigmodel.cn/api/paas/v4
    max_tokens: 4096

default_model: glm-4
```


---

### 5.5 Ollama 本地模型配置

**Ollama** 是一款开源的本地大模型运行工具，让你在自己的电脑上运行 Llama、Qwen、Mistral 等开源模型，无需联网、完全免费、隐私安全。

#### 5.5.1 Ollama vs 云端 API 对比

| 对比项 | Ollama 本地 | 云端 API（DeepSeek 等） |
|--------|-------------|-------------------------|
| **费用** | 完全免费（只需电费） | 按调用量付费 |
| **隐私** | 数据永不离开本地 | 数据上传到第三方 |
| **速度** | 取决于显卡（RTX 4090 很快） | 取决于网络 |
| **模型质量** | 中等（7B~70B 开源模型） | 顶级（GPT-4、Claude 等闭源模型） |
| **适用场景** | 日常学习、隐私敏感任务 | 高质量输出、复杂推理 |
| **推荐配置** | RTX 3060 以上显卡，16GB+ 显存 | 任意设备，有网络即可 |

---

#### 5.5.2 安装 Ollama

**Windows / macOS / Linux 通用：**

1. 访问官网：https://ollama.com/download
2. 下载对应系统的安装包
3. 双击安装，无需配置

**Linux 一行命令安装：**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**验证安装成功：**
```bash
ollama --version
# 输出类似：ollama version 0.5.x
```

---

#### 5.5.3 下载模型

Ollama 从模型库（ollama.com/library）下载模型。常用模型推荐：

| 模型 | 参数量 | 显存要求 | 适用场景 | 下载命令 |
|------|--------|----------|---------|---------|
| **Llama 3.2** | 3B / 70B | 4GB / 48GB | 日常对话、编程 | `ollama pull llama3.2` |
| **Qwen2.5** | 7B / 14B / 32B | 8GB / 16GB / 24GB | 中文对话（推荐） | `ollama pull qwen2.5:7b` |
| **Mistral** | 7B | 8GB | 英文为主 | `ollama pull mistral` |
| **DeepSeek R1** | 7B / 14B / 70B | 8GB / 16GB / 48GB | 推理能力强 | `ollama pull deepseek-r1:7b` |
| **Codellama** | 7B / 34B | 8GB / 24GB | 代码生成（推荐） | `ollama pull codellama:7b` |

> 💡 **显存参考**：RTX 3060（12GB）推荐 7B 模型；RTX 4090（24GB）可跑 14B~32B 模型；70B 模型需要专业级显卡（如 A100 80GB）。

**快速开始（推荐中文用户）：**
```bash
# 下载 Qwen2.5 7B 模型（中文优化，约 4.7GB）
ollama pull qwen2.5:7b

# 如果显存充足，下载 14B 版本（效果更好）
ollama pull qwen2.5:14b
```

**下载进度查看**：Ollama 会显示下载百分比和速度，耐心等待即可。

---

#### 5.5.4 测试 Ollama

安装完成后，在终端测试：

```bash
# 启动 Ollama 服务（后台自动运行）
ollama serve

# 测试对话（另一个终端窗口）
ollama run qwen2.5:7b "你好，请用一句话介绍自己"
```

如果看到 AI 的回复，说明安装成功！

---

#### 5.5.5 配置 DeerFlow 使用 Ollama

Ollama 提供与 OpenAI 兼容的 API 接口，DeerFlow 可以直接使用。

**步骤 1：确认 Ollama 服务运行中**

Ollama 默认在 `http://localhost:11434` 提供服务。

```bash
# 检查服务状态
curl http://localhost:11434/api/tags
```

如果返回 JSON 列表，说明服务正常。

**步骤 2：配置 DeerFlow 的 config.yaml**

在 `config.yaml` 中添加 Ollama provider：

```yaml
models:
  # ... 其他模型配置 ...

  # ========== Ollama 本地模型 ==========
  - name: qwen2.5:7b
    display_name: Qwen 2.5 7B（本地）
    use: langchain_openai:ChatOpenAI
    model: qwen2.5:7b
    api_key: ollama  # Ollama 不需要真实 API Key，使用占位符
    base_url: http://localhost:11434/v1
    max_tokens: 4096
    temperature: 0.7

  - name: llama3.2
    display_name: Llama 3.2 3B（本地）
    use: langchain_openai:ChatOpenAI
    model: llama3.2
    api_key: ollama
    base_url: http://localhost:11434/v1
    max_tokens: 4096
    temperature: 0.7

# 设置默认模型（可选）
default_model: qwen2.5:7b
```

**步骤 3：`.env` 文件配置**

`.env` 中无需添加特殊变量（因为用的是占位符 `ollama`）：

```env
# Ollama 不需要 API Key，保持 .env 简洁即可
# 如果后续要切换到云端模型，在此处添加对应的 API Key
```

---

#### 5.5.6 Ollama 常用命令

| 命令 | 说明 |
|------|------|
| `ollama run <model>` | 运行模型并进入对话 |
| `ollama pull <model>` | 下载模型 |
| `ollama list` | 列出已下载的模型 |
| `ollama rm <model>` | 删除模型 |
| `ollama show <model>` | 显示模型信息 |
| `ollama serve` | 启动 Ollama 服务（后台自动启动） |
| `ollama ps` | 显示正在运行的模型 |

---

#### 5.5.7 Ollama + DeerFlow 高级玩法

**玩法 1：多模型切换**

配置多个 Ollama 模型 + 云端模型，通过修改 `default_model` 快速切换：

```yaml
models:
  # 本地快速响应
  - name: qwen2.5:7b
    display_name: Qwen 2.5 7B（本地）
    use: langchain_openai:ChatOpenAI
    model: qwen2.5:7b
    api_key: ollama
    base_url: http://localhost:11434/v1

  # 云端高质量输出
  - name: deepseek-chat
    display_name: DeepSeek V3（云端）
    use: langchain_deepseek:ChatDeepSeek
    model: deepseek-chat
    api_key: $DEEPSEEK_API_KEY

default_model: qwen2.5:7b  # 默认用本地，省钱
```

**玩法 2：用 Codellama 专门处理代码任务**

通过 DeerFlow 的 MCP 工具或自定义路由，让特定任务使用不同模型：

```yaml
models:
  # 通用对话
  - name: qwen2.5:7b
    display_name: Qwen 2.5 7B
    use: langchain_openai:ChatOpenAI
    model: qwen2.5:7b
    api_key: ollama
    base_url: http://localhost:11434/v1

  # 代码专用
  - name: codellama:7b
    display_name: CodeLlama 7B（代码）
    use: langchain_openai:ChatOpenAI
    model: codellama:7b
    api_key: ollama
    base_url: http://localhost:11434/v1
```

**玩法 3：Ollama 量化模型（节省显存）**

Ollama 支持模型量化，用更少显存运行更大模型：

```bash
# 下载量化版本（Q 4_K_M 表示 4-bit 量化，Medium 质量）
ollama pull qwen2.5:7b-q4_k_m

# 可用量化等级：
# Q2_K - 最低质量，最大压缩
# Q3_K_M - 低质量
# Q4_K_M - 平衡（推荐）
# Q5_K_M - 高质量
# Q6_K - 接近原始质量
# Q8_0 - 几乎无压缩
```

---

#### 5.5.8 Ollama 常见问题

**❌ 问题 1：ollama run 报错 "model not found"**

**原因**：模型未下载

**解决**：先下载模型
```bash
ollama pull qwen2.5:7b
```

---

**❌ 问题 2：显存不足（CUDA out of memory）**

**原因**：模型太大，显卡显存不够

**解决**：
1. 下载更小的模型（如 3B、7B 版本）
2. 使用量化版本（如 `qwen2.5:7b-q4_k_m`）
3. 关闭其他占用显存的程序

```bash
# 查看正在运行的模型
ollama ps

# 停止运行中的模型
ollama stop <model-name>

# 或者直接重启 Ollama
sudo systemctl restart ollama
```

---

**❌ 问题 3：DeerFlow 连接 Ollama 失败**

**检查步骤**：
1. 确认 Ollama 服务正在运行：
   ```bash
   curl http://localhost:11434/api/tags
   ```

2. 确认 `base_url` 配置正确（注意是 `/v1` 后缀）：
   ```yaml
   base_url: http://localhost:11434/v1  # ✓ 正确
   base_url: http://localhost:11434     # ✗ 错误
   ```

3. 如果 Ollama 在虚拟机/远程服务器，修改 IP：
   ```yaml
   base_url: http://192.168.1.100:11434/v1
   ```

---

**❌ 问题 4：Windows 上 Ollama 速度很慢**

**原因**：Windows 原生支持不如 Linux/macOS，且默认用 CPU 加速

**解决**：
1. 确保 NVIDIA 驱动和 CUDA 已安装
2. 在环境变量中指定 CUDA 路径：
   ```powershell
   setx OLLAMA_HOST "http://localhost:11434"
   ```

---

#### 5.5.9 Ollama 进阶：Ollama API 详解

Ollama 提供 REST API，可以直接调用：

```bash
# 对话 API
curl http://localhost:11434/api/chat -d '{
  "model": "qwen2.5:7b",
  "messages": [{"role": "user", "content": "你好"}]
}'

# 生成 API（不带对话上下文）
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5:7b",
  "prompt": "写一首关于春天的诗"
}'

# 查看可用模型
curl http://localhost:11434/api/tags
```

DeerFlow 内部通过 LangChain 调用 Ollama，API 格式与 OpenAI 兼容，所以 `base_url` 需要加 `/v1` 后缀。

---

#### 5.5.10 推荐：Ollama + DeerFlow 最佳实践

| 场景 | 推荐配置 | 说明 |
|------|---------|------|
| **日常学习** | Qwen 2.5 7B + 量化 | 速度快，效果够用 |
| **隐私任务** | 任意本地模型 | 数据完全本地 |
| **代码开发** | CodeLlama 7B + Qwen 2.5 | 分工协作 |
| **低成本测试** | Llama 3.2 3B | 最低门槛 |
| **最高质量（需高端显卡）** | Qwen 2.5 32B + DeepSeek R1 70B | 专业级体验 |

---

> 💡 **提示**：Ollama 非常适合作为 DeerFlow 的"入门配置"，先用本地模型熟悉 DeerFlow 的使用方式，之后再根据需要升级到云端 API 获得更高质量输出。

---

### 5.6 多模型配置与智能路由

#### 5.6.1 为什么需要多模型？

单一模型难以应对所有场景，多模型配置可以：

| 优势 | 说明 |
|------|------|
| **专业分工** | 代码用 CodeLlama，写作用 GPT-4，各尽其长 |
| **成本优化** | 简单任务用本地模型，复杂任务用云端 |
| **容错备份** | 一个模型不可用时自动切换到另一个 |
| **隐私保护** | 敏感任务走本地模型，数据不出本机 |

---

#### 5.6.2 DeerFlow 多模型配置架构

DeerFlow 支持在 `config.yaml` 中配置多个模型，通过 `default_model` 设置默认模型：

```yaml
models:
  # ===== 主力云端模型 =====
  - name: deepseek-chat
    display_name: DeepSeek V3
    use: langchain_deepseek:ChatDeepSeek
    model: deepseek-chat
    api_key: $DEEPSEEK_API_KEY
    max_tokens: 8192
    temperature: 0.7

  # ===== 代码专用模型 =====
  - name: codellama:7b
    display_name: CodeLlama 7B（本地）
    use: langchain_openai:ChatOpenAI
    model: codellama:7b
    api_key: ollama
    base_url: http://localhost:11434/v1
    max_tokens: 4096

  # ===== 本地备用模型 =====
  - name: qwen2.5:7b
    display_name: Qwen 2.5 7B（本地）
    use: langchain_openai:ChatOpenAI
    model: qwen2.5:7b
    api_key: ollama
    base_url: http://localhost:11434/v1
    max_tokens: 4096

  # ===== 推理增强模型 =====
  - name: deepseek-ai/DeepSeek-R1
    display_name: DeepSeek R1（推理）
    use: langchain_openai:ChatOpenAI
    model: deepseek-ai/DeepSeek-R1
    api_key: $OPENAI_API_KEY
    base_url: https://api.siliconflow.cn/v1
    max_tokens: 8192

# 设置默认模型
default_model: deepseek-chat
```

---

#### 5.6.3 `.env` 文件配置

```env
# 云端 API Keys
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# 本地 Ollama 不需要 API Key（已在 config.yaml 中使用占位符 "ollama"）
```

---

#### 5.6.4 模型选择策略

##### 策略一：按任务类型自动选择

DeerFlow 可以通过**系统提示词**让 Agent 自动选择合适的模型：

```yaml
# 在 config.yaml 中配置模型描述，帮助 Agent 做决策
models:
  - name: deepseek-chat
    display_name: DeepSeek V3（通用）
    description: "综合能力强，适合日常对话、写作、分析"

  - name: codellama:7b
    display_name: CodeLlama（代码专用）
    description: "代码生成、调试、解释能力强"

  - name: qwen2.5:7b
    display_name: Qwen 2.5（本地快速）
    description: "响应快、免费、本地运行，适合简单任务"

  - name: deepseek-ai/DeepSeek-R1
    display_name: DeepSeek R1（深度推理）
    description: "复杂推理、数学问题、逻辑分析"
```

##### 策略二：手动切换（推荐新手）

在 DeerFlow 界面中，可以手动指定使用的模型：

```
# 在任务描述中指定
@model:codellama:7b
请帮我写一个 Python 快速排序算法

# 或者切换默认模型后发起新对话
```

##### 策略三：按成本自动路由

```yaml
# config.yaml 中的高级配置示例
model_routing:
  strategy: cost_aware  # cost_aware | quality_first | speed_first

  # 不同任务的模型选择
  task_models:
    code_generation: codellama:7b      # 便宜
    complex_reasoning: deepseek-ai/DeepSeek-R1  # 稍贵但强
    quick_lookup: qwen2.5:7b           # 最快最便宜
    default: deepseek-chat             # 综合最优
```

---

#### 5.6.5 多模型协作工作流

#### 工作流示例：复杂项目任务

```
用户任务：写一个 Web 应用，生成配套图标，并部署到云服务器

DeerFlow 多模型协作流程：

Step 1: 规划 Agent（DeepSeek V3）
├── 分析任务需求
├── 拆分子任务：后端 + 前端 + 图标 + 部署
└── 制定执行计划

Step 2: 代码 Agent（CodeLlama）
├── 编写后端 API
├── 编写前端界面
└── 编写部署脚本

Step 3: 图标生成（如果有 MCP 图像工具）
├── 调用 FLUX/DALL-E
└── 生成应用图标

Step 4: 质量审查 Agent（DeepSeek V3）
├── 代码审查
├── 提出优化建议
└── 最终确认

Step 5: 部署执行
└── 脚本部署到服务器
```

---

#### 5.6.6 实战配置模板

##### 模板 A：个人开发者（预算有限）

```yaml
models:
  # 主力模型 - DeepSeek（性价比高）
  - name: deepseek-chat
    display_name: DeepSeek V3
    use: langchain_deepseek:ChatDeepSeek
    model: deepseek-chat
    api_key: $DEEPSEEK_API_KEY

  # 代码专用 - Ollama 本地
  - name: codellama:7b
    display_name: CodeLlama（本地）
    use: langchain_openai:ChatOpenAI
    model: codellama:7b
    api_key: ollama
    base_url: http://localhost:11434/v1

  # 快速响应 - Ollama 本地
  - name: qwen2.5:7b
    display_name: Qwen 2.5（本地）
    use: langchain_openai:ChatOpenAI
    model: qwen2.5:7b
    api_key: ollama
    base_url: http://localhost:11434/v1

default_model: deepseek-chat
```

**.env 配置：**
```env
DEEPSEEK_API_KEY=sk-your-key
```

**使用策略：**
- 日常对话 → DeepSeek V3
- 写代码 → CodeLlama（免费）
- 隐私任务 → Qwen 2.5（完全本地）
- 复杂推理 → DeepSeek V3

---

##### 模板 B：追求最高质量

```yaml
models:
  # 通用对话 - Claude
  - name: claude-3-5-sonnet
    display_name: Claude 3.5 Sonnet
    use: langchain_anthropic:ChatAnthropic
    model: claude-3-5-sonnet-20240620
    api_key: $ANTHROPIC_API_KEY

  # 代码专用 - GPT-4o
  - name: gpt-4o
    display_name: GPT-4o（代码）
    use: langchain_openai:ChatOpenAI
    model: gpt-4o
    api_key: $OPENAI_API_KEY

  # 深度推理 - DeepSeek R1
  - name: deepseek-ai/DeepSeek-R1
    display_name: DeepSeek R1
    use: langchain_openai:ChatOpenAI
    model: deepseek-ai/DeepSeek-R1
    api_key: $OPENAI_API_KEY
    base_url: https://api.siliconflow.cn/v1

  # 备用 - DeepSeek V3
  - name: deepseek-chat
    display_name: DeepSeek V3
    use: langchain_deepseek:ChatDeepSeek
    model: deepseek-chat
    api_key: $DEEPSEEK_API_KEY

default_model: claude-3-5-sonnet
```

**.env 配置：**
```env
ANTHROPIC_API_KEY=sk-ant-xxxxxxxx
OPENAI_API_KEY=sk-xxxxxxxx
DEEPSEEK_API_KEY=sk-xxxxxxxx
```

**使用策略：**
- 写作/分析 → Claude 3.5
- 代码 → GPT-4o
- 复杂推理 → DeepSeek R1
- 备用 → DeepSeek V3

---

##### 模板 C：企业内网部署

```yaml
models:
  # 主力 - 内部部署模型
  - name: qwen2.5:72b-instruct
    display_name: Qwen 2.5 72B（内网）
    use: langchain_openai:ChatOpenAI
    model: qwen2.5:72b-instruct
    api_key: internal-key
    base_url: http://192.168.1.100:8000/v1

  # 代码 - 内网 CodeLlama
  - name: codellama:34b
    display_name: CodeLlama 34B（内网）
    use: langchain_openai:ChatOpenAI
    model: codellama:34b
    api_key: internal-key
    base_url: http://192.168.1.100:8000/v1

  # 推理 - DeepSeek R1
  - name: deepseek-r1:70b
    display_name: DeepSeek R1 70B（内网）
    use: langchain_openai:ChatOpenAI
    model: deepseek-r1:70b
    api_key: internal-key
    base_url: http://192.168.1.100:8000/v1

default_model: qwen2.5:72b-instruct
```

**优势：** 完全内网，数据不外泄，适合金融、医疗、政府等敏感行业

---

#### 5.6.7 成本优化技巧

##### 技巧 1：模型分级使用

```
优先级从低到高：

1. 免费（本地 Ollama）
   ↓ 效果不够用？
2. 便宜（DeepSeek V3，约 1元/百万token）
   ↓ 推理不够强？
3. 中等（GPT-4o，约 15元/百万token）
   ↓ 需要顶级推理？
4. 昂贵（GPT-4o 32K，约 60元/百万token）
```

##### 技巧 2：利用上下文缓存

部分模型（如 DeepSeek）支持上下文缓存，重复内容不计费：

```yaml
models:
  - name: deepseek-chat
    display_name: DeepSeek V3
    use: langchain_deepseek:ChatDeepSeek
    model: deepseek-chat
    api_key: $DEEPSEEK_API_KEY
    # DeepSeek V3 支持上下文复用，降低成本
```

##### 技巧 3：本地处理简单任务

```yaml
# 将简单任务自动路由到本地模型
local_model: qwen2.5:7b

# 需要触发的关键词
local_keywords:
  - "翻译"
  - "解释"
  - "简单"
  - "快速"
  - "查一下"
```

##### 技巧 4：监控与告警

```yaml
# 配置成本监控
cost_monitoring:
  enabled: true
  daily_limit: 50  # 每日预算 50 元
  alert_email: your@email.com
  
  # 模型成本表
  model_costs:
    deepseek-chat: 1      # 元/百万token
    gpt-4o: 15            # 元/百万token
    claude-3-5-sonnet: 18 # 元/百万token
```

---

#### 5.6.8 多模型配置检查清单

配置完多模型后，按此清单检查：

- [ ] `.env` 文件包含所有需要的 API Keys
- [ ] `config.yaml` 中每个模型都有唯一 `name`
- [ ] 每个模型都有有效的 `api_key`（或占位符）
- [ ] `base_url` 正确（注意 `/v1` 后缀）
- [ ] `default_model` 设置为存在的模型名
- [ ] 启动 DeerFlow 后测试每个模型连通性

**测试命令：**

```bash
# 测试 Ollama
curl http://localhost:11434/api/tags

# 测试 DeepSeek
curl https://api.deepseek.com/chat/completions \
  -H "Authorization: Bearer $DEEPSEEK_API_KEY" \
  -d '{"model":"deepseek-chat","messages":[{"role":"user","content":"hi"}]}'

# 在 DeerFlow 界面中分别用不同模型执行简单任务，验证正常工作
```

---

#### 5.6.9 常见问题

**❌ 问题 1：模型切换后上下文丢失**

**原因**：不同模型有不同的上下文窗口，切换模型会丢失之前的对话历史。

**解决**：
- 如果需要多模型协作，在一个任务内不要频繁切换
- 使用 DeerFlow 的"任务续写"功能时注意模型一致性

---

**❌ 问题 2：本地模型响应很慢**

**原因**：模型太大或显卡性能不足

**解决**：
- 使用更小的量化模型（如 `qwen2.5:3b`）
- 关闭其他占用 GPU 的程序
- 使用 Q4_K_M 量化版本

---

**❌ 问题 3：API Key 费用超支**

**解决**：
1. 设置 API 平台的用量限制
2. 使用成本更低的模型处理简单任务
3. 增加本地模型的使用比例
4. 开启成本监控和告警

---

#### 5.6.10 进阶：自定义模型路由

对于高级用户，可以通过修改 DeerFlow 的源代码实现更精细的路由控制：

```python
# backend/app/core/model_router.py（示例路径）

def select_model(task: str, context: dict) -> str:
    """根据任务类型选择最合适的模型"""
    
    # 代码相关任务
    if any(kw in task for kw in ["代码", "code", "python", "bug"]):
        return "codellama:7b"
    
    # 复杂推理任务
    if any(kw in task for kw in ["推理", "证明", "分析", "reason"]):
        return "deepseek-ai/DeepSeek-R1"
    
    # 隐私敏感任务
    if context.get("sensitive"):
        return "qwen2.5:7b"  # 本地模型
    
    # 默认使用主力模型
    return "deepseek-chat"
```

---

> 💡 **最佳实践**：从简单配置开始，逐步增加模型。先配置 1-2 个模型跑通流程，再根据实际需求添加专用模型。

---

## 6. 启动 DeerFlow

### 6.1 Windows / Linux Docker 方式

```powershell
# 进入 deer-flow 目录
cd deer-flow

# 首次启动：拉取镜像并构建（约 5-15 分钟）
make docker-start

# 后续启动（无需重新拉取）
make docker-start
```

如果 `make` 命令不可用：

```bash
cd docker
docker compose --env-file ../.env up -d --build
```

### 6.2 检查启动状态

```bash
docker compose ps
```

看到以下服务状态都是 **Up** 就成功了：

| 服务 | 状态 |
|------|------|
| nginx | Up |
| frontend | Up |
| gateway | Up |
| langgraph | Up |
| postgres | Up |

### 6.3 访问

打开浏览器，访问：

```
http://localhost:2026
```

看到 DeerFlow 的 Web 界面，就说明安装成功了！🎉

---

## 7. 开始使用：你的第一个任务

### 7.1 界面介绍

DeerFlow 的界面非常简洁：

- **左侧边栏**：工作区列表
- **中央区域**：对话和任务执行区域
- **底部对话框**：输入你的任务

### 7.2 第一次对话

在底部对话框中，输入一个简单的测试任务：

```
请用 Python 写一个计算斐波那契数列的函数，并解释它的原理
```

DeerFlow 会自动：
1. 分析你的需求
2. 调用子智能体执行代码
3. 在沙箱中运行代码
4. 返回结果给你

---

## 8. 复杂任务示例：解锁 DeerFlow 的真正实力

### 8.1 🎮 任务一：生成一套游戏图标

**任务描述：**
```
帮我设计一套（共8个）像素风格 RPG 游戏道具图标，分别是：生命药水、魔法药水、武器（剑）、盾牌、钥匙、金币、宝石、地图卷轴。要求 PNG 格式，64x64 像素，统一的像素艺术风格。
```

**DeerFlow 会自动：**
1. 生成 8 个 SVG/PNG 图标文件
2. 统一风格（颜色、线宽、像素密度）
3. 在沙箱中渲染并导出 PNG
4. 打包成一个 ZIP 文件供下载

**进阶玩法：** 附加"添加阴影效果"、"导出 128x128 大版本"、"制作图标预览大图"

---

### 8.2 📸 任务二：生成一套美女写真图

**任务描述：**
```
使用 Stable Diffusion 或其他图像生成模型，生成一套（共6张）国风汉服美女写真。主题是"竹林七贤"，每张图需要不同的姿态和表情，风格统一为工笔画效果，背景为中国古典园林。
```

**DeerFlow 会自动：**
1. 检查当前环境的图像生成工具（MCP 工具或 API）
2. 如果未配置，推荐配置硅基流动的 FLUX 模型或使用 OpenAI DALL-E
3. 生成 6 张不同构图和姿态的图像
4. 统一风格后处理
5. 展示并提供下载

> ⚠️ **注意**：图像生成需要配置图像生成 API（如 OpenAI DALL-E、Stable Diffusion、FLUX 等）。DeerFlow 2.0 支持 MCP 工具扩展，可接入图像生成服务。

**MCP 图像生成工具配置示例（config.yaml）：**
```yaml
mcp_tools:
  - name: image_generation
    type: sdxl  # 或 dall-e, flux
    api_key: $IMAGE_API_KEY
    base_url: https://api.siliconflow.cn/v1  # 硅基流动 FLUX
```

---

### 8.3 🎬 任务三：生成水墨风格太极拳视频

**任务描述：**
```
帮我制作一个 30 秒的水墨风格太极拳教学视频。内容是"起势"到"左右野马分鬃"的前 15 秒动作。要求：
1. 水墨画风格，人物轮廓用毛笔笔触勾勒
2. 背景是中国传统山水画（竹林、山石、云雾）
3. 配乐使用古琴背景音乐
4. 底部有太极拳动作名称的文字标注
5. 输出 MP4 格式，1080P 分辨率
```

**DeerFlow 会自动：**
1. 设计视频分镜脚本（逐帧动作描述）
2. 生成每帧的水墨风格图像
3. 如果配置了视频生成工具（如 Runway、Pika、Sora），生成视频片段
4. 如无视频生成能力，生成完整的**分镜图册 + 动作指南文档**，说明如何用手绘或 AI 视频工具继续制作
5. 提供配乐建议和下载链接

> 💡 **DeerFlow 的能力边界**：DeerFlow 擅长**文本内容生成、代码执行、网页开发、数据分析**。对于图像/视频生成，它通过**调用外部 API**（如硅基流动 FLUX、火山引擎等）或 **MCP 工具**来实现。如果当前环境未配置这些工具，DeerFlow 会生成详细的制作指南作为替代方案。

---

### 8.4 更多复杂任务示例

| 任务类型 | 示例任务 |
|---------|---------|
| **📊 数据分析** | 分析这份销售 CSV 数据，生成月趋势图、高亮异常月份、输出 Word 分析报告 |
| **💻 网页开发** | 做一个待办事项 Web 应用，支持拖拽排序、数据持久化到本地存储 |
| **📝 内容创作** | 写一篇 2000 字的科技评测文章，主题是"2026 年 AI Agent 发展趋势"，配 3 张示意图 |
| **🔍 深度研究** | 调研"AI Agent 在金融风控领域的落地现状"，输出包含数据图表的研究报告 |
| **📊 数据可视化** | 分析 2025 年全球票房数据，制作交互式图表和 PPT |
| **🎨 图文创作** | 制作一份"二十四节气"主题的图文专辑，每个节气配一张水墨风格插画和介绍文字 |

---

## 9. 常见问题与解决

### ❌ 问题 1：Docker Desktop 启动报错（Windows）

**症状**：Docker Desktop 打开后报错或闪退

**解决方法**：
1. 检查 Windows 功能：设置 → 应用 → 程序和功能 → 启用或关闭 Windows 功能
2. 确保勾选了 **"适用于 Linux 的 Windows 子系统 (WSL2)"**
3. PowerShell 管理员模式运行：
   ```powershell
   wsl --update
   ```
4. 重启电脑后重新打开 Docker Desktop

---

### ❌ 问题 2：启动后 502 Bad Gateway

**症状**：浏览器访问 `http://localhost:2026` 显示 502

**原因**：`config.yaml` 路径在 Windows 和 Linux 容器之间不兼容

**解决方法**：
打开 `docker/docker-compose.yml`，找到 `gateway` 服务，添加环境变量覆盖：

```yaml
services:
  gateway:
    environment:
      - DEER_FLOW_CONFIG_PATH=/app/backend/config.yaml
```

然后重启：
```bash
cd docker
docker compose --env-file ../.env up -d
```

---

### ❌ 问题 3：API Key 认证失败

**症状**：DeerFlow 提示"API Key 认证失败"或"网络错误"

**解决方法**：
1. 确认 `.env` 文件中的 API Key 没有多余的空格或引号
2. 确认 API Key 还有余额
3. 检查 `config.yaml` 中的 `base_url` 是否正确
4. 如果是国内平台，检查是否有网络访问限制

---

### ❌ 问题 4：镜像拉取太慢 / 超时

**解决方法**：
1. 确认已配置 Docker 镜像加速（见 §2.4）
2. 手动拉取基础镜像：
   ```bash
   docker pull nginx:alpine
   docker pull node:20-alpine
   docker pull python:3.11-slim
   ```

---

### ❌ 问题 5：前端编译报错（TypeScript / ESLint）

**解决方法**：
打开 `frontend/next.config.js`，添加忽略错误的配置：

```javascript
const config = {
  devIndicators: false,
  typescript: { ignoreBuildErrors: true },
  eslint: { ignoreDuringBuilds: true },
};
```

---

### ❌ 问题 6：联网搜索不工作

**症状**：DeerFlow 无法进行网络搜索

**原因**：Tavily 搜索需要 API Key，且在国内访问不稳定

**解决方法**：使用无需 API Key 的 DuckDuckGo 搜索：
1. 在 `backend/packages/harness/deerflow/community/` 目录下创建 `duckduckgo` 文件夹
2. 新建 `__init__.py` 和 `tools.py`，实现搜索工具
3. 修改 `config.yaml` 中 `web_search` 工具指向新工具
4. 重新构建镜像

---

## 📚 进阶资源

| 资源 | 链接 |
|------|------|
| 🌐 官方文档 | https://deerflow.tech/ |
| 💻 GitHub 仓库 | https://github.com/bytedance/deer-flow |
| 📖 架构解析（DeepWiki） | https://deepwiki.com/bytedance/deer-flow |
| 🐛 问题反馈 | GitHub Issues |
| 📦 硅基流动（多模型 API） | https://cloud.siliconflow.cn/ |
| 🏢 火山引擎 ARK | https://ark.console.volcengine.com/ |
| 🔮 智谱 AI | https://open.bigmodel.cn/ |

---

## 总结：安装流程一览

```
Windows 用户：
1️⃣ 安装 Docker Desktop（1次）
   ↓
2️⃣ 克隆 DeerFlow 代码（1次）
   ↓
3️⃣ 配置 API Key（1次）
   ↓
4️⃣ make docker-start 启动（每次使用）
   ↓
5️⃣ 打开 http://localhost:2026 开始使用 ✅

Linux 用户（Docker 方式）：
1️⃣ 安装 Docker + git（1次）
   ↓
2️⃣ git clone + 配置（1次）
   ↓
3️⃣ docker compose up 启动
   ↓
4️⃣ 浏览器访问 2026 端口 ✅

Linux 用户（原生方式）：
1️⃣ 安装 uv + nvm + pnpm（1次）
   ↓
2️⃣ git clone + uv sync（1次）
   ↓
3️⃣ ./bootstrap.sh -d 启动
   ↓
4️⃣ 浏览器访问 localhost:3000 ✅
```

