# 代码格式化

## 使用flake8 配置代码格式化

### 下载 flake8 插件

```
pip install yapf
```

### 修改vscode配置

```json
    "python.formatting.provider": "yapf",
    "python.linting.pylintEnabled": false,
    "python.linting.flake8Enabled": true,
```

### 配置项目 .flake8 文件

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

## **C/CPP命名规范**

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

## **格式**

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
