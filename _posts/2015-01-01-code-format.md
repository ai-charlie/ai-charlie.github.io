## 提交前代码使用pre-commit统一格式化代码

`.pre-commit-config.yaml` 是用于配置 pre-commit 工具的文件。pre-commit 是一个在代码提交前运行一系列检查和格式化任务的工具，它有助于确保代码质量、遵循编码规范、避免一些常见错误进入版本控制系统，以下是对其常见内容和使用方法的详细介绍：

**一、文件结构**

`.pre-commit-config.yaml` 文件通常采用 YAML 格式，基本结构包含 `repos`（仓库）和 `hooks`（钩子）两大部分：

```yaml
repos:
  - repo: 仓库地址
    rev: 版本号或提交哈希
    hooks:
      - id: 钩子唯一标识
        name: 钩子名称
        description: 简要描述
        entrypoint: 可执行程序路径
        args: 执行参数
        language: 运行语言
        files: 作用的文件类型或路径
        exclude_files: 排除的文件类型或路径
```

**二、常用配置项说明**

1. `repos`：

   - `repo`：指定要获取钩子脚本的代码仓库地址，可以是公共的 GitHub、GitLab 等仓库，例如 `https://github.com/pre-commit/pre-commit-hooks`，这里存放了很多通用的预提交钩子脚本。
   - `rev`：对应的是该仓库的一个版本号或提交哈希，用于确定获取的钩子脚本版本，确保稳定性与可重复性，比如 `v2.0.0`。
2. `hooks`：

   - `id`：在当前配置文件中唯一标识一个钩子，便于管理与查找，不同的钩子有不同的 `id`，如 `flake8` 用于代码风格检查，`black` 用于代码格式化。
   - `name`：给钩子一个人类可读的名称，方便理解其作用，像 `Check code style with flake8` 这样直观表述。
   - `description`：简短介绍钩子功能，辅助用户了解，例如 `Ensures code conforms to PEP8 style guidelines` 说明 `flake8` 钩子确保代码符合 PEP8 风格规范。
   - `entrypoint`：指向执行具体任务的可执行程序路径，如果是 Python 脚本，通常是 `python -m 模块名` 形式，比如 `python -m flake8`。
   - `args`：传递给 `entrypoint` 的执行参数，可定制化钩子的具体行为，如 `flake8` 可能有 `--max-line-length=120` 来限定每行代码长度。
   - `language`：钩子运行所使用的语言，常见的有 `python`、`shell` 等，这决定了 `pre-commit` 在执行时需要准备的运行环境。
   - `files`：明确该钩子作用的文件类型或路径范围，可用通配符，如 `*.py` 表示作用于所有 Python 文件，`src/*.java` 表示仅作用于 `src` 目录下的 Java 文件。
   - `exclude_files`：与 `files` 相反，指定不被该钩子处理的文件，同样能用通配符，如 `test/*` 排除测试目录下的文件不做处理。

**三、示例配置**

以下是一个简单但实用的 `.pre-commit-config.yaml` 配置示例，用于 Python 项目：

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
        name: Remove trailing whitespace
        description: Removes trailing whitespace from files.
        entrypoint: python -m pre-commit-hooks.trailing_whitespace
        files: "*.py"
      - id: end-of-file-fixer
        name: Fix end of file newline
        description: Ensures files end with a newline.
        entrypoint: python -m pre-commit-hooks.end_of_file_fixer
        files: "*.py"
  - repo: https://github.com/psf/black
    rev: v23.7.0
    hooks:
      - id: black
        name: Format Python code
        description: Formats Python code using Black.
        entrypoint: python -m black
        args: ["--line-length", "120"]
        files: "*.py"
  - repo: https://github.com/pyer/fmt
    rev: v0.1.0
    hooks:
        - id: fmt
        name: Format Python code
        description: Formats Python code using fmt.
        entrypoint: python -m fmt
        args: ["--style", "pep8"]
        files: "*.py"
```

在这个示例中：

- 从 `pre-commit/pre-commit-hooks` 仓库获取了 `trailing-whitespace` 和 `end-of-file-fixer` 两个钩子，分别用于去除 Python 文件末尾的空格和确保文件以换行符结尾。
- 从 `psf/black` 仓库引入 `black` 钩子，按照每行 120 字的标准格式化 Python 代码。
- 从 `pyer/fmt` 仓库拿来 `fmt` 钩子，依据 PEP8 风格格式化 Python 代码。

**四、使用步骤**

1. 安装 pre-commit：在项目根目录下，运行 `pip install pre-commit`。
2. 配置 `.pre-commit-config.yaml`：按照上述方法编写或修改配置文件，适配项目需求。
3. 安装钩子：在项目根目录下执行 `pre-commit install`，这会在本地 `.git` 目录下创建一些脚本链接，用于在代码提交前触发相应检查与格式化操作。
4. 日常使用：之后每次在项目根目录下执行 `git commit` 时，`pre-commit` 工具就会按照配置文件中的要求，对即将提交的代码进行检查、格式化等操作。如果代码不符合要求，提交将会被阻止，并提示需要修正的问题，直到代码通过所有预提交检查，才能成功提交。

通过合理配置 `.pre-commit-config.yaml` 文件并有效利用 `pre-commit` 工具，可以大幅提升代码质量，减少低级错误进入版本控制系统，让团队开发更加高效、规范。

## 使用flake8 配置代码格式化

下载 flake8 插件

```
pip install yapf
```

修改vscode配置

```json
    "python.formatting.provider": "yapf",
    "python.linting.pylintEnabled": false,
    "python.linting.flake8Enabled": true,
```

配置项目 .flake8 文件

```flake8
[flake8]
max-line-length = 80
; Black violates PEP8
based_on_style = pep8
select = C,E,F,W,B
ignore = F401, W291, E501, W293, F541, F841, E126, W504, E402
spaces_before_comment = 4
split_before_logical_operator = true
exclude=.git, .pytest_cache, .tox, .vscode, venv, .venv, .idea, __pycache__, datasets, data
```

# C代码格式化

## C/CPP命名规范

**1. 文件名**
  小写，单词之间用_分隔。 例如 rtp_session.h, rtp_session.cpp

**2. 类名:**

  C打头，按大驼峰的方式命名。 例如 CRtpSession

  不要使用typedef struct/class

**3. 接口命名：**
  I打头，按大驼峰的方式命名。 IRtpSession

**4. 函数命名：**
  按大驼峰的方式命名。CRtpSession::OnVideoReceive

**5. enum命名**
  enum 以大驼峰命名， enum里的每一项以e开头。
  enum MediaType
  {
   eVideoMedia = 0,
   eAudioMedia = 1,
  }

**6. 变量命名**
  变量统一按小驼峰的方式命名。

  成员变量：
  m_打头，按小驼峰的方式命名。 m_rtpSession;

  局部变量：
  rtpSession;

  静态变量：
  s_打头, s_configInstance;

  全局变量：
  g_configInstance;

  指针命名：
  裸指针在名字后面加Ptr。 m_rtpSessionPtr
  共享指针在名字后面加Ref. m_rtpSessionRef

**7.参数命名**

  a_打头，按小驼峰的方式命名。a_rtpPacket

**8.常量命名**
 全部大写,单词之间用_分隔。
 const int REPORT_INTERVAL = 2000;
 #define REPORT_INTERVAL 2000

## 格式

1. 不使用tab，用4个空格来替换tab键

  visual studio可以通过菜单：Tools->Options->TextEditor->C/C++->Tab 进行设置

2. 复杂的条件表达式，每行一个条件，条件操作符放在行尾。
   if (a == b &&
   c != d &&
   ret == 0)
3. if/for 后面统一要有大括号.

## Best Practice

1. 杜绝重复代码
2. 初始化成员变量和局部变量。
3. 函数尽量简短。行数不要超过200行. （可以在一屏内显示）
4. 函数参数不要过多。（5个）
5. 函数参数尽量是传引用。可以用const修饰的参数尽量用const。
6. 可以用const修饰的函数，要尽量加上const。
7. 类的成员变量都应该是保护或私有成员的, 通过getter和setter来访问。纯粹用来传递数据的struct可以例外。
8. 避免过多过深的if/else。 当某个类中，if/else越来越多时，考虑重构。
9. 在头文件中使用前置声明，在cpp中包含头文件来减少编译依赖，提升编译速度。
10. 类声明和实现分开。
11. 设计类的接口。 通过接口指针/引用来访问某个具体的对象。
12. 设计工厂类或工厂方法来创建具体的对象，返回接口指针.
13. 少用实现继承，多用接口包含。

# Python代码更优雅的格式和风格建议

### 遵循PEP8风格指南

- **缩进**：使用4个空格进行缩进，不使用制表符。这能保证代码在不同的编辑器和环境中都有一致的外观。
- **行长度**：尽量将每行代码的长度控制在79个字符以内，如果一行代码过长，可以使用括号、反斜杠等方式进行换行。
- **空格使用**：在运算符两侧、函数参数之间等适当位置添加空格，增强代码的可读性。

### 函数和类的定义

- **函数**：函数定义之间空两行，函数内部的逻辑块之间可以空一行。函数的参数列表如果很长，可进行换行，每个参数独占一行，并适当缩进。
- **类**：类定义之间空两行，类中的方法之间空一行。类名使用大写字母开头的驼峰命名法，函数名和变量名使用小写字母加下划线的命名法。

### 代码布局和结构

- **模块导入**：将模块导入放在文件的开头，按照标准库、第三方库、自定义库的顺序进行分组，每组之间空一行。
- **代码块和注释**：在代码中使用空行将不同的逻辑代码块分隔开，使代码结构更清晰。同时，为代码添加必要的注释，解释代码的功能、用途和重要的逻辑。

### 表达式和语句

- **避免过长的表达式**：如果表达式过于复杂，可将其分解为多个中间变量，提高代码的可读性。
- **合理使用括号**：在需要明确表达式运算顺序的地方，使用括号，避免因运算符优先级问题导致代码逻辑错误。
- **条件语句和循环语句**：条件语句和循环语句的条件表达式如果很长，可进行换行，并添加适当的缩进和注释。

以下是一个遵循上述建议的优雅Python代码示例：

```python
import os
import sys

# 导入第三方库
import requests

# 导入自定义库
from utils import data_processing, data_visualization


# 定义一个函数，用于获取数据
def get_data(url):
    try:
        response = requests.get(url)
        response.raise_for_status()
        return response.json()
    except requests.RequestException as e:
        print(f"Error fetching data: {e}")
        sys.exit(1)


# 定义一个类，用于处理和分析数据
class DataAnalyzer:
    def __init__(self, data):
        self.data = data

    def process_data(self):
        # 调用外部函数进行数据处理
        processed_data = data_processing(self.data)
        return processed_data

    def analyze_data(self):
        processed_data = self.process_data()
        # 进行一些数据分析操作
        result = sum(processed_data) / len(processed_data)
        return result


if __name__ == "__main__":
    # 设置数据的URL
    data_url = "https://example.com/api/data"

    # 获取数据
    data = get_data(data_url)

    # 创建DataAnalyzer实例并进行数据分析
    analyzer = DataAnalyzer(data)
    analysis_result = analyzer.analyze_data()

    # 可视化数据
    data_visualization(analysis_result)
```
