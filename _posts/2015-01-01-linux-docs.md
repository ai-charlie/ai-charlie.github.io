---
layout: post
author: charlie
tags: [docs]
title: linux-docs
---
# linux-docs

## 统计文件数量

统计当前文件夹下文件的个数，包括子文件夹里的

```
ls -lR|grep "^-"|wc -l
```

 统计文件夹下目录的个数，包括子文件夹里的

```
ls -lR|grep "^d"|wc -l
```

 统计当前文件夹下文件的个数

```
ls -l |grep "^-"|wc -l
```

 统计当前文件夹下目录的个数

```
ls -l |grep "^d"|wc -l
```

## 历史记录

> linux 保存操作记录以便复盘

使用上一个命令的最后一个参数

```

[Command] !$

```

查看历史记录

```

history | grep [keyword]

```

查看近期操作n个操作中关于ping的操作

```

history | grep ping |tail -n

```

查看历史记录及其时间

```

export HISTTIMEFORMAT='%F, %T'

```

删除全部历史记录

```

history -c

```

不过在大多数情况下只需要清除部分命令即可

方法1.直接修改历史记录存储文件

```

vi ~/.bash_history

```

删除不希望其他人看到的命令并保存文件退出

```

history -r

```

方法2.删除指定行的历史记录

```

history -d 指定行号

```

## Linux 修改文件夹权限

批量修改/file_or_folder_path文件夹及其子文件所有者

```bash
sudo chown  user:user /file_or_folder_path
```

批量修改/file_or_folder_path文件夹及其子文件权限

```bash
sudo chmod  /example_file_or_folder  /file_or_folder_path
```

常用命令

```bash
sudo chmod 777 file

# 查看当前路径文件属性
ls -l

# 查看指定文件、文件夹属性
ls -ld /file_or_folder_path

```

## 清除缓存
```
# 删除docker缓存
docker system prune #慎用
docker builder prune

# 删除tmp文件
# 以__pycache__为例
find . -name '__pycache__' -type d -exec rm -rf {} \;
# 删除vscode缓存
echo 3 > /proc/sys/vm/drop_caches
```

## 备份命令
### 基本语法
rsync的基本语法格式为：`rsync [OPTION]... SRC... [USER@]HOST:DEST`，其中：
- `SRC`是源文件或目录，可以是本地路径，也可以是远程路径。
- `[USER@]HOST:DEST`是目标位置，`USER`是远程主机的用户名，`HOST`是远程主机的地址，`DEST`是目标文件或目录在远程主机上的路径。

### 常用选项
- `-a`：归档模式，以递归方式传输文件，并保留文件的所有属性，相当于`-rlptgoD`。
- `-v`：详细模式，输出详细的同步信息，可查看同步过程中的文件传输情况。
- `-z`：传输时对文件进行压缩，减少传输的数据量，提高传输速度。
- `-r`：递归复制目录及其子目录和文件。
- `-P`：显示同步的进度，并支持断点续传。
- `--delete`：删除目标目录中源目录没有的文件，确保目标目录与源目录内容一致。

