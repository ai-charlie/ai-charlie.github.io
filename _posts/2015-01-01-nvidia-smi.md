# nvidia 常用命令

## 已安装nvidia驱动时查看gpu信息

### 查看gpu占用

nvidia-smi

```
Thu Nov 17 17:14:13 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 465.19.01    Driver Version: 465.19.01    CUDA Version: 11.3     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:21:00.0 Off |                  N/A |
|  0%   41C    P8    25W / 350W |      0MiB / 24265MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA GeForce ...  Off  | 00000000:4A:00.0 Off |                  N/A |
|  0%   43C    P8    26W / 350W |      0MiB / 24268MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

<detail>
<summary>
表格参数详解
</summary>

    GPU：本机中的GPU编号（有多块显卡的时候，从0开始编号）图上GPU的编号是：0
    Fan：风扇转速（0%-100%），N/A表示没有风扇
    Name：GPU类型，图上GPU的类型是：Tesla T4
    Temp：GPU的温度（GPU温度过高会导致GPU的频率下降）
    Perf：GPU的性能状态，从P0（最大性能）到P12（最小性能），图上是：P0
    Persistence-M：持续模式的状态，持续模式虽然耗能大，但是在新的GPU应用启动时花费的时间更少，图上显示的是：off
    Pwr：Usager/Cap：能耗表示，Usage：用了多少，Cap总共多少
    Bus-Id：GPU总线相关显示，domain：bus：device.function
    Disp.A：Display Active ，表示GPU的显示是否初始化
    Memory-Usage：显存使用率
    Volatile GPU-Util：GPU使用率
    Uncorr. ECC：关于ECC的东西，是否开启错误检查和纠正技术，0/disabled,1/enabled
    Compute M：计算模式，0/DEFAULT,1/EXCLUSIVE_PROCESS,2/PROHIBITED
    Processes：显示每个进程占用的显存使用率、进程号、占用的哪个GPU

</detail>

### 查看gpu占用

隔几秒刷新一下显存状态：nvidia-smi -l 秒数

```
nvidia-smi -l
```

### 查看当前所有GPU的信息，

```
nvidia-smi -q
```

### 查看指定GPU的信息 nvidia-smi -q -i GPU编号

```
nvidia-smi -q -i 0
```

## 未安装nvidia驱动时查看

```
lspci | grep -i vga
```

> 21:00.0 VGA compatible controller: NVIDIA Corporation Device **2204** (rev a1)

根据上述结果得到的**2204**在 http://pci-ids.ucw.cz/mods/PC/10de/1db6 查看GPU

## 将监控结果写入文件，并且指定写入文件的监控字段

nvidia-smi -l 1 --format=csv --filename=nvidia-smi-logs.csv --query-gpu=timestamp,name,index,utilization.gpu,memory.total,memory.used,power.draw

```
    -l：隔多久记录一次，命令中写的是1
    --format：结果记录文件格式是csv
    --filename: 结果记录文件的名字
    --query-gpu：记录哪些数据到csv文件
    timestamp：时间戳
    memory.total：显存大小
    memory.total：显存使用了多少
    utilization.gpu：GPU使用率
    power.draw：显存功耗，对应Pwr：Usage
```

如果想调整结果记录文件的字段，可以通过下面的命令查看对应的字段：

```
nvidia-smi --help-query-gpu
```

### nvidia-smi执行时间函数

```
/!bin/bash
# nvidia-smi-logs.sh
function  timeout(){
    timeStart=`date +%s`
    timeEnd=`date +%s`
#echo "starttime is  $timeStart"
#echo "endtiem is $timeEnd"
    i=$(($timeEnd - $timeStart))
    timeout=$1
    echo "timeout is :$timeout"

while ([ $i -ne $timeout ])
    do
       timeEnd=`date +%s`
       i=$(($timeEnd - $timeStart))
    done
}
nvidia-smi -l 1 --format=csv --filename=$1 --query-gpu=timestamp,name,index,utilization.gpu,memory.total,memory.used,power.draw &
echo "shell  PID: $$"
echo "nvidia-smi  PID: $!"
id=$!
echo $id
timeout $2
echo $id
kill -9 "$id"
```

bash nvidia-smi-logs.sh log文件路径 运行时间/s
bash nvidia-smi-logs.sh nvidia-smi.log 3600

参考链接：https://blog.51cto.com/u_15077560/3465364

## 查看cpu

```
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```

> 64  AMD Ryzen Threadripper 3970X 32-Core Processor

> 可以看到有64个逻辑CPU

```
16  Intel(R) Xeon(R) Gold 5118 CPU @ 2.30GHz
```
