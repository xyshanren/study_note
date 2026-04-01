> 更新时间：2026-03-31 | 版本：1.0

---

## 目录

1. [OpenMAIC 是什么](#1-openmaic-是什么)
2. [环境要求](#2-环境要求)
3. [安装方式](#3-安装方式)
4. [配置大语言模型 API](#4-配置大语言模型-api)
5. [启动与使用](#5-启动与使用)
6. [使用案例](#6-使用案例)
7. [常见问题](#7-常见问题)

---

## 1. OpenMAIC 是什么

**OpenMAIC** 是清华大学开源的 **AI 教学智能体运行框架**。

简单来说，它能帮你创建一个 AI 虚拟课堂，让 AI 扮演"教师"和"学生"，通过互动式对话来学习知识。

**它最擅长：**
- 🎓 互动式教学（AI 教师 + AI 学生角色扮演）
- 📚 自适应课程生成（根据用户水平自动调整难度）
- 🎥 多模态内容（语音、图像、视频、PPT）
- 💬 圆桌讨论模式（多 AI 角色辩论探讨）

**与 DeerFlow 的区别：**

| 特性 | OpenMAIC | DeerFlow |
|------|----------|----------|
| 核心场景 | 互动教学、课程生成 | 深度研究、报告生成 |
| 交互模式 | 角色扮演、多人对话 | 单 agent 任务执行 |
| 输出形式 | 课堂演示、PPT、视频 | 报告、数据分析 |

---

## 2. 环境要求

| 组件 | 版本要求 |
|------|---------|
| **Node.js** | ≥ 20 |
| **pnpm** | ≥ 10 |
| **Git** | 最新版 |

---

## 3. 安装方式

### 3.1 本地开发部署

```bash
git clone https://github.com/THU-MAIC/OpenMAIC.git
cd OpenMAIC
pnpm install
cp .env.example .env.local
# 编辑 .env.local（见 §4）
pnpm dev
# 访问 http://localhost:3000
```

### 3.2 Docker 部署

```bash
git clone https://github.com/THU-MAIC/OpenMAIC.git
cd OpenMAIC
cp .env.example .env.local
docker compose up --build
# 访问 http://localhost:3000
```

### 3.3 Vercel 一键部署

1. Fork https://github.com/THU-MAIC/OpenMAIC
2. 在 Vercel 导入仓库，配置环境变量
3. Deploy

---

## 4. 配置大语言模型 API

> ⚠️ **重要**：OpenMAIC 使用 `.env.local` 文件配置，**不是 config.yaml**！

### 4.1 支持的 LLM 提供商

| 提供商 | 变量前缀 | 默认 base_url |
|-------|---------|--------------|
| DeepSeek | `DEEPSEEK_` | `https://api.deepseek.com/v1` |
| 硅基流动 | `SILICONFLOW_` | `https://api.siliconflow.cn/v1` |
| 阿里 Qwen | `QWEN_` | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
| 智谱 GLM | `GLM_` | `https://open.bigmodel.cn/api/paas/v4` |
| MiniMax | `MINIMAX_` | `https://api.minimax.chat/v1` |
| 豆包 | `DOUBAO_` | `https://ark.cn-beijing.volces.com/api/v3` |

### 4.2 华为云 ModelArts 配置

```env
# 通过 DeepSeek 兼容接口配置华为云 MaaS
DEEPSEEK_API_KEY=your-maas-api-key
DEEPSEEK_BASE_URL=https://api.modelarts-maas.com/openai/v1
DEFAULT_MODEL=DeepSeek-V3
```

> ⚠️ 注意：华为云 MaaS 仅支持**西南-贵阳一**区域

### 4.3 硅基流动配置（推荐）

```env
SILICONFLOW_API_KEY=your-key
SILICONFLOW_BASE_URL=https://api.siliconflow.cn/v1
DEFAULT_MODEL=Qwen/Qwen2.5-72B-Instruct
```

### 4.4 多平台配置示例

```env
# 主模型
DEFAULT_MODEL=deepseek-chat

# DeepSeek 官方
DEEPSEEK_API_KEY=sk-xxxxx

# 硅基流动（GitCode 开源模型）
SILICONFLOW_API_KEY=sk-xxxxx

# 华为云 ModelArts（通过 DeepSeek 兼容）
DEEPSEEK_API_KEY=your-maas-key
DEEPSEEK_BASE_URL=https://api.modelarts-maas.com/openai/v1

# TTS（可选）
TTS_OPENAI_API_KEY=sk-xxxxx

# 搜索（可选）
TAVILY_API_KEY=tvly-xxxxx
```

---

## 5. 启动与使用

```bash
pnpm dev
# 访问 http://localhost:3000
```

---

## 6. 使用案例

### 案例 1：零基础学 Python
```
输入："用 30 分钟从零开始教我 Python 基础"
```

### 案例 2：技术选型讨论
```
输入："Vue vs React vs Next.js 各方发表观点"
```

### 案例 3：论文解读
```
输入："解读 DeepSeek-V3 论文的 MoE 架构创新"
```

---

## 7. 常见问题

- **启动失败**：`rm -rf node_modules .next && pnpm install`
- **API 报错**：检查 `.env.local` 配置
- **切换模型**：修改 `DEFAULT_MODEL` 后重启

---

## 📚 相关资源

- GitHub：https://github.com/THU-MAIC/OpenMAIC
- 在线演示：https://open.maic.chat

