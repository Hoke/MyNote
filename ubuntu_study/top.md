# top 命令

- [top 命令](#top-%e5%91%bd%e4%bb%a4)
  - [语法](#%e8%af%ad%e6%b3%95)
  - [选项](#%e9%80%89%e9%a1%b9)
  - [交互命令](#%e4%ba%a4%e4%ba%92%e5%91%bd%e4%bb%a4)
  - [多核 CPU 监控](#%e5%a4%9a%e6%a0%b8-cpu-%e7%9b%91%e6%8e%a7)
  - [示例解析](#%e7%a4%ba%e4%be%8b%e8%a7%a3%e6%9e%90)
    - [统计信息区](#%e7%bb%9f%e8%ae%a1%e4%bf%a1%e6%81%af%e5%8c%ba)
    - [进程信息区](#%e8%bf%9b%e7%a8%8b%e4%bf%a1%e6%81%af%e5%8c%ba)
  - [其他相关命令](#%e5%85%b6%e4%bb%96%e7%9b%b8%e5%85%b3%e5%91%bd%e4%bb%a4)

top 命令可以实时动态地查看系统的整体运行情况。

## 语法

`top 选项 参数`

## 选项

- `-b`: 以批处理模式操作，搭配 `n` 参数一起使用，可以用来将 top 的结果输出到文档
- `-c`: 显示完整命令
- `-d`: 改变屏幕刷新间隔时间，单位秒
- `-I`: 忽略失效过程
- `-s`: 保密模式，将交互式指令取消, 避免潜在的危机
- `-S`: 累积模式
- `-i 时间`: 设置间隔时间
- `-u/U 用户名`: 指定用户名
- `-p 进程号`: 指定进程
- `-n 次数`: 循环显示的次数，完成后将会退出 top

## 交互命令

在 top 命令执行过程中可以使用的一些交互命令。这些命令都是单字母的，如果在命令行中使用了 `-s` 选项， 其中一些命令可能会被屏蔽。

- `c`：切换显示命令名称和完整命令行
- `f/F`：从当前显示中添加或者删除项目
- `h/?`：显示帮助画面，给出一些简短的命令总结说明
- `i`：忽略闲置和僵死进程，这是一个开关式命令
- `k`：终止一个进程
- `l`：切换显示平均负载和启动时间信息
- `m`：切换显示内存信息
- `M`：根据驻留内存大小进行排序
- `o/O`：改变显示项目的顺序
- `P`：根据 CPU 使用百分比大小 (%CPU) 进行排序
- `q`：退出程序
- `r`：重新安排一个进程的优先级别。系统提示用户输入需要改变的进程 PID 以及需要设置的进程优先级值。输入一个正值将使优先级降低，反之则可以使该进程拥有更高的优先权。默认值是 10
- `s`：改变两次刷新之间的延迟时间(单位 s)，如果有小数，就换算成 ms。输入 0 则系统将不断刷新，默认值是 5s。需要注意如果设置时间太小，很可能会引起不断刷新，从而根本来不及看清显示的情况，而且系统负载也会大大增加
- `S`：切换到累计模式
- `t`：切换显示进程和 CPU 状态信息
- `T`：根据时间/累计时间 (TIME+) 进行排序
- `w`：将当前设置写入 `~/.toprc` 文件中。这是写 top 配置文件的推荐方法
- `Shift+M`: 可按内存占用情况 (%MEM) 进行排序

## 多核 CPU 监控

在 top 基本视图中，按键盘数字 `1`，可监控每个逻辑 CPU 的状况。

## 示例解析

```sh
kiki@ubuntu:~/qt-workspace/ffmpeg-3.4.1$ top

top - 10:43:40 up 21:02,  1 user,  load average: 0.67, 0.87, 0.90
Tasks: 313 total,   2 running, 214 sleeping,   0 stopped,   0 zombie
%Cpu(s):  8.6 us,  1.3 sy,  0.0 ni, 89.8 id,  0.0 wa,  0.0 hi,  0.3 si,  0.0 st
KiB Mem :  8144688 total,  1952452 free,  2211160 used,  3981076 buff/cache
KiB Swap:  4191228 total,  3902716 free,   288512 used.  5453920 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 76031 kiki      20   0 1872632 251988  38616 S  70.8  3.1  29:05.48 ffplay
  6491 kiki      20   0  691980  29688  16912 R   6.3  0.4  59:09.28 gnome-terminal-
  3586 kiki      20   0 1351776  59896  24988 S   5.0  0.7  53:45.88 compiz
  1533 root      20   0  574116  56816  14004 S   3.7  0.7  35:11.40 Xorg
 76179 kiki      20   0  239760  34608  12512 S   3.0  0.4   1:23.99 ffmpeg
  3480 kiki       9 -11  649816   9808   8112 S   2.3  0.1  21:38.05 pulseaudio
  1091 mongodb   20   0  716044   6868   3408 S   0.7  0.1   5:04.44 mongod
  1477 rabbitmq  20   0 5916188  16076   3944 S   0.7  0.2   8:46.64 beam.smp  
 78995 kiki      20   0   49028   3780   3000 R   0.7  0.0   0:00.08 top
     8 root      20   0       0      0      0 I   0.3  0.0   2:33.38 rcu_sched
  1588 redis     20   0   47204   2952   2148 S   0.3  0.0   1:07.34 redis-server
  1659 mysql     20   0 1246060  83864   6456 S   0.3  1.0   1:07.98 mysqld
     1 root      20   0  185140   4228   2832 S   0.0  0.1   0:17.55 systemd
# 查看单个用户进程使用情况
kiki@ubuntu:~/qt-workspace/ffmpeg-3.4.1$ top -u kiki

top - 13:56:54 up 1 day, 15 min,  1 user,  load average: 1.09, 0.90, 0.87
Tasks: 311 total,   1 running, 213 sleeping,   0 stopped,   0 zombie
%Cpu(s):  7.4 us,  1.4 sy,  0.0 ni, 90.7 id,  0.4 wa,  0.0 hi,  0.1 si,  0.0 st
KiB Mem :  8144688 total,  1714020 free,  2421044 used,  4009624 buff/cache
KiB Swap:  4191228 total,  3903228 free,   288000 used.  5231848 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 76031 kiki      20   0 2069240 501892  38616 S  33.3  6.2 150:29.10 ffplay
  3480 kiki       9 -11  649816   9808   8112 S   6.7  0.1  25:27.69 pulseaudio
 89040 kiki      20   0   49012   3796   3016 R   6.7  0.0   0:00.01 top
```

### 统计信息区

前五行是系统整体的统计信息：

- 第 1 行是任务队列信息，同 `uptime` 命令的执行结果。其内容如下：
  - `10:43:40`: 当前时间
  - `up 21:02`: 系统运行时间，格式为 `时:分`
  - `1 user`: 当前登录用户数
  - `load average: 0.67, 0.87, 0.90`: 系统负载，即任务队列的平均长度
    - 三个数值分别为 1 分钟、5 分钟、15 分钟前到现在的平均值
- 第 2-3 行为进程和 CPU 的信息。当有多个 CPU 时，这些内容可能会超过两行。内容如下：
  - `Tasks: 313 total`: 进程总数
  - `2 running`: 正在运行的进程数
  - `214 sleeping`: 睡眠的进程数
  - `0 stopped`: 停止的进程数
  - `0 zombie`: 僵尸进程数
  - `%Cpu(s):  8.6 us,  1.3 sy,  0.0 ni, 89.8 id,  0.0 wa,  0.0 hi,  0.3 si,  0.0 st`: CPU 占用百分比，依次是
    - 用户空间占用 CPU 百分比
    - 内核空间占用 CPU 百分比
    - 用户进程空间内改变过优先级的进程占用 CPU 百分比
    - 空闲 CPU 百分比
    - 等待 I/O 的 CPU 时间百分比
    - 服务硬件中断的 CPU 时间百分比
    - 服务软件中断的 CPU 时间百分比
    - vm 的 hypervisor 使用的 CPU 视角百分比
- 第 4-5 行为内存信息。内容如下：
  - `KiB Mem :  8144688 total,  1952452 free,  2211160 used,  3981076 buff/cache`
    - 物理内存：总量/空闲内存总量/使用的物理内存总量/用作内核缓存或缓冲的内存量
  - `KiB Swap:  4191228 total,  3902716 free,   288512 used.  5453920 avail Mem`
    - 虚拟内存：交换区总量/空闲交换区总量/使用的交换区总量/估算得到的用于启动一个新应用而不需要交换出可用的物理内存

### 进程信息区

统计信息区域的下方显示了各个进程的详细信息。

| 列 | 含义 |
| --- | --- |
| PID | 进程 ID |
| USER | 进程所有者的用户名 |
| PR | 优先级 |
| NI | nice 值。负值表示高优先级，正值表示低优先级 |
| VIRT | 进程使用的虚拟内存总量，单位 KB。VIRT= SWAP(进程使用的被换出的虚拟内存) + RES |
| RES | 进程使用的、未被换出的物理内存大小，单位 KB。RES = CODE(可执行代码占用的物理内存) + DATA(可执行代码以外的部分(数据段+栈)占用的物理内存) |
| SHR | 共享内存大小，单位 KB |
| S | 进程状态: D-不可中断的睡眠状态;R-运行;S-睡眠;T-跟踪/停止;t-跟踪过程中被 debugger 中断;Z-僵尸进程 |
| %CPU | 上次更新到现在的 CPU 时间占用百分比 |
| %MEM | 进程使用的物理内存百分比 |
| TIME+ | 进程使用的 CPU 时间总计，单位 1/100 秒 |
| COMMAND | 命令名/命令行 |

## 其他相关命令

- pmap: 根据进程查看进程相关信息占用的内存情况
- ps: 用于报告当前系统的进程状态
