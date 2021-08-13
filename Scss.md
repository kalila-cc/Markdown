- [Scss](#scss)
  - [CSS功能拓展](#css功能拓展)
  - [注释](#注释)
  - [SassScript](#sassscript)
  - [@-rules与指令](#-rules与指令)
  - [控制指令](#控制指令)
  - [混合指令](#混合指令)
  - [函数指令](#函数指令)
  - [输出格式](#输出格式)

# Scss

## CSS功能拓展

```scss
selector {
  child-selector {...}
  & {...}
  &-xxx {...}
  ...
}
xxx: [zzz]{
  yyy: zzz;
  ...
}
```

- 内层的样式将它外层的选择器作为父选择器
- 通过`&`代表嵌套规则外层的父选择器
- 通过`&`跟随后缀生成复合的选择器
- 通过`xxx:`形成命名空间，命名空间也可以包含自己的属性值

## 注释

```scss
$name: "interpolation";
// 编译后不被保留的单行注释
/* 编译后会被保留的多行注释 */
/* !将`!`作为多行注释的第一个字符表示在压缩输出模式下保留这条注释并输出到 CSS 文件中，通常用于添加版权信息 */
/* 插值语句（#{$name}）也可写进多行注释中输出变量值 */
```

## SassScript

- `$variable: value [!global];`
  - 变量以`$`开头，通过`$variable`直接使用
  - 变量支持块级作用域
  - 通过`!global`将局部变量转换为全局变量
  - 通过`#{$variable}`插值语句可以在选择器或属性名中使用变量，可以避免被解析为运算表达式
- 数据类型
  - `number`：`1, 2, 13, 10px`
  - `string`：有引号字符串与无引号字符串，`"foo", 'bar', baz`
  - `color`：`blue, #04a3f9, rgba(255,0,0,0.5)`
  - `boolean`：`true, false`
  - `null`：`null`
  - `list`：用空格或逗号作分隔符，`1.5em 1em 0 2em, Helvetica, Arial, sans-serif`
  - `map`：`(key1: value1, key2: value2)`
- 运算
  - 支持数字的加减乘除、取整等运算`(+, -, *, /, %)`，如果必要会在不同单位间转换值
  - 关系运算`<, >, <=, >=`也可用于数字运算
  - 相等运算`==, !=`可用于所有数据类型
  - 颜色值的运算`(+)`是分段计算进行的，数字与颜色值之间也可以进行算数运算`(*)`，如果颜色值包含 alpha 通道，必须拥有相等的 alpha 值才能进行运算，因为算术运算不会作用于 alpha 值
  - 支持字符串运算`(+)`，如果左侧有引号字符串连接无引号字符串，运算结果是有引号的，相反，左侧无引号字符串连接有引号字符串，运算结果则没有引号
  - 支持布尔型的`and/or/not`运算
- `$variable: value !default;`
  - 为已声明而未赋值或为空值的变量赋值

## @-rules与指令

- `@import "xxx"[, "yyy"]`
  - 导入 Scss/Sass 文件，被导入的文件将合并编译到同一个 CSS 文件
  - 文件名若是`http://`开头，或是`url()`，或扩展名是`.css`，则语句仅作为普通的 CSS 语句
  - 允许同时导入多个文件
  - 在`url()`导入方式中，可以使用`#{}`插值语句
  - 以下划线开头的文件不会被编译为 CSS
  - 支持在块级作用域使用`@import`，这只会在该区域中起作用，但不可以在混合指令或控制指令中嵌套
- `@media [screen|speecn|print|all] and (xxx: yyy) {...}`
  - 针对不同的媒体类型定义不同的样式
  - 块级作用域的`@media`将被编译到文件的最外层
  - 允许互相嵌套使用，编译自动添加`and`
- `xxx { @extend yyy; ... }`
  - 同一个选择器可以延伸多个选择器，后定义的样式享有优先权
  - 通过`!optional`声明可以不生成新选择器
  - 如果在`@media`层中使用`@extend`，只能延伸同层选择器
- `@at-root [xxx] {...}`
  - 将样式置为在 CSS 文档最外层
- `@debug/@warn/@error expr`
  - 将表达式的值打印到标准错误输出流
  - `@warn`会产生样式表跟着，`@error`会产生堆栈跟踪

## 控制指令

- `@if expr {...} @else if expr {...} ... @else {...}`
- `@for $var from <start> through/to <end>`
  - `through`表示包含两区间端点，`to`表示只包含`<start>`而不包含`<end>`
- `@each $var in <list> {...}`
  - `<list>`可以是`a, b, ...`
  - `$var`可以是`$var1, $var2, ...`，`<list>`中每个元素需要都是`list`
  - 若`$var`恰好是`$key, $value`，则`<list>`可以是`<map>`
- `@while expr {...}`
  - 通过`$var: expr;`进行变量更新

## 混合指令

- `@mixin xxx[($var[: value], ...)] {...}`
  - 通过包含多种 CSS 规则，定义可重复使用的样式，减少样式数量
  - 支持设置参数的默认值
- `@include <mixin>[([$var: ]value, ...)];`
  - 引用混合样式，可在其后添加需要的参数（可选）
  - 可以直接在最外层使用`@incldue`
  - 可以在`@mixin`中使用`@incldue`
  - 支持关键词参数，阅读起来会更方便，且关键词参数可以打乱顺序使用
- `$var...`
  - 在`@mixin`参数列表中，用于将多个参数作为值列表类型(`list`)
  - 在`@include`参数列表中，用于将值列表类型(`list`)的值逐个作为参数引用
- `@content`
  - 在`@mixin`内部样式中的`@content;`，会将`@include <mixin> {...}`内部的样式导入到`@content;`处，并将导入后的`@mixin`的样式导出
  - 当`@content;`在指令中出现过多次或者出现在循环中时，额外的代码将被导入到每一个地方

## 函数指令

- `@function func($var[: value], ...) { ... @return expr; }`
  - 自定义的函数也可以使用关键词参数
  - 建议在自定义函数前添加前缀避免命名冲突，其他人阅读代码时也会知道这不是 Sass 或者 CSS 的自带功能

## 输出格式

通过`:style option`选项设定，或者在命令行中使用`--style`选项

```scss
/*
  :nested
  更耗表现结构
*/
selector {
  ...}

/*
  :expanded
  类似手写方式
*/
selector {
  ...
}

/*
  :compact
  嵌套过的选择器在输出时没有空行
  不嵌套的选择器会输出空白行作为分隔符
*/
selector { ... }

/*
  :compressed
  删除所有无意义的空格、空白行、注释
  以及自动替换占用空间最小的颜色表达方式
*/
selector{...}
```
