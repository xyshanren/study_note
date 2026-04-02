Poetry 是一个现代化的 Python 依赖管理和打包工具，它集成了**依赖管理、虚拟环境管理、项目构建和发布**于一体，解决了传统 `pip + requirements.txt`方式的诸多痛点。

## 一、Poetry 的核心优势

|对比项|pip + virtualenv|Poetry|
|---|---|---|
|**依赖声明**​|requirements.txt|pyproject.toml|
|**依赖锁定**​|手动（需 pip freeze）|自动生成 poetry.lock|
|**虚拟环境**​|需手动创建与激活|自动创建与管理|
|**构建发布**​|需编写 setup.py|poetry build + poetry publish|
|**版本冲突处理**​|手动调试|内置版本解析器自动解决|
|**可重现环境**​|弱|强（依赖锁定）|

## 二、安装 Poetry

### 推荐安装方式（官方脚本）

```
# Linux/macOS
curl -sSL https://install.python-poetry.org | python3 -

# Windows (PowerShell)
(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | python -
```

### 验证安装

```
poetry --version
```

### 配置环境变量（如需要）

安装后，Poetry 通常位于 `~/.local/bin/`（Linux/macOS）或 `%APPDATA%\Python\Scripts`（Windows），确保该路径在系统 PATH 中。

## 三、基础配置

### 1. 推荐配置

```
# 让虚拟环境创建在项目目录内（推荐，方便管理）
poetry config virtualenvs.in-project true

# 启用并行安装（加速依赖安装）
poetry config installer.parallel true

# 查看所有配置
poetry config --list
```

### 2. 配置国内镜像源（加速下载）

```
# 添加清华源
poetry source add --priority=primary tsinghua https://pypi.tuna.tsinghua.edu.cn/simple/

# 或全局配置
poetry config repositories.tsinghua https://pypi.tuna.tsinghua.edu.cn/simple/
```

## 四、项目管理

### 1. 创建新项目

```
# 创建标准项目结构
poetry new my-project

# 生成的项目结构：
# my-project/
# ├── pyproject.toml    # 项目配置文件
# ├── README.md
# ├── my_project/       # 源码目录
# │   └── __init__.py
# └── tests/            # 测试目录
#     └── __init__.py
```

### 2. 初始化已有项目

```
cd /path/to/your/existing-project
poetry init
```

交互式引导配置项目信息（名称、版本、描述、作者等），生成 `pyproject.toml`文件。

## 五、依赖管理

### 1. 添加依赖

```
# 添加生产依赖
poetry add requests flask pandas

# 添加开发依赖（测试、代码检查等工具）
poetry add --dev pytest black mypy

# 添加指定版本
poetry add "django>=4.0,<5.0"
poetry add numpy@1.24.0
```

### 2. 安装所有依赖

```
# 安装所有依赖（包括开发依赖）
poetry install

# 仅安装生产依赖
poetry install --no-dev

# 仅安装开发依赖
poetry install --only dev
```

### 3. 更新依赖

```
# 更新所有依赖到最新兼容版本
poetry update

# 更新指定包
poetry update requests flask
```

### 4. 移除依赖

```
poetry remove package-name
```

### 5. 查看依赖信息

```
# 查看所有已安装依赖
poetry show

# 查看指定包信息
poetry show requests

# 查看依赖树
poetry show --tree
```

## 六、虚拟环境管理

### 1. 自动创建与激活

Poetry 会自动为每个项目创建独立的虚拟环境：

- 首次运行 `poetry install`或 `poetry add`时会自动创建虚拟环境
    
- 默认位置：`~/.cache/pypoetry/virtualenvs/`（如配置了 `virtualenvs.in-project true`则在项目目录 `.venv/`）
    

### 2. 手动管理虚拟环境

```
# 激活虚拟环境
poetry shell

# 退出虚拟环境
exit

# 在虚拟环境中执行命令（无需激活）
poetry run python script.py
poetry run pytest tests/

# 查看虚拟环境信息
poetry env info

# 列出所有虚拟环境
poetry env list

# 删除虚拟环境
poetry env remove python3.11
poetry env remove --all  # 删除所有
```

### 3. 指定 Python 版本

```
# 指定项目使用的 Python 版本
poetry env use python3.11
```

## 七、配置文件详解

`pyproject.toml`是 Poetry 的核心配置文件：

```
[tool.poetry]
name = "my-project"
version = "0.1.0"
description = "A short description"
authors = ["Your Name <you@example.com>"]
license = "MIT"
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.8"  # Python 版本要求
requests = "^2.31.0"
flask = "^3.0.0"

[tool.poetry.group.dev.dependencies]  # 开发依赖组
pytest = "^7.4.0"
black = "^23.0.0"
mypy = "^1.5.0"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

**版本约束语法**：

- `^2.31.0`：兼容 2.31.0 及以上，但小于 3.0.0
    
- `~2.31.0`：兼容 2.31.0 及以上，但小于 2.32.0
    
- `>=2.31.0,<3.0.0`：明确指定范围
    
- `*`：任何版本
    
- `2.31.0`：精确版本
    

## 八、构建与发布

### 1. 构建项目

```
# 构建包（生成 dist/ 目录）
poetry build

# 生成文件：
# dist/
# ├── my-project-0.1.0.tar.gz
# └── my-project-0.1.0-py3-none-any.whl
```

### 2. 发布到 PyPI

```
# 首次发布需要配置凭据
poetry config pypi-token.pypi your-token-here

# 发布到 PyPI
poetry publish

# 或构建并发布
poetry publish --build
```

### 3. 版本管理

```
# 查看当前版本
poetry version

# 更新版本
poetry version patch  # 0.1.0 → 0.1.1
poetry version minor  # 0.1.0 → 0.2.0
poetry version major  # 0.1.0 → 1.0.0

# 设置特定版本
poetry version 2.0.0
```

## 九、迁移现有项目

### 1. 从 requirements.txt 迁移

```
# 方法1：批量导入（简单但可能出错）
poetry add $(cat requirements.txt)

# 方法2：逐行导入（推荐）
while read requirement; do
    poetry add "${requirement}"
done < requirements.txt

# 方法3：手动添加重要依赖
poetry add django pandas numpy requests
```

### 2. 导出为 requirements.txt

```
# 导出生产依赖
poetry export -f requirements.txt --output requirements.txt --without-hashes

# 导出开发依赖
poetry export -f requirements.txt --output requirements-dev.txt --dev --without-hashes
```

## 十、常用命令速查表

|命令|功能|说明|
|---|---|---|
|`poetry new <name>`|创建新项目|生成标准项目结构|
|`poetry init`|初始化现有项目|交互式创建 pyproject.toml|
|`poetry add <pkg>`|添加依赖|自动更新 pyproject.toml 和 poetry.lock|
|`poetry add <pkg> --dev`|添加开发依赖|添加到 dev 组|
|`poetry remove <pkg>`|移除依赖|自动清理不再需要的依赖|
|`poetry install`|安装所有依赖|创建虚拟环境并安装|
|`poetry update`|更新依赖|更新到最新兼容版本|
|`poetry show`|查看依赖|显示已安装包信息|
|`poetry show --tree`|查看依赖树|显示依赖关系树|
|`poetry shell`|激活虚拟环境|进入虚拟环境 shell|
|`poetry run <cmd>`|在虚拟环境中运行命令|无需激活环境|
|`poetry env info`|查看环境信息|显示虚拟环境路径等|
|`poetry env list`|列出环境|显示所有虚拟环境|
|`poetry build`|构建项目|生成可发布包|
|`poetry publish`|发布到 PyPI|上传到包仓库|
|`poetry version`|版本管理|查看或更新项目版本|
|`poetry config --list`|查看配置|显示所有配置项|

## 十一、最佳实践

### 1. **项目结构规范**

```
my-project/
├── .venv/              # 虚拟环境（配置 virtualenvs.in-project true）
├── .gitignore          # 忽略虚拟环境等
├── pyproject.toml      # 项目配置
├── poetry.lock         # 依赖锁定文件（提交到版本控制）
├── README.md
├── src/                # 源码目录（可选）
│   └── my_package/
│       ├── __init__.py
│       └── main.py
├── tests/              # 测试目录
│   └── test_main.py
└── docs/               # 文档目录
```

### 2. **团队协作**

- 将 `pyproject.toml`和 `poetry.lock`都提交到版本控制
    
- `poetry.lock`确保所有开发者环境一致
    
- 新成员只需运行 `poetry install`即可获得完全相同的环境
    

### 3. **CI/CD 集成**

```
# GitHub Actions 示例
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install Poetry
        run: pip install poetry
      - name: Install dependencies
        run: poetry install --no-dev
      - name: Run tests
        run: poetry run pytest
```

### 4. **故障排除**

- **依赖解析超时**：配置国内镜像源加速
    
- **SecretStorage 错误**：配置 keyring 使用 null 后端
    
- **虚拟环境问题**：使用 `poetry env info`查看环境路径，确保配置正确
    

## 十二、与 Docker 集成

```
# Dockerfile 示例
FROM python:3.11-slim

# 安装 Poetry
RUN pip install poetry

# 配置 Poetry 不创建虚拟环境（在 Docker 中不需要）
RUN poetry config virtualenvs.create false

WORKDIR /app

# 复制依赖文件
COPY pyproject.toml poetry.lock ./

# 安装依赖（不安装开发依赖）
RUN poetry install --no-dev --no-interaction --no-ansi

# 复制应用代码
COPY . .

# 运行应用
CMD ["poetry", "run", "python", "app/main.py"]
```

## 总结

Poetry 作为现代 Python 项目管理的标准工具，通过统一的 `pyproject.toml`配置文件和自动化的依赖管理，极大地简化了 Python 项目的开发、测试和部署流程。对于新项目，强烈推荐使用 Poetry 替代传统的 `pip + virtualenv + requirements.txt`组合，特别是在团队协作和复杂依赖管理的场景下。

**核心工作流**：

1. `poetry new`或 `poetry init`创建项目
    
2. `poetry add`添加依赖
    
3. `poetry install`安装依赖并创建虚拟环境
    
4. `poetry run`或 `poetry shell`在虚拟环境中工作
    
5. `poetry build`和 `poetry publish`打包发布
    

通过遵循上述指南，你可以充分利用 Poetry 的强大功能，实现高效、可靠的 Python 项目管理。