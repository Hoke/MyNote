# 2 重构原则

重构的定义分为名词定义和动词定义：

- 名词定义：对软件内部结构的一种调整，目的是在不改变软件可观察行为的前提下，提高其可理解性，降低其修改成本。
- 动词定义：使用一系列重构手法，在不改变软件可观察行为的前提下，调整其结构。

重构目的：

- 改进软件设计：消除重复代码
- 使软件更容易理解：让代码更好地表达自己的用途；协助理解不熟悉的代码
- 帮助找到 bug：理解代码，弄清楚程序结构，验证程序假设
- 提高编程速度：提高代码质量，维持良好设计

重构时机：一句话概括为“事不过三，三则重构”

- 添加功能时：重构代码使得更容易增加新特性
- 修补错误时：重构代码提高代码可读性，加深对代码的列
- 审核代码时

间接层和重构：间接层的价值包括

- 允许逻辑共享：父类和子类，或多处调用一个相同的函数
- 分开解释意图和实现：使用更小的类和函数，更好地解释实现意图
- 隔离变化：修改子类实现而不影响其他地方
- 封装条件逻辑：将条件逻辑转化为消息形式

重构与设计：保持设计的简单性，使用重构满足灵活性

重构与性能：维持程序结构良好，在进入性能优化阶段时再进行优化