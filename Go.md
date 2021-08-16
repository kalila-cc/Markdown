- [Go](#go)
  - [基础语法](#基础语法)
  - [数据类型](#数据类型)
  - [变量](#变量)
  - [常量](#常量)
  - [运算符](#运算符)
  - [条件语句](#条件语句)
  - [循环语句](#循环语句)
  - [函数](#函数)
  - [错误处理](#错误处理)
  - [并发](#并发)

# Go

## 基础语法

- 定义：通过`package <packageName>`定义包名，只能出现一次
- 注释：单行注释(`// ...`)和多行注释(`/* ... */`)
- 关键字：`[break, default, func, interface, select, case, defer, go, map, struct, chan, else, goto, package, switch, const, fallthrough, if, range, type, continue, for, import, return, var]`
- 预定义标识符：`[append, bool, byte, cap, close, complex, complex64, complex128, uint16, copy, false, float32, float64, imag, int, int8, int16, uint32, int32, int64, iota, len, make, new, nil, panic, uint64, print, println, real, recover, string, true, uint, uint8, uintptr]`
- 类型转换：`<type>(<name>)`

## 数据类型

- 布尔类型
  - `bool`：只可以是常量 true 或者 false
- 数字类型
  - `uint8`：无符号 8 位整型 (0 到 255)
  - `int8`：有符号 8 位整型 (-128 到 127)
  - `uint16`：无符号 16 位整型 (0 到 65535)
  - `int16`：有符号 16 位整型 (-32768 到 32767)
  - `uint32`：无符号 32 位整型 (0 到 4294967295)
  - `int32`：有符号 32 位整型 (-2147483648 到 2147483647)
  - `uint64`：无符号 64 位整型 (0 到 18446744073709551615)
  - `int64`：有符号 64 位整型 (-9223372036854775808 到 9223372036854775807)
  - `float32`：IEEE-754 32位浮点型数
  - `float64`：IEEE-754 64位浮点型数
  - `complex64`：32 位实数和虚数
  - `complex128`：64 位实数和虚数
  - `byte`：类似 uint8
  - `rune`：类似 int32
  - `uint`：32 或 64 位
  - `int`：与 uint 一样大小
  - `uintptr`：无符号整型，用于存放一个指针
- 字符串类型
  - `string`：由单个字节连接起来，字符串的字节使用 UTF-8 编码标识 Unicode 文本
- 派生类型
  - 指针
    - 定义：`var <name> *<type>`
    - 空指针：`nil`
    - 指针数组：`[size]* <type>`
    - 指向指针的指针：`** <type>`
  - 数组
    - 定义：`var <name> [<size>]<type> [{<value>, ...}]`：数组长度不确定可以用`...`代替`<size>`，如果设置了数组长度，还可以通过指定下标来初始化元素(`{index: value, ...}`)
    - 多维：`[<size1>][<size2>]...`：初始化使用`{{...}, ...}`，块内最后一个`{...}`不能单独成行，需要以`,`或`}`结尾
  - 结构体
    - 定义：`type <name> struct { <member> <type> ... }`
    - 声明：`<name> := <struct> { <value>, ... }`或`{ <member>: <value>, ... }`
    - 访问：无论是结构体变量本身还是其指针，都通过`<struct>.<member>`访问成员
  - 通道
    - 定义：`make(chan <type>[, <size>])`，默认不带缓冲，若缓冲满则发生阻塞
    - 通讯：`<name> <- <chan>`或`<name> := <- <chan>`
    - 迭代：`<value> := range <chan>`，必须先执行`close(<chan>)`，否则会发生阻塞
  - 函数
    - 定义：`func [<name>](<arg>, ...) [<type>] {...}`
  - 切片
    - 定义：`var <name> []<type> [{<value>, ...}]`或`make([]type, length, capacity?)`
    - 截取：`[<array>|<slice>][start?: end?]`，用于对数组或切片进行截取，不支持负数索引
    - 相关
      - `len(<slice>)`：获取长度
      - `cap(<slice>)`：获取容量
      - `<slice> == nil`：判断是否为空切片，切片只能与`nil`进行直接比较，空的切片不一定与`nil`相等
      - `append(<slice>, value, ...)`：添加元素到切片末尾，支持传入任意多个值
      - `copy(dst, src)`：复制切片内容，若目标切片容量不足，则不会复制多余的值
  - 接口
    - 定义：`type <name> interface { <method>([<arg>, ...]) [<type>], ... }`
    - 实现：`func (<name>, <struct>) <mothod>([<arg>, ...]) <type> {...}`，实现了接口的结构体通过`new(<struct>)`可以调用方法
  - map
    - 定义：`var <name> map[<type>] <type> [{<key>: <value>, ...}]`或`make(map[<type>]<type>)`，前者若不初始化则为`nil map`，无法添加键值对，通过`<key>[, <exist>] := <map>[<type>]`可以获取键对应的值以及值是否存在

## 变量

- `var <name>[, ...] [<type>][ = <value>[, ...]]`，没有初始化时系统默认设置零值
- `<name>[, ...] := [<type>] <value>[, ...]`：省略`var`，若已声明会产生编译错误，不带声明格式的只能在函数体中出现，如果声明的局部变量没有在相同的代码块中使用它，也会得到编译错误，可以使用只写变量`_`
- `var ( <name> [<type>][ = value][, ...] )`：因式分解关键字的写法一般用于声明全局变量

## 常量

- `const <name> [<type>] = value`：`<type>`只能是布尔型、数字型和字符串型，支持多种声明形式（见`var`声明形式）
- `itoa`：在`const`块中其值为行索引值，未赋值的行将自动继承上一行的赋值表达式

## 运算符

- 算术运算符：`[+, -, *, /, %, ++, --]`
- 关系运算符：`[==, !=, >, <, >=, <=]`
- 逻辑运算符：`[&&, ||, !]`
- 位运算符：`[&, |, ^, <<, >>]`
- 赋值运算符：`[=, +=, -=, *=, /=, %=, &=, |=, ^=, <<=, >>=]`
- 其他运算符：`[&, *]`

## 条件语句

- `if expr {...}[ else[ if expr {...} else] {...}]`：else 位置不能在新的一行
- `switch <name> { case <value>[, ...]: ... [default: ...] }`：`value`必须同类型
- `switch { case <expr>: ... [default: ...] }`：另一种形式的`switch`
- `switch <interface>.(type) { case <type>: ... [default: ...] }`：用于`type-switch`来判断某个 interface 变量中实际存储的变量类型
- `fallthrough`：强制执行下一条的 case 语句，无论下一条 case 的表达式结果是否为 true
- `break`：强制退出`switch`块

## 循环语句

- `for init; condition; post {...}`：for 循环
- `for condition {...}`：同时省略`init`和`post`则等同于`while`
- `for {...}`：等同于 `for(;;) {...}`
- `for <var>[, ...] := range <string>|<array>|<slice>`：for-each eage 循环，对可迭代对象进行迭代，对于数组和切片，迭代变量为`(index, element)`，对于 map，迭代变量为`(key, value)`，对于字符串，迭代变量为`(index, char)`
- `<label>:`：标号，常用于跳转语句
- `break [<label>]`：用于跳出`for`或`switch`，可以通过标号跳出需要跳出的循环
- `continue [<label>]`：用于跳过`for`的当前循环执行下一次循环，可以通过标号跳过当前需要跳过的循环执行下一次循环

## 函数

- 定义
  - `func <name>(<arg> [type][, ...]) [<type>] {...}`：基本定义格式，省略`<name>`则为匿名函数，可以直接赋值给变量
- 闭包
  - 在函数中定义变量并返回新的函数，新的函数可以直接使用函数中的变量，形成闭包，并且变量将持续存在
- 方法
  - `func (<name> <struct>) <method>(<arg> [type][, ...]) <type> {...}`

## 错误处理

- 定义
  - `type error interface { Error() string }`
- 实现
  - 让结构体实现上述接口，并将返回值格式设置为`(<arg>, ..., error)`，即可实现错误处理

## 并发

- `go <func>(...)`：开启一个新的运行期线程， 即 goroutine，通过 channel 进行两个 goroutine 之间的同步运行和通讯
