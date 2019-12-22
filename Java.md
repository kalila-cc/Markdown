[TOC]

# 数据类型

## byte

+ 基本信息：1B；-128 ~ 127

## short

+ 基本信息：2B；-32768 ~ 32767

## int

+ 基本信息：4B； -2147483648 ~ 2147483647

## long

+ 基本信息：8B；-9223372036854775808 ~ 9223372036854775807

## float

+ 基本信息：4B；3.4x10^38
+ 其他：除以0时，可能产生`NaN, Infinity, -Infinity`

## double

+ 基本信息：8B；1.79x10^308
+ 其他：除以0时，可能产生`NaN, Infinity, -Infinity`

## char

+ 基本信息：2B；`ascii / unicode`

## boolean

+ 基本信息：4B；`true / false`

## String

+ 基本信息：引用类型，使用上与值类型接近
+ 相关方法
  + `str.equals(string)`：判断字符串内容是否相同，若用`==`只能判断是否指向同一个对象，在`switch`语句对比内容时却可以直接对应字符串；如果变量为`null`，会报`NullPointerException`，要避免`NullPointerException`错误，可以利用短路运算符`&&`，如`str != null && str.equals(string)`

## Array

+ 基本信息：不可变，通过`<type>[] arr = [new <type>[len]][{...}]`创建，可以通过`arr.length`获取数组长度，所有值默认为 0

# 面向对象编程

## 类

+ `field`：
  + `public/private`则都能被内部`method`通过`field|this.field(recommend)`访问，`private`则外部无法访问
  + 可通过`setField/getField`提供接口给外部修改`field`
  + 若`field`为引用类型，外部修改只会改变引用对象的指向，即浅拷贝
  + 未初始化的`field`，值类型默认为 0，引用类型默认为 `null`
+ `method`：
  + `private`则仅在内部可访问
  + `method`的参数可以用可变参数`<type>... v`，如此可避免传参时构造数组的麻烦
+ `constructor`
  + `public <class>(args) {statements}`：构造器的定义方式，不需要返回值
  + 调用构造方法必须用`new`操作符
  + 可定义多个构造器
  + 在执行构造器之前先执行`field`定义语句
  + 可通过`this(args)`在一个构造器内部调用另一个构造器，便于代码复用

# java 特性

+ java 11
  + `for (<type> v: arr) {statements}`：直接迭代数组内容
+ java 12
  + `var v = val`：自动判断数据类型
  + `switch(val){case val -> statements;...}`：省略`break`子句