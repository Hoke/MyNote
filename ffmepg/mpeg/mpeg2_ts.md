# MPEG2-TS

- [MPEG2-TS](#mpeg2-ts)
  - [缩略词](#%e7%bc%a9%e7%95%a5%e8%af%8d)
    - [TS packet](#ts-packet)
  - [前言](#%e5%89%8d%e8%a8%80)
  - [概述](#%e6%a6%82%e8%bf%b0)
  - [元素](#%e5%85%83%e7%b4%a0)
    - [包 packet](#%e5%8c%85-packet)
      - [部分 TS 包格式](#%e9%83%a8%e5%88%86-ts-%e5%8c%85%e6%a0%bc%e5%bc%8f)
      - [适配域格式](#%e9%80%82%e9%85%8d%e5%9f%9f%e6%a0%bc%e5%bc%8f)
      - [适配域扩展格式](#%e9%80%82%e9%85%8d%e5%9f%9f%e6%89%a9%e5%b1%95%e6%a0%bc%e5%bc%8f)
    - [包标识符 PID](#%e5%8c%85%e6%a0%87%e8%af%86%e7%ac%a6-pid)
    - [节目 program](#%e8%8a%82%e7%9b%ae-program)
    - [节目专用信息 PSI](#%e8%8a%82%e7%9b%ae%e4%b8%93%e7%94%a8%e4%bf%a1%e6%81%af-psi)
      - [表的分节 table section](#%e8%a1%a8%e7%9a%84%e5%88%86%e8%8a%82-table-section)
        - [pointer](#pointer)
      - [描述子 descriptor](#%e6%8f%8f%e8%bf%b0%e5%ad%90-descriptor)
      - [节目关联表 PAT](#%e8%8a%82%e7%9b%ae%e5%85%b3%e8%81%94%e8%a1%a8-pat)
      - [节目映射表 PMT](#%e8%8a%82%e7%9b%ae%e6%98%a0%e5%b0%84%e8%a1%a8-pmt)
    - [PCR](#pcr)
    - [空包 null packet](#%e7%a9%ba%e5%8c%85-null-packet)
  - [参考](#%e5%8f%82%e8%80%83)

## 缩略词

```text
ES
  elementary stream, 基本流。
PES
  packetized elementary stream, 分组化基本流。PES 承载 ES，或 非 MPEG 的编码流。
EPG
  electronic program guide, 电子节目指南(节目指南、电子节目表或者电子节目导览)，是一种电视节目单，通常伴随数字电视信号或数字广播信号传送。这些信号可以通过有线电视、卫星电视、或地面电视被接收。
  通过在接收设备上浏览 EPG，典型的接收设备包括电视机及机顶盒，用户可以看到当前节目及未来节目的更多的信息。通常 EPG 是图形界面，包括节目名称、频道、播出时间等，并可以选择观看每个节目的更详细的内容。广播节目可能包含一个简单的基于文字的显示。
CBR
  constant bitrate, 固定码率。这是一个用来形容通信服务质量 (quality of service, QoS) 的术语。相对应的词是可变码率或可变比特率 (variable bit rate, VBR)。
TS packet
  TS 包。是多路复用的基本单位。多个不同的 ES 的内容会分别被封装在 TS packet 中通过同一 TS 传输。
PID
  packet identifier, 包标识符。
MPTS
  multi-program transport stream, 多节目传输流。
SPTS
  single-program transport stream, 单一节目传输流。
PSI
  program specific information, 节目专用信息。是一种有关节目程序与 M2T 的元数据，在数位电视系统中，指出节目的特别信息，终端机器(如机顶盒)只有通过这些信息才能搜索出节目来。
  一般包括：PAT(program association table, 节目关联表)、PMT(program map table, 节目映射表)、NIT(network information table, 网络信息表)、CAT(condition access table, 条件接收表)。
PAT
  program association table, 节目关联表。是数字电视系统中节目指示的根节点。其包标识符 (PID) 为 0。终端设备(如机顶盒)搜索节目时最先都是从这张表开始搜索的。从 PAT 中解析出  PMT，再从 PMT 解析出基本元素(如视频、音频、数据等)的 PID 及节目号、再根据节目从节目业务描述表(service description table, SDT)中搜索出节目名称。CAT 相关表是从 PMT 中得到。
PMT
  program map table, 节目映射表。
NIT
  network information table, 网络信息表。是数字电视系统中用于传送网络名称、传送节目的通道(频点、符号率、调制方式、纠错编码等)等网信息的一张表格。最重要的功能就是终端设备(如机顶盒)可以通过这张表自动搜索出网络中的所有节目来，而不用一个一个地输入所有节目的参数，对于有一、二个节目的网络来说这特别有用。节目有变化时也可以通过这张表的版本变化加以通知终端设备(如机顶盒)自动更新节目列表。
CAT
  condition access table, 条件接收表。用于节目的加密与解密。对应 PID 为 0x0001。
PCR
  program clock reference, 节目时钟参考。
STC
  system time clock, 系统校时时钟。
PTS
  presentation time stamp, 显示时间戳。
TDM
  time-division multiplexing, 时分多路复用。是一种数字或者模拟(较罕见)的多路复用技术。使用这种技术，两个以上的信号或数据流可以同时在一条通信线路上传输，其表现为同一通信信道的子信道。但在物理上来看，信号还是轮流占用物理信道的。
  时间域被分成周期循环的一些小段，每段时间长度是固定的，每个时段用来传输一个子信道。例如子信道 1 的采样(可能是字节或者是数据块)使用时间段 1，子信道 2 使用时间段 2 等等。一个 TDM 的帧包含了一个子信道的一个时间段，当最后一个子信道传输完毕，这样的过程将会再重复来传输新的帧，也就是下个信号片段。
jitter
  抖动，又可称为时基误差，指的是电子学和电信领域中，周期信号与真实周期之间的差异，通常是相当于参考时钟信号而言。
ECM
  entitlement control message, 授权信息控制。
TSDT
  transport stream description table, 传输流描述表。
TEI
  transport error indicator, 传输错误指示位。
PUSI
  payload unit start indicator, 载荷单元开始指示位。
TSC
  Transport scrambling control, 传输加扰控制。
LTW
  Legal time window, 合法时间窗口
FEC
  forward error correction, 前向纠错/前向纠错码，是增加数据通讯可信度的方法。在单向通讯信道中，一旦错误被发现，其接收器将无权再请求传输。FEC 是利用数据进行传输冗余信息的方法，当传输中出现错误，将允许接收器再建数据。
```

### TS packet

TS packet 是 TS 的基本传输单位。在 TS 范围以外并不存在描述一个 TS 的属性的全局性的描述体。TS 自身的全部信息仅由其自身描述，TS 仅由一系列的 TS packet 构成。

每个 TS packet 以固定的同步字节起始，这个同步字节的值为 0x47，它也是 TS packet 头的一部分。TS packet 的必选头长度为 4 字节，其后为可选部分，为载荷或适配域。TS packet 的头部固定以大端序读写。TS packet 长度为 188 字节。

## 前言

MPEG2-TS 传输串流，MPEG-2 Transport Stream，又称 MPEG-TS、MTS、TS。是一种用于音频、视频与程序系统信息协议(program and system information protocol, PSIP) 数据传输和存储的标准视频格式。它被用于广播系统，如 DVB、ATSC 和 IPTV。

TS 指定一种封装格式，封装了 PES，包括错误纠正和同步讯号特性以便在携带流的通信通道变差时维护传输的完整性。

TS 和类似命名 MPEG PS (program stream) 在一些重要方面是不同的：PS 用于比较可靠的介质，比如磁盘(像 DVD )，但是 TS 用于相对不可靠的传输，也就是地面和卫星广播。将来，一个 TS 可能携带多个节目 (program)。

TS 定义于 MPEG-2 第一部分：系统，正式的称为 ISO/IEC 标准 13818-1 或 ITU-T Rec. H.222.0。

## 概述

一个 TS 封装了一些其他子流，通常是 PES。PES 反过来使用 MPEG 编解码器或其他一些非 MPEG 编解码器(比如 AC3 或 DTS 音频，和 MJPEG 或 JPEG 2000 视频)封装了 ES、字幕的文本和图像、标识 ES 的表，甚至是类似 EPG 的广播信息。许多 ES 通常被混合在一起，比如一些不同的电视频道，或一部电影的许多角度。

每个 ES 被分割成(至多) 188 字节的段且互相交织；由于包尺寸较小，比起 PS 和其他常见格式比如  AVI、MOV/MP4 和 MKV 通常将每个帧封在一个包中，TS 交织的流具有更小的延迟和更高的容错性。这对于视频会议尤其重要，因为大的帧会导致不能接受的音频延迟。

TS 倾向于使用固定码流广播，当不存在足够大小的数据时，使用填充字节。

日本地面数字电视 ISDB-T 在 TS、PES、ES 层次上，使用的 MPEG-2 TS 协议具体标准

- ES/Table 协议栈：
  - ES: MPEG-2 视频 (H.262) (ISO/IEC 13818-2), MPEG-2 AAC (ISO/IEC 13818-7), 数据服务(独立 PES 模式) (ARIB STD-B24), TRMP(加密) (ARIB STD-B25)
  - Table: PSI/SI (ISO/IEC 13818-1、ARIB STD-B10)
  - 数据服务(轮播模式) (ARIB STD-B24)
- PES/Section 协议栈：
  - PES: MPEG-2 PES (ISO/IEC 13818-1、ARIB STD-B32)
  - Section: 表的分节(Table Section) (ISO/IEC 13818-1、ARIB STD-B32)
- TS  协议栈: MPEG-2 TS (ISO/IEC 13818-1)

## 元素

### 包 packet

一个网络包是 TS 中的基础数据单位，而且一个 TS 只是一系列包。每个包以一个同步字节和一个头开始，其后可以有可选的其他头；包的剩余部分由载荷组成。所有头部以大端序读写。包的尺寸是 188 字节，但是通信中介可能增加其他信息。起初选择 188 字节的包尺寸是用于兼容异步传输模式 (asynchronous transfer mode,  ATM) 系统。

#### 部分 TS 包格式

TS 头(4 字节)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| 同步字节 | 8 | 0xff000000 | 位元 0x47 (ASCII 码 'G') |
| 传输错误指示位 TEI | 1 | 0x800000 | 当解调器不能从 FEC 数据修改错误时，设置为 1 表示这个包是错的 |
| 载荷单元开始指示位 | 1 | 0x400000 | 当紧跟着头域是一个 PES、PSI 或 DVB-MIP 起始包时设置为 1 |
| 传输优先级 | 1 | 0x200000 | 当前包比其他有相同 PID 的包优先级更高时设置为 1 |
| PID | 13 | 0x1fff00 | 包标识符，描述载荷数据 |
| 传输加扰控制 | 2 | 0xc0 | '00'-未加密；仅用于DVB-CSA 和 ATSC DCS：'01'-预留;'10'-以偶数密钥加密;'11'-以奇数密钥加密 |
| 适配域存在标志 | 2 | 0x30 | 00-预留;01-只有载荷;10-只有适配域;11-适配器域后是载荷 |
| 连续性计数器 | 4 | 0xf | 每个流(除了 PID 8191)载荷包的序列号(0x00-0x0F)，循环。只有当载荷标记设置时，每个 PID 加 1。用于检查同一个 PID 的 TS packet 的连续性 |

可选域：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| 适配域 | 改变 | - | 适配域存在标志为 10 或 11 时才会存在 |
| 载荷 | 改变 | - | 适配域存在标志为 01 或 11 时才会存在。载荷可以是 PES 包、PSI 或其他数据 |

#### 适配域格式

(2 字节)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| 适配域长度 | 8 | - | 这个字节之后紧跟的适配域的字节数 |
| 不连续指示位 TEI | 1 | 0x80 | 如果关于连续性计数器或 PCR，当前 TS 包处于不连续状态，设置为 1 |
| 随机访问指示位 | 1 | 0x40 | 从这个点开始可以不出错解码此流设置为 1 |
| ES 优先级指示位 | 1 | 0x20 | 此流被认为是“高优先级”时设置为 1 |
| PCR 标识 | 1 | 0x10 | 存在 PCR 域设置为 1 |
| OPCR 标识 | 1 | 0x08 | 存在 OPCR 域设置为 1 |
| 接续点标识 | 1 | 0x04 | 存在接续倒数计时器设置为 1 |
| 传输私有数据标识 | 1 | 0x02 | 存在传输私有数据设置为 1 |
| 适配域扩展标识 | 1 | 0x01 | 存在适配域扩展数据设置为 1 |

可选域(14 字节+可变长度)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| PCR | 33+6+9=48 | - | 节目时钟参考，存储为 33 比特基 + 6 比特预留 + 9 比特扩展。值的计算公式：pcr_base*300+pcr_ext |
| OPCR | 33+6+9=48 | - | 原节目时钟参考。在复制 TS 时有用 |
| 接续倒数计时器 | 8 | - | 指示从当前包起多少个 TS 包之后出现接续点(有符号二进制补码；取值可为负) |
| 传输私有数据长度 | 8 | - | 下一个域的长度 |
| 传输私有数据 | 可变 | - | 私有数据 |
| 适配域扩展 | 可变 | - | 参考下面 |
| 填充字节 | 可变 | - | 总是 0xFF |

#### 适配域扩展格式

(2 字节)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| 适配域扩展长度 | 8 | 0xff00 | 头的长度 |
| LTW 标记 | 1 | 0x80 | - |
| piecewise rate 标记 | 1 | 0x40 | - |
| seamless splice 标记 | 1 | 0x20 | - |
| 预留 | 5 | 0x001f | - |

可选域：

LTW 标记设置( 2 字节)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| LTW  有效性标记 | 1 | 0x8000 | - |
| LTW  偏移量 | 15 | 0x7fff | 当包可能丢失时，转播商用于确定缓存状态的额外信息 |

piecewise rate 标记设置( 3 字节)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| 预留 | 2 | 0xc00000 | - |
| piecewise rate | 22 | 0x3fffff | 流率，按照 1888 字节包测量，用于确定 LTW 的结束时间 |

seamless splice 标记设置( 5 字节)：

| 名称 | 比特数 | 位掩码(大端) | 描述 |
| --- | --- | --- | --- |
| splice 类型 | 4 | 0xf000000000 | 指示 H.262 splice 的参数 |
| DTS next access unit | 36 | 0x0efffefffe | splice 点的 PES DTS。分成多个域。1 比特标记位(0x1) + 15 比特 + 1 比特标记位 + 15 比特 + 1 比特标记位 = 33 比特 |

### 包标识符 PID

TS 中的每个表或 ES 使用一个 13 比特的包标识符 (PID) 标识。一个解复用器通过查找相同的 PID 包从分割的 TS 中抽出 ES。在大多数应用中，使用时分复用确定一个特定的 PID 在 TS 中出现的频率。

使用的 PID：

| 十进制 | 十六进制 | 描述 |
| --- | --- | --- |
| 0 | 0x0000 | PAT 包含一个目录，列举所有 PMT |
| 1 | 0x0001 | CAT 包含一个目录，列举 PMT 使用的所有 ITU-T REC. H.222 ECM 流  |
| 2 | 0x0002 | TSDT 包含和整个 TS 相关的描述子 |
| 3 | 0x0003 | IPMP 控制信息表包含一个目录，列举 PMT 使用的所有 ISO/IEC 14496-13 控制流 |
| 4-15 | 0x0004-0x000F | 预留 |
| 16-31 | 0x0010-0x001F | DVB 元数据使用 |
| 32-8186 | 0x0020-0x1FFA | 可按需赋值给 PMT、ES 和其他数据表 |
| 8187 | 0x1FFB | DigiCipher 2/ATSC MGT 元数据使用 |
| 8188-8190 | 0x1FFC-0x1FFE | 可按需赋值给 PMT、ES 和其他数据表 |
| 8191 | 0x1FFF | 空包(用于固定带宽填充) |

DVB 元数据使用的 PID：

| 十进制 | 十六进制 | 描述 |
| --- | --- | --- |
| 16 | 0x0010 | NIT、ST |
| 17 | 0x0011 | SDT、BAT、ST |
| 18 | 0x0012 | EIT、ST、CIT |
| 19 | 0x0013 | RST、ST |
| 20 | 0x0014 | TDT、TOT、ST |
| 21 | 0x0015 | 网络同步 |
| 22 | 0x0016 | RNT |
| 23-27 | 0x0017-0x001B | 预留 |
| 28 | 0x001C | 带内信令 |
| 29 | 0x001D | 度量 (measurement)  |
| 30 | 0x001E | DIT |
| 31 | 0x001F | SIT |

### 节目 program

TS 有一个节目概念。每个节目使用一个 PMT 描述。和节目关联的 ES 在 PMT 列表中有 PID。还有一个 PID 和 PMT 本身关联。比如，数字电视中使用的一个 TS 可能包含三个节目，来标识三个电视频道。假定每个频道包含一个视频流、一个或两个音频流和其他所需的元数据。一个希望解码其中一个频道的接收者只需要解码和节目相关的每个 PID 的载荷。它可以丢弃所有其他 PID 的内容。一个包含多个节目的 TS 使用一个 MPTS 引用。包含单一节目的 TS 使用 SPTS 引用。

### 节目专用信息 PSI

有 4 中节目专用信息表。MPEG-2 规范没有指定 CAT 和 NIT 的格式：

- PAT(program association table, 节目关联表)
- PMT(program map table, 节目映射表)
- NIT(network information table, 网络信息表)
- CAT(condition access table, 条件接收表)

#### 表的分节 table section

使用表结构携带 PSI。每个表结构被分成节(section)。每个节可以跨多个 TS 包。另一方面，一个 TS 包也可以包含多个相同 PID 的节。适配域也会出现在携带 PSI 数据的 TS 包。PSI 数据不会加密以便接收端的解码器易于识别流属性。

组成 PAT 和 CAT 表的节和预先定义的 PID 关联。一个流中可能有多个独立的 PMT 节；每个节有一个唯一的用户定义的 PID 并且映射一个节目编码到元数据，该元数据描述了节内的节目和流。PMT 节 PID 在且仅在 PAT 中定义。

##### pointer

| 名称 | 比特数 | 描述 |
| --- | --- | --- |
| pointer 域 | 8 | 出现在 TS 包载荷的起始处，由 TS 头中的载荷单元开始指示位指示。用于在表的载荷数据开始之前设置包对齐字节或内容 |
| pointer 填充字节 | 8*N | 当 pointer 域不为 0 时，这是 pointer 域表示的对齐填充字节数(设置为 0xFF)或者是上一个跨 TS 包的表分节的末尾(电子编程指南) |

payload_unit_start_indicator = 0 表示 TS 包没有 section 的起始,且 TS 负载中没有 pointer_field。
pointer_field = 0x00 表示 section 的起始就在这个字节之后。

#### 描述子 descriptor

| 名称 | 比特数 | 描述 |
| --- | --- | --- |
| 描述子标签(tag) | 8 | 标签定义了描述子长度之后包含的数据结构 |
| 描述子长度 | 8 | 之后的字节数 |
| 描述子数据 | N*8 | 描述子标签定义的数据 |

#### 节目关联表 PAT

PAT 相关数据会一直重复直到 section 末尾。

| 名称 | 比特数 | 描述 |
| --- | --- | --- |
| 节目编号(program num) | 16 | 和关联的 PMT 中的 表 id 扩展相关。0x0000 预留作为 NIT PID |
| 预留位 | 3 | 设置为 0x07 (打开所有位) |
| 节目映射 PID (program map PID) | 13 | 包含关联 PMT 的 PID |

PAT 使用固定值的 PID 0x0000，表 ID 0x00。TS 包含至少一个 PID 为 0x0000 的 TS packet。一些连续的 TS packet 构成 PAT。在解码器端，PSI 节过滤器(section filter) 监听到达的 TS packet。当解码器识别出 PAT 表，会重组包并解码。一个 PAT 包含 TS 中包含的所有节目信息。PAT 包含的信息显示了 PMT PID 和节目编号。PAT 应以一个 32 位的 CRC结束。

PAT 列举 TS 中所有可用的节目。每个列举的节目使用一个 16 比特值 program_number 标识。PAT 中列举的每个程序在 PMT 中有一个关联的 PID 值。

节目编号预留 0x0000 固定值指定 PID 用于查找网络信息表的位置。如果 PAT 没有出现这个节目，默认的 PID 值 (0x0010) 应被用于 NIT。

#### 节目映射表 PMT

PMT 包含关于节目的信息。每个节目有一个 PMT。虽然 MPEG-2 标准允许在一个 PID 上传输多个 PMT (单个 TS PID 包含多个节目的 PMT 信息)，大多数 MPEG-2 “用户”(如 ATSC 和 SCTE) 要求每个 PMT 在单独的 PID 上传输，该 PID 不能被任何其他包使用。PMT 提供了 TS 中出现的所有节目的信息，包括 program_number，并列举了描述的 MPEG-2 节目包含的 ES。PMT 中也提供了位置给可选描述(描述了整个 MPEG-2 节目)、可选描述子(每个 ES 一个)。每个 ES 使用一个 stream_type 值标记。

| 名称 | 比特数 | 描述 |
| --- | --- | --- |
| 预留位 | 3 | 设置为 0x07 (打开所有位) |
| PCR PID | 13 | 包含 PCR 的 PID，用于优化随机访问流时间(从节目时间衍生)的准确性。如果未使用，设置为 0x1FFF (打开所有位) |
| 预留位 | 4 | 设置为 0x07 (打开所有位) |
| 节目信息长度未使用位 | 2 | 设置为 0 (关闭所有位) |
| 节目信息长度 | 10 | 随后的节目描述子的字节数 |
| 节目描述子 | N*8 | 当节目信息长度不为 0，这是节目信息长度的节目描述子字节 |
| ES 信息数据 | N*8 | 节目映射中使用的流 |

ES 信息数据会一直重复直到 section 末尾。

| 名称 | 比特数 | 描述 |
| --- | --- | --- |
| 流类型 | 8 | 定义在基础包标识中包含的数据结构 |
| 预留位 | 3 | 设置为 0x07 (打开所有位) |
| 基础 PID | 13 | 包含流类型数据的 PID |
| 预留位 | 4 | 设置为 0x07 (打开所有位) |
| ES 信息长度未使用位 | 2 | 设置为 0 (关闭所有位) |
| ES 信息长度 | 10 | 随后的 ES 描述子的字节数 |
| ES 描述子 | N*8 | 当 ES 信息长度不为 0，这是 ES 信息长度的 ES 描述子字节 |

这个表包含和节目相关的 ES 的 PID 编号，且包含这些 ES (视频、音频等)类型的信息。此外它可能包含一个用于其他加密流的 ECM 流。这些消息提供加密密钥选择阶段所需的信息。

### PCR

要使能解码器显示同步的内容，比如匹配关联视频的音轨，至多每 100ms 在一个 MPEG-2 TS 包的适配域传输一个 PCR。PCR 的 的 PID 通过该 MPEG-2 节目关联的 PMT 的 pcr_pid 值标识。如果应用得当，PCR 的值用于在解码器生成一个 system_timing_clock。如果实现得当，STC 解码器提供一个高精度时钟基准用于同步视频和音频 ES。MPEG2 的时间引用这个时钟。比如，PTS 本来和 PCR 相关。前 33 个比特基于 90 kHZ 时钟(低精度部分)。最后 9 个比特基于 27 MHZ 时钟(高精度部分)。PCR 抖动最大支持 +/- 500 ns。

### 空包 null packet

一些传输机制，比如 ATSC 和 DVB，在 TS 上有严格的固定码率要求。为了保证流维持一个固定码率，复用器可能需要插入额外的包。为此预留 PID 0x1FFF 值。空包的载荷可能不会包含任何数据，且接收者应该忽视空包的内容。

## 参考

- [MPEG transport stream](https://en.wikipedia.org/wiki/MPEG_transport_stream)
- [PSI](https://en.wikipedia.org/wiki/Program-specific_information)
- [MPEG-2 TS 格式解析](https://blog.csdn.net/NB_vol_1/article/details/57416971)