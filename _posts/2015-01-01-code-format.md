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
