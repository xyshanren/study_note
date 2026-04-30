## Ollama 加载 GGUF 格式模型主要有两种方式：**手动导入本地 GGUF 文件**和**直接加载在线 GGUF 模型**。以下是详细步骤：

### 一、手动导入本地 GGUF 文件（最常用）

#### 1. **准备 GGUF 模型文件**

- 从 Hugging Face、魔搭社区等平台下载 GGUF 格式模型文件
    
- 建议将文件放在专用目录（如 `D:\AI\Models`）
    

#### 2. **创建 Modelfile 配置文件**

在模型文件同级目录创建 `Modelfile`文件（无扩展名），内容如下：

```
# 基本格式（模型文件与Modelfile在同一目录）
FROM ./your-model.gguf

# 完整示例（可添加参数配置）
FROM ./qwen2.5-7b-instruct-q4_0.gguf
SYSTEM "你是一个专业的AI助手"
PARAMETER temperature 0.7
PARAMETER top_p 0.9
```

**路径说明**：

- 相对路径：`./model.gguf`（同一目录）
    
- 绝对路径：`FROM D:\AI\Models\qwen2.5-7b.gguf`（推荐使用绝对路径）
    

#### 3. **导入模型到 Ollama**

```
# 在Modelfile所在目录打开终端
ollama create mymodel -f ./Modelfile

# 或指定完整路径
ollama create qwen2.5-7b -f D:\AI\Models\Modelfile
```

- `mymodel`：自定义模型名称
    
- `-f`：指定 Modelfile 路径
    

#### 4. **验证与运行**

```
# 查看已导入模型
ollama list

# 运行模型
ollama run mymodel
```

### 二、直接加载在线 GGUF 模型（Ollama ≥ v0.3.12）

#### 1. **从 Hugging Face 加载**

```
# 基本格式
ollama run hf.co/{username}/{repository}

# 示例
ollama run hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF
ollama run hf.co/mlabonne/Meta-Llama-3.1-8B-Instruct-GGUF
```

#### 2. **从魔搭社区（ModelScope）加载**

```
# 基本格式
ollama run modelscope.cn/{username}/{model}

# 示例
ollama run modelscope.cn/Qwen/Qwen2.5-3B-Instruct-GGUF
ollama run modelscope.cn/second-state/gemma-2-2b-it-GGUF
```

#### 3. **指定量化版本**

```
# 添加量化标签
ollama run hf.co/bartowski/Llama-3.2-3B-Instruct-GGUF:IQ3_M
ollama run hf.co/bartowski/Llama-3.2-3B-Instruct-GGUF:Q8_0

# 或使用完整文件名
ollama run hf.co/bartowski/Llama-3.2-3B-Instruct-GGUF:Llama-3.2-3B-Instruct-IQ3_M.gguf
```

### 三、完整工作流程示例

### 场景：导入本地 Qwen2.5-7B GGUF 模型

```
# 1. 下载模型到 D:\AI\Models\qwen2.5-7b-instruct-q4_0.gguf

# 2. 创建 Modelfile（内容如下）
# FROM D:\AI\Models\qwen2.5-7b-instruct-q4_0.gguf
# SYSTEM "你是一个专业的AI助手"
# PARAMETER temperature 0.7

# 3. 导入模型
ollama create qwen2.5-7b -f D:\AI\Models\Modelfile

# 4. 运行测试
ollama run qwen2.5-7b
```

### 四、高级配置选项

### 1. **自定义模型参数**

```
# Modelfile 完整配置示例
FROM ./llama3.2-3b-instruct.gguf
SYSTEM "你是一个幽默的助手，用轻松的方式回答问题"
TEMPLATE "{{ .Prompt }}"
PARAMETER temperature 0.8
PARAMETER top_p 0.95
PARAMETER num_ctx 4096
PARAMETER top_k 40
```

#### 2. **查看模型配置**

```
# 查看已导入模型的Modelfile内容
ollama show mymodel --modelfile
```

#### 3. **复制与删除模型**

```
# 复制模型（创建新名称）
ollama cp mymodel mymodel-copy

# 删除模型
ollama rm mymodel
```

### 五、注意事项

1. **路径格式**：
    
    - Windows：使用双反斜杠 `D:\\AI\\Models\\model.gguf`或正斜杠 `D:/AI/Models/model.gguf`
        
    - 避免中文路径和特殊字符
        
    
2. **文件完整性**：
    
    - 确保 GGUF 文件完整下载
        
    - 可通过 `ollama create`时的进度条验证
        
    
3. **版本兼容性**：
    
    - 在线加载功能需要 Ollama ≥ v0.3.12
        
    - 检查版本：`ollama --version`
        
    
4. **存储位置**：
    
    - 导入的模型会存储在 Ollama 默认目录（可通过设置修改）
        
    - 查看存储：`ollama list`显示模型大小
        
    

### 六、故障排除

### 常见问题：

1. **"Error loading model"**：
    
    - 检查 GGUF 文件路径是否正确
        
    - 确认文件权限（Windows 管理员权限运行终端）
        
    
2. **"Model not found"**：
    
    - 确认 Modelfile 中 FROM 路径正确
        
    - 尝试使用绝对路径
        
    
3. **内存不足**：
    
    - 选择合适量化的模型（Q4_K_M 比 Q8_0 更小）
        
    - 关闭其他占用显存的程序
        
    

**推荐方案**：

- 网络良好时：直接使用 `ollama run hf.co/...`或 `ollama run modelscope.cn/...`
    
- 网络受限或需要特定版本：下载 GGUF 文件后手动导入
    
- 企业部署：使用 Modelfile 统一配置，便于版本管理
    

通过以上方法，您可以灵活加载任何 GGUF 格式模型到 Ollama 中运行。

## 如何使用已导入的模型

### 基本运行与对话

```
# 交互式对话
ollama run mymodel

# 单次提问
ollama run mymodel "介绍一下你自己"

# 多行输入（输入 /bye 退出）
ollama run mymodel
>>> 你好，请用Python写一个斐波那契数列函数
>>> 再写一个JavaScript版本
>>> /bye
# 或按 Ctrl+D（Linux/macOS）或 Ctrl+Z（Windows）

# 发送空提示词，然后立即退出
ollama run qwen3-vl:4b ""
# 这会启动模型，但立即返回空响应
```

### 使用自定义名称调用

```
# 你完全可以用有意义的名称
ollama run qwen2.5-7b-chat
ollama run llama-coding-assistant
ollama run deepseek-helper
```

### 使用 `ollama start`命令
**这是最直接的方法**，专门用于后台启动模型而不进入对话：

```powershell
# 启动模型到后台（不进入对话）
ollama start qwen3-vl:4b

# 验证模型是否在运行
ollama ps

# 输出示例：
# NAME           ID              SIZE      PROCESSOR          CONTEXT    UNTIL
# qwen3-vl:4b    1343d82ebee3    9.3 GB    36%/64% CPU/GPU    32768      5 minutes from now
```

**特点**：

- 模型加载到内存/GPU，但**不进入交互式对话**
    
- 可以通过 API 立即调用（`http://localhost:11434`）
    
- 默认保持 5 分钟，可通过 `--keep_alive`参数调整

### 启动 Ollama 服务（推荐用于API调用）

```powershell
# 启动 Ollama 服务（所有模型都可通过API调用）
ollama serve

# 或后台运行
ollama serve &
```

**服务模式的特点**：

- 启动 HTTP API 服务（端口 11434）
    
- 所有已下载模型都可通过 API 调用
    
- 模型按需加载，不立即占用内存
    
- 适合开发和生产环境

## 模型管理操作

### 查看和管理模型

```
# 查看所有模型
ollama list

# 查看模型详情
ollama show mymodel
ollama show mymodel --modelfile  # 查看配置文件

# 复制模型（创建别名）
ollama cp mymodel mymodel-backup
ollama cp mymodel qwen-chinese
```

### 删除模型

```
# 删除单个模型
ollama rm mymodel

# 删除所有模型
ollama rm $(ollama list | Select-Object -ExpandProperty NAME)
```

### 查看正在运行的模型

```powershell
# 查看哪些模型正在占用GPU内存
ollama ps

# 输出示例：
# NAME              ID              SIZE      MODIFIED      STATUS
# mymodel:latest    11d8c2483df9    2.5 GB    2 minutes ago running (GPU)
```

### 停止特定模型（释放GPU内存）

```powershell
# 停止单个模型
ollama stop mymodel

# 停止所有运行中的模型
ollama stop $(ollama ps --format json | ConvertFrom-Json).name
```

### 验证内存释放

```powershell
# 再次查看运行状态
ollama ps
# 应该显示为空或只有你需要的模型

# 查看GPU内存使用情况（需要NVIDIA显卡）
nvidia-smi
```


### ollama rmvs ollama stop的区别

| **<br><br>命令<br><br>** | **<br><br>作用<br><br>** | **<br><br>影响<br><br>** | **<br><br>恢复方式<br><br>** |
| ---------------------- | ---------------------- | ---------------------- | ------------------------ |
| **`ollama stop`**​     | 停止运行中的模型               | 释放GPU/内存               | 下次运行自动重新加载               |
| **`ollama rm`**​       | 删除模型文件                 | 释放磁盘空间                 | 需要重新下载 (`ollama pull`)   |

**关键区别**：

- `ollama stop`：临时释放GPU内存，模型文件还在
    
- `ollama rm`：永久删除模型文件，需要重新下载

## API调用（编程使用）

### 1. **REST API**

Ollama默认在 `http://127.0.0.1:11434`提供API：

```
# 生成文本
curl http://127.0.0.1:11434/api/generate -d '{
  "model": "mymodel",
  "prompt": "你好，请介绍一下AI的发展历史",
  "stream": false
}'

# 聊天接口
curl http://127.0.0.1:11434/api/chat -d '{
  "model": "mymodel",
  "messages": [
    {"role": "user", "content": "Python中如何读取文件？"}
  ]
}'
```

### 2. **Python调用示例**

```
import requests
import json

# 简单生成
response = requests.post('http://127.0.0.1:11434/api/generate',
    json={
        'model': 'mymodel',
        'prompt': '解释一下量子计算',
        'stream': False
    }
)
print(response.json()['response'])

# 流式输出
response = requests.post('http://127.0.0.1:11434/api/generate',
    json={
        'model': 'mymodel',
        'prompt': '写一首关于春天的诗',
        'stream': True
    },
    stream=True
)

for line in response.iter_lines():
    if line:
        data = json.loads(line.decode('utf-8'))
        print(data.get('response', ''), end='')
```

## 与其他工具集成

### 1. **OpenAI兼容接口**

Ollama提供OpenAI兼容的API，可在支持OpenAI的工具中使用：

```
from openai import OpenAI

client = OpenAI(
    base_url='http://127.0.0.1:11434/v1',
    api_key='ollama'  # 这里可以随便填
)

response = client.chat.completions.create(
    model="mymodel",
    messages=[
        {"role": "user", "content": "你好"}
    ]
)
print(response.choices[0].message.content)
```

### 2. **在应用中使用**

- **Chatbox/Open WebUI**：设置模型为 `mymodel`
    
- **Continue/VSCode插件**：配置Ollama并选择你的模型
    
- **LangChain**：使用 `Ollama`或 `ChatOllama`类
    

## 批量处理和自动化

### 1. **批量问答脚本**

```
# batch_ask.ps1
$questions = @(
    "什么是机器学习？",
    "Python的优缺点是什么？",
    "如何学习编程？"
)

foreach ($q in $questions) {
    Write-Host "问题: $q" -ForegroundColor Green
    ollama run mymodel $q
    Write-Host "-" * 50
}
```

### 2. **模型性能测试**

```
# 测试响应速度
Measure-Command { ollama run mymodel "你好" }

# 批量测试
$prompts = 1..10 | ForEach-Object { "这是第 $_ 个测试问题" }
foreach ($p in $prompts) {
    $time = (Measure-Command { ollama run mymodel $p }).TotalMilliseconds
    Write-Host "耗时: ${time}ms"
}
```

## 高级用法

### 1. **自定义运行参数**

```
# 临时修改参数运行
ollama run mymodel --temperature 0.8 --top-p 0.9 "请发挥创造力"

# 查看可用参数
ollama run mymodel --help
```

### 2. **模型组合与对比**

```
# 同时运行多个模型对比
$prompt = "用Python实现快速排序"
Write-Host "模型1回答:" -ForegroundColor Yellow
ollama run mymodel $prompt
Write-Host "`n模型2回答:" -ForegroundColor Cyan
ollama run llama3.2-3b $prompt
```

### 3. **创建带版本标签的模型**

```
# 创建时指定版本
ollama create mymodel:v1 -f ./Modelfile
ollama create mymodel:latest -f ./Modelfile
ollama create mymodel:chat -f ./chat.Modelfile

# 使用特定版本
ollama run mymodel:v1
ollama run mymodel:chat
```

## 实际工作流程示例

```
# 1. 导入多个专用模型
ollama create code-assistant -f D:\Models\codellama\Modelfile
ollama create chinese-chat -f D:\Models\qwen2.5\Modelfile
ollama create english-writer -f D:\Models\llama3\Modelfile

# 2. 根据不同场景使用
function Ask-CodeQuestion {
    param([string]$question)
    ollama run code-assistant $question
}

function Ask-GeneralQuestion {
    param([string]$question)
    ollama run chinese-chat $question
}

# 3. 使用
Ask-CodeQuestion "Python中如何实现单例模式？"
Ask-GeneralQuestion "推荐几本好书"
```

## 命名最佳实践

1. **描述性命名**：
    
    - `qwen2.5-7b-chat`
        
    - `llama3.2-3b-code`
        
    - `deepseek-r1-7b-math`
        
    
2. **带版本控制**：
    
    - `mymodel-v1.0`
        
    - `mymodel-v2.0-beta`
        
    - `mymodel:latest`(默认标签)
        
    
3. **功能区分**：
    
    - `assistant-zh`(中文助手)
        
    - `assistant-en`(英文助手)
        
    - `coder-python`(Python专家)
        
    

## Ollama 主要支持 **GGUF 格式**，但也可以通过转换或特定方式导入其他格式模型。以下是各种格式的导入方法：

### Ollama 支持的模型格式

|格式|是否原生支持|导入方式|备注|
|---|---|---|---|
|**GGUF**​|✅ 完全支持|直接导入|推荐格式，支持量化|
|**Safetensors**​|⚠️ 有限支持|直接导入或转换|需特定目录结构|
|**PyTorch (.bin)**​|❌ 不支持|需转换|必须转为GGUF|
|**ONNX**​|❌ 不支持|需转换|需转为GGUF|
|**适配器 (LoRA)**​|✅ 支持|结合基础模型导入|用于微调模型|

### Safetensors 格式导入（官方支持）

#### 方法1：直接导入完整模型

**要求**：模型目录需包含完整的配置文件

```
# 目录结构示例
D:\Models\qwen2.5-7b\
├── model.safetensors
├── config.json
├── tokenizer.json
└── tokenizer_config.json

# Modelfile 配置
FROM D:\Models\qwen2.5-7b
```

#### 方法2：导入微调适配器

适用于 LoRA 等微调权重：

```
# Modelfile 配置
FROM qwen2.5:7b  # 基础模型
ADAPTER ./lora-weights  # 适配器目录

# 或使用绝对路径
FROM qwen2.5:7b
ADAPTER D:\Models\lora-weights
```

### PyTorch (.bin) 格式转换导入

#### 步骤1：准备转换环境

```
# 1. 克隆 llama.cpp
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp

# 2. 安装依赖
pip install torch transformers safetensors

# 3. 准备模型文件
# 确保包含：pytorch_model.bin、config.json、tokenizer.json
```

#### 步骤2：转换为 GGUF 格式

```
# 转换为 FP16 格式
python convert_hf_to_gguf.py /path/to/your/model --outtype f16 --outfile model.f16.gguf

# 或转换为量化格式（节省空间）
python convert_hf_to_gguf.py /path/to/your/model --outtype q4_0 --outfile model.q4_0.gguf

# 支持的量化类型：
# q4_0, q4_1, q5_0, q5_1, q8_0, q2_K, q3_K_S, q3_K_M, q4_K_S, q4_K_M
```

#### 步骤3：导入到 Ollama

```
# 创建 Modelfile
FROM ./model.q4_0.gguf
SYSTEM "你的系统提示词"

# 导入模型
ollama create mymodel -f ./Modelfile
```

### 从 Hugging Face/ModelScope 下载转换

#### 方案1：直接下载 GGUF 版本

```
# 从 Hugging Face 下载 GGUF
ollama run hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF

# 从 ModelScope 下载 GGUF
ollama run modelscope.cn/Qwen/Qwen2.5-3B-Instruct-GGUF
```

#### 方案2：下载原始格式后转换

```
# 1. 下载原始模型（需 Git LFS）
git lfs install
git clone https://huggingface.co/Qwen/Qwen2.5-7B-Instruct

# 2. 转换为 GGUF
cd llama.cpp
python convert_hf_to_gguf.py ../Qwen2.5-7B-Instruct --outtype q4_K_M --outfile qwen2.5-7b-q4_K_M.gguf

# 3. 导入 Ollama
ollama create qwen2.5-7b -f ./Modelfile
```

### 完整工作流程示例

#### 案例：导入 Qwen2.5-7B PyTorch 模型

```
# 1. 下载模型
git clone https://huggingface.co/Qwen/Qwen2.5-7B-Instruct

# 2. 转换格式
cd llama.cpp
python convert_hf_to_gguf.py ../Qwen2.5-7B-Instruct \
  --outtype q4_K_M \
  --outfile qwen2.5-7b-q4_K_M.gguf

# 3. 创建 Modelfile
echo "FROM ./qwen2.5-7b-q4_K_M.gguf
SYSTEM \"你是Qwen2.5-7B模型\"
PARAMETER temperature 0.7
PARAMETER num_ctx 8192" > Modelfile

# 4. 导入 Ollama
ollama create qwen2.5-7b -f ./Modelfile

# 5. 测试运行
ollama run qwen2.5-7b "你好"
```

### 自动化脚本

### Windows PowerShell 转换脚本

```
# convert_model.ps1
param(
    [string]$ModelPath,
    [string]$OutputName,
    [string]$QuantType = "q4_K_M"
)

# 检查模型文件
if (-not (Test-Path "$ModelPath\pytorch_model.bin") -and 
    -not (Test-Path "$ModelPath\model.safetensors")) {
    Write-Host "错误：未找到模型文件" -ForegroundColor Red
    exit 1
}

# 转换模型
Write-Host "正在转换模型..." -ForegroundColor Yellow
python llama.cpp/convert_hf_to_gguf.py $ModelPath `
    --outtype $QuantType `
    --outfile "$OutputName.$QuantType.gguf"

# 创建 Modelfile
$ModelfileContent = @"
FROM ./$OutputName.$QuantType.gguf
SYSTEM "你是一个AI助手"
PARAMETER temperature 0.7
"@

$ModelfileContent | Out-File -FilePath "Modelfile" -Encoding UTF8

# 导入 Ollama
Write-Host "正在导入到 Ollama..." -ForegroundColor Green
ollama create $OutputName -f ./Modelfile

Write-Host "转换完成！使用命令运行：ollama run $OutputName" -ForegroundColor Cyan
```

### 注意事项

#### 1. **格式兼容性**

- **推荐使用 GGUF**：最稳定，支持量化
    
- **Safetensors 有限支持**：需完整配置文件
    
- **PyTorch .bin 需转换**：必须通过 llama.cpp 转换
    

#### 2. **量化建议**

```
# 不同硬件推荐
低配设备：q4_0, q4_K_S      # 最小内存占用
平衡性能：q4_K_M, q5_K_M    # 推荐选择
高精度：q8_0, f16          # 最大精度
```

#### 3. **常见错误解决**

```
# 错误：unsupported format
# 原因：格式不支持
# 解决：转换为 GGUF 格式

# 错误：missing config.json
# 原因：配置文件不全
# 解决：确保有 config.json、tokenizer.json

# 错误：out of memory
# 原因：内存不足
# 解决：使用更低量化的版本
```

### 最佳实践总结

1. **优先下载 GGUF 格式**：直接从 Hugging Face/ModelScope 下载 GGUF 版本
    
2. **转换工具准备**：安装好 llama.cpp 和 Python 依赖
    
3. **目录结构规范**：为每个模型创建独立目录
    
4. **量化策略**：根据硬件选择合适量化级别
    
5. **测试验证**：导入后立即测试运行
    

**推荐流程**：

1. 优先寻找 GGUF 格式直接下载
    
2. 若无 GGUF，下载 Safetensors 尝试直接导入
    
3. 若只有 PyTorch .bin，使用 llama.cpp 转换
    
4. 导入后测试模型功能是否正常
    

