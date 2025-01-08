---
layout: post
author: charlie
tags: [docs]
title: python-venv-docs
---
# 设置当前项目pypi镜像地址

阿里云镜像：http://mirrors.aliyun.com/pypi/simple/

清华大学镜像：https://pypi.tuna.tsinghua.edu.cn/simple/

豆瓣镜像：http://pypi.doubanio.com/simple/

中科大镜像：https://*pypi*.mirrors.*ustc*.edu.cn/simple/

就在你的当前项目下，创建一个 `pip.ini`文件，直接调用这个文件内容就OK了

```jsx
[global]
index-url = https://*pypi*.mirrors.*ustc*.edu.cn/simple/
[install]
trusted-host = https://*pypi*.mirrors.*ustc*.edu.cn
```

# 环境配置

- docker内cuda和本地的cuda版本不用一致，只依赖于驱动版本，139是3090的显卡，支持515.76的
- tensorrt 支持10.2 和 11+系列

# python虚拟环境

* venv
* conda

* docker

# 生成requirements

[https://github.com/bndr/pipreqs]()

Installation

```bash
pip install pipreqs
```

在当前目录中生成

```python
pipreqs .
```

指定编码方式

```python
pipreqs . --encoding=utf8 --force
```

保存文件名修改为pipreqs.txt

```
pipreqs . --savepath=pipreqs.txt
```
