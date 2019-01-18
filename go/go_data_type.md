# go 数据类型

- 数据类型把数据分成所需内存大小不同的数据
  - 布尔型：true 或 false
  - 数字类型：
    - 整型
      - 有符号 int(int, int8, int16, int32, int64)，默认是 int
        - int 是 32 或 64 位，取决于底层平台。建议使用 int 来表示整数，除非需要指定大小
        - 32 位系统就是 32 位，64 位 系统就是 64 位
      - 无符号 uint(uint, uint8, uint16, uint32, unit64)
        - uint 32 或 64 位，同上
    - 浮点型 float(float32, float64)，默认是 float64
    - 复数 complex(complex64, complex128)
      - complex64 32 位实数和虚数
      - complex128 64 位实数和虚数
      - 使用内置函数 complex 构造一个复数`func complex(r, i FloatType) ComplexType`
        - 实部 r 和虚部 i 应该是同一类型，float32 或 float64，返回的复数类型是 complex64 或 complex128
      - 也可直接生成复数`c := 6 + 7i`
    - byte 是 uint8 的别名
    - rune 是 int32 的别名
    - uintptr 无符号整型，用于存放一个指针
  - 字符串类型：字节使用 UTF-8 编码标识 Unicode 文本，是字节的集合
  - 派生类型：
    - 指针类型 pointer
    - 数组类型
    - 结构化类型 struct
    - Channel 类型
    - 函数类型
    - 切片类型
    - 接口类型 interface
    - map 类型
  - 变量默认值，即默认初始化的值
    - int 默认值 0
    - float32 默认值 0
    - pointer 默认值 nil
  - 可以使用`%T`格式化打印变量的类型