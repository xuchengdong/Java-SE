# 第2章Java虚拟机的结构
本文档规定了一个抽象机器。它没有描述Java虚拟机的任何特定实现。

要正确实现Java虚拟机，您只需要读取类文件格式并正确执行其中指定的操作即可。不属于Java虚拟机规范的实现细节将不必要地限制实现者的创造力。例如，运行时数据区的内存布局，所使用的垃圾收集算法以及Java虚拟机指令的任何内部优化（例如，将其转换为机器代码）都由执行者决定。

关于 *Unicode Standard, Version 8.0.0*，可在[http://www.unicode.org/](http://www.unicode.org/)上获得本规范中所有对Unicode的引用。

## 2.1 类文件格式
要由Java虚拟机执行的编译代码使用硬件和操作系统无关的二进制格式来表示，通常（但不一定）存储在文件中，称为类文件格式。类文件格式精确地定义了一个类或接口的表示形式，包括可能被视为特定于平台的对象文件格式的字节顺序的细节。

第4章“类文件格式”详细介绍了类文件格式。

## 2.2 数据类型
像Java编程语言一样，Java虚拟机运行于两种类型：基本类型和引用类型。相应地，存在可以存储在变量中的两种值，作为参数传递，由方法返回，并且操作在：基本值和引用值。

Java虚拟机希望几乎所有的类型检查都在运行时间之前完成，通常由编译器完成，并且不需要由Java虚拟机本身完成。原始类型的值不需要被标记，否则可以检查，以便在运行时确定它们的类型，或者与引用类型的值区别开来。相反，Java虚拟机的指令集使用旨在对特定类型的值进行操作的指令区分其操作数类型。例如，iadd，ladd，fadd和dadd都是添加两个数值并产生数字结果的Java虚拟机指令，但每个都分别专用于其操作数类型：int，long，float和double。有关Java虚拟机指令集中类型支持的摘要，请参见§2.11.1。

Java虚拟机包含对对象的显式支持。对象是动态分配的类实例或数组。对对象的引用被认为具有Java虚拟机类型引用。类型引用的值可以被认为是对象的指针。可能存在对对象的多个引用。对象总是通过类型引用的值进行操作，传递和测试。

## 2.3 基本类型和值
Java虚拟机支持的原始数据类型是数字类型，布尔类型（§2.3.4）和returnAddress类型（§2.3.3）。

数字类型由整型（§2.3.1）和浮点类型（§2.3.2）组成。

整型有：

* `byte`，其值为8位有符号的二进制补码整数，其默认值为零
* `short`，其值为16位有符号的二进制补码整数，其默认值为零
* `int`，其值为32位有符号的二进制补码整数，其默认值为零
* `long`，其值为64位有符号的二进制补码整数，其默认值为零
* `char`，其值为16位无符号整数，表示基本多语言平面中的Unicode代码点，以UTF-16编码，其默认值为空代码点（'\ u0000'）

浮点类型有：
* `float`，其值是float值的元素，或者在支持的情况下，float-extended-exponent值集合，其默认值为正零
* `double`，其值是双重值集合的元素，或者在支持的情况下，双扩展指数值集合，其默认值为正零

布尔类型的值将真值与真值进行编码，默认值为false。

*Java®虚拟机规范的第一版并未将布尔值视为Java虚拟机类型。但是，布尔值在Java虚拟机中的支持有限。 Java®虚拟机规范的第二版通过将布尔值作为一种类型来解决问题。*

returnAddress类型的值是Java虚拟机指令操作码的指针。在原始类型中，只有returnAddress类型不直接与Java编程语言类型相关联。

### 2.3.1 整型范围
Java虚拟机的整型的值为：

* `byte`，从-128到127（-2^7到2^7-1），闭区间
* `short`，从-32768到32767（-2^15到2^15-1），闭区间
* `int`，从-2147483648到2147483647（-2^31到2^31-1），闭区间
* `long`，从-9223372036854775808到9223372036854775807（-2^63到2^63-1），闭区间
* `char`，从0到65535（含）

### 2.3.2 浮点类型，值集和值
浮点类型是float型和dubbo型，它们概念上与IEEE标准中的32位单精度和64位双精度格式IEEE 754值和操作相关联（ANSI / IEEE标准754-1985，纽约）。

IEEE 754标准不仅包括正数和负号数值数字，而且包括正和负零，正负无穷大和特殊非数字值（以下简称“NaN”）。 NaN值用于表示某些无效操作的结果，例如将零除零。

需要Java虚拟机的每个实现来支持两个标准的浮点值集合，称为浮点值集合和双重值集合。此外，Java虚拟机的实现可以在其选项上支持两个扩展指数浮点值集合中的一个或两个，称为浮点扩展指数值集合和双扩展指数值集合。在某些情况下，这些扩展指数值集可以用于代替标准值集合来表示float或double类型的值。


