[TOC]

# 关键字和保留字

| keyword    | keyword     | keyword      | keyword     | keyword        |
| ---------- | ----------- | ------------ | ----------- | -------------- |
| `abstract` | `arguments` | `boolean`    | `break`     | `byte`         |
| `case`     | `catch`     | `char`       | `class`     | `const`        |
| `continue` | `debugger`  | `default`    | `delete`    | `do`           |
| `double`   | `else`      | `enum`       | `eval`      | `export`       |
| `extends`  | `false`     | `final`      | `finally`   | `float`        |
| `for`      | `function`  | `goto`       | `if `       | `implements`   |
| `import`   | `in`        | `instanceof` | `int`       | `interface`    |
| `let`      | `long`      | `native`     | `new`       | `null`         |
| `package`  | `private`   | `protected`  | `public`    | `return`       |
| `short`    | `static`    | `super`      | `switch`    | `synchronized` |
| `this`     | `throw`     | `throws`     | `transient` | `true`         |
| `try`      | `typeof`    | `var`        | `void`      | `volatile`     |
| `while`    | `with`      | `yield`      |             |                |

+ 说明
  + `typeof variable`：只能判断`number, string, boolean, function, undefined`

# 数据类型

## `Number`

+ 相关实例
  + 整数：`233, -666`
  + 浮点数：`233.3, 6.02e-23`
  + 非数字：`NaN, Infinity`
+ 相关函数
  + `Number(v)`：将任意变量转为数字

## `String`

+ 相关实例
  + 单行字符串：`'str', "str", 'a'+'b'`
  + 多行字符串：`'... // 用一对反引号'`
  + 转义字符串：`'\x##'(ascii), "\u####"(unicode)`
  + 模板字符串：`var str = 'xxx';	var des = 'ab${str}'`
  + 索引字符串：`str[i]	//字符串不可变，赋值无效`
+ 相关函数
  + `String(v)`：将任意变量转为字符串
  + `v.toString()`：将任意变量转为字符串
  + `num..toString()|(num).toString()`：将数字转为字符串
  + `parseInt(string, ?radix)`：将字符串转为数字
  + `parseFloat(string)`：将字符串转为浮点数
+ 相关字段
  + `s.length`：返回字符串的长度
+ 相关方法
  + `s.slice(?start, ?stop)`：返回字符串的切片，允许反向索引
  + `s.split(string|re)`：以`string|re`切割字符串，返回切割结果的`arr`
  + `s.replace(string|re, str)`：将匹配的字符串中的`string|re`替换为`str`，若`re`为`g`则全部替换
  + `s.toUpperCase()`：返回字符串的大写形式
  + `s.toLowerCase()`：返回字符串的小写形式
  + `s.indexOf(substr)`：返回指定字符串出现的位置，没找到则返回`-1`
  + `s.substring(start, stop)`：返回指定索引区间的子串
  + `s.startsWith(searchString, position)`：判断是否以指定字符串开头
  + `s.endsWith(searchString, ?endPosition)`：判断是否以指定字符串结尾
  + `s.trim()`：除去字符串两端的不可见字符
  + `s.trimLeft()|s.trimStart()`：除去字符串左端的不可见字符
  + `s.trimRight()|s.trimEnd()`：除去字符串右端的不可见字符

## `boolean`

+ 相关实例
  + 布尔值：`true, false, a op b && c || d && !e	// '=='会自动转换数据类型, '==='不会`
+ 相关函数
  + `isNaN(x)`：判断是否是`NaN`，它是特殊的数据类型，无法用`==`或`===`判断

## `null, undefined`

+ 相关实例
  + 空值：`null	// 大部分时候使用这个`
  + 未定义值：`undefined	// 仅在判断函数参数是否传递的情况下有用`
+ 相关函数
  + pass

## `Array`

+ 相关实例
  + 建立：`var arr = new Array(val,...); var arr = [val,...]; var arr = [[1,2],3]`
    + 索引：`arr[i] = val	// 若给越界下标数组元素赋值，数组中间的空余值为 undefined
+ 相关函数
  + `Array.isarray(arr)`：判断对象是否为`Array`类型
+ 相关字段
  - `arr.length`：返回数组的长度，若重新赋更小的值，多余的元素将会截断，若索引越界会得到`undefined`
+ 相关方法
  + `arr.slice(?start, ?stop)`：返回字符串的切片，允许反向索引
  + `arr.push(...items)`：向`arr`的末尾添加若干元素，同时返回最后被添加的元素
  + `arr.pop()`：把`arr`的最后一个元素删除，同时返回被删除的元素，若`arr`空了，会返回`undefined`
  + `arr.unshift(...items)`：向`arr`的头部添加若干元素，同时返回最后被添加的元素
  + `arr.shift()`：把`arr`的第一个元素删除，同时返回被删除的元素，若`arr`空了，会返回`undefined`
  + `arr.sort(?func)`：按照指定顺序进行排序，默认按照`ascii`码值升序排列，直接改变`arr`，返回值也是`arr`
  + `arr.reverse()`：反转整个`arr`的顺序
  + `arr.splice(start, ?deleteCount, ...items)`：从`start`处开始删除`deleteCount`个元素后紧接着添加`items`
  + `arr.join(?separater)`：把`arr`的每个元素都用`separater=','`连接起来，返回连接后的字符串
  + `arr.map(func)`：平行遍历参数列表代入函数，返回新数组
  + `arr.reduce(func)`：归并计算参数列表代入函数，返回计算结果
  + `arr.filter(func)`：过滤函数不仅可以传入数组元素还可以接收另外两个参数，表示元素的位置`index`和数组本身`self`
  + `arr.every(func)`：判断是否所有数组原元素都满足某些条件
  + `arr.find(func)`：返回符合条件的首个元素，如果找到了，返回这个元素，否则，返回`undefined`
  + `arr.findIndex(func)`：返回符合条件的首个元素下标，如果找到了，返回下标，否则，返回`-1`
  + `arr.forEach(func(value, index, data))`：把每个元素依次作用于传入的函数，但不会返回新的数组，常用于遍历数组，因此，传入的函数不需要返回值

## `Map`

- 相关实例
  - 对象创建：`var m = new Map([(key, value),...])`
- 相关方法
  - `m.set(key, value)`：增加键值对，返回更改后的`m`
  - `m.get(key)`：通过`key`获取`value`，返回值为对应的`value`，若无对应的值则返回`undefined`
  - `m.delete(key)`：删除键值对，成功删除则返回`true`

## `Set`

- 相关实例
  - 对象创建：`var s = new Set([key,...])`
- 相关方法
  - `s.add(key)`：增加键，返回更改后的`s`
  - `s.delete(key)`：删除键，成功删除则返回`true

## `Variable`

- 相关实例
  - 变量建立：`var|const a[ = val]; var m = new Map()`：变量只能申明一次，不用`var`申明的变量会被视为全局变量, `const`用以修饰常量，多变量赋值时允许解构赋值
  - 数组解构赋值：`var [x,[y,z],...] = [val,[val,val],...]`：只要`arr`与参数表结构相同都可以赋值
  - 对象解构赋值：`var {property[=default]...} = obj; let {property[:value],...} = obj`：通过`object`解构赋值不需要按顺序列出所有属性，如果对应的属性不存在，变量将被赋值为`undefined`；若使用`let {property:value,...}`进行解构赋值，则`property`不会被赋值；变量允许设置默认值，如`var {p=default} = obj`，当`obj`中无此属性，默认值则会被赋给变量；若解构赋值出错可能是被当作块处理，可以用括号将整个语句括起来

## `Object`

+ 相关实例
  + 对象创建：`var obj = {property:val,...,var function func(){}}	// property 若有非法字符，可以用单引号括起来整个属性名，并且通过obj['property']进行访问`，内部的方法有`this`指向本对象，但方法内部定义的函数又无法正确引用`this`，需要事先捕获`var that = this`以供方法的内部函数正确引用
  + 获取属性：`obj.property[ = value]; obj['property'][ = value]`
  + 存在属性：`'property' in obj; obj.hasOwnProperty('property')	// in 包括继承得到的, hasOwnProperty()则不包括`
  + 全局变量：`window`为默认存在的全局变量，函数体外的函数和变量默认为`window.foo`全局变量，为避免命名空间冲突，可以将所有原全局函数和变量绑定至自己的命名空间`var xxx = {}; xxx.foo = value`
  + 构造对象：见构造函数
+ 标准对象
  + `Date`
    + `var date = new Date(?timestamp|?year,month,date,hour,minute,second,millisecond)`：获取当前时间
    + `date.getFullYear()`：获取年份
    + `date.getMonth()`：获取月份（`0-11`）
    + `date.getDate()`：获取日/号
    + `date.getDay()`：获取星期
    + `date.getHours()`：获取时
    + `date.getMinutes()`：获取分
    + `date.getSeconds()`：获取秒
    + `date.getMilliseconds()`获取毫秒
    + `date.getTime()`：获取时间戳
    + `date.toLocaleString()`：返回`year/month/date 上午|下午hour:minute:second`字符串
    + `date.toLocaleDateString()`：返回`year/month/date`字符串
    + `date.toLocaleTimeString()`：返回`上午|下午hour:minute:second`字符串
  + `RegExp`
    + `var re = /pattern/<mode>|new RegExp(pattern, ?mode)`：`pattern`中有括号则可以提取子串，`<mode>`参数有：`g`代表可以进行全局匹配；`i`代表不区分大小写匹配；`m`代表可以进行多行匹配
    + `re.test(string)`：测试是否存在匹配的子串
    + `re.exec(string)`：返回的数组中，`arr[0]`为原字符串，后继为提取的字串，若无需要提取的子串或无匹配子串则返回`null`
  + `JSON`
    + `var js = JSON.stringify(e, t, n)`：`e`为任意数据类型，将被格式化为`string`；`t`可以是`keyArray, func(key,value), null`，用于指定需要的键，`JSON`对象的所有键都是用双引号组成的`string`；`n`为显示样式，一个空格表示按照`object`的缩进显示
    + `var obj = {property:value,...,toJSON():function(){statements}}`：自定义返回对象的`JSON`格式
    + `JSON.parse(string)`：将`string`反序列化为对应数据类型
+ 对象继承
  + `obj.__proto__ = cls`：直接继承`cls`的属性和方法
  
  + `Object.create(cls)`：返回一个对象的原型新对象，但新对象没有自己的属性，即`hasOwnProperty()`测试为空
  
  + ```javascript
    // 借助一个中间对象来实现正确的原型链
    function subcls(args) {
        cls.call(this,argArray)
    	statements
    }
    function F(){}
    F.prototype = cls.prototype
    // 把subcls的原型指向一个新的F对象，F对象的原型正好指向cls.prototype
    subcls.prototype = new F()
    // 把subcls原型的构造函数修复为subcls
    subcls.prototype.constructor = subcls
    // 继续在subcls原型（就是new F()对象）上定义方法
    subcls.prototype.method = function(args){statements}
    
    // 通过构造inherits()进行封装
    function inherits(Child, Parent) {
        var F = function () {};
        F.prototype = Parent.prototype;
        Child.prototype = new F();
        Child.prototype.constructor = Child;
    }
    ```
  
  + ```javascript
    // 通过class直接构造类
    class cls {
        constructor(args) {
            this.args = args
        }
    	method(args) {
            statements
        }
    }
    // 通过extends直接继承类
    class subcls entends cls {
        constructor(args) {
            super(args)
            this.args = args
        }
    	method(args) {
            statements
        }
    }
    ```

# 程序结构

## 条件结构

+ `if (condition) {statements}`
+ `if (condition) {statements} else {statements}`
+ `if (condition) {statements} else if (condition) {statements} else if,..., else {statements}`

## 循环结构

+ `for ([initialization];[condition];[variation]) {statements}`
+ `for ([var|let ]index in arr) {statements}	// let 可以申明一个块级作用域的变量，下同`
+ `for ([var|let ]property in obj) {statements}`
+ `for ([var|let ]ele of arr|generator) {statements}`
+ `for ([var|let ][key, value] of map) {statements}`
+ `for ([var|let ]key of set) {statements}`
+ `while (condition) {statememts}`
+ `do {statememts} whlle (condition)`

# 函数

## 普通函数

+ `function func(args1,...,args2=default,...,...rest) {statements,...,return value}`：传入参数过多不影响，过少则自动传入`undefined`，自带`arguments`，可以通过`arguments[i]`进行参数列表的访问，`arguments`具有`length`属性，要把中间某些参数变为可选参数，就只能通过`arguments`判断，然后重新调整参数并赋值；自带`rest`，可以通过`func(args,...rest)`得到多余参数列表；若函数变量声明在语句后面，会被自动提升声明于语句前，但不会提升赋值；若函数中引用了`this`，可以通过`func.apply(obj, argArray)`或`func.call(obj, args,...)`绑定对象，若不需要绑定对象则令`obj=null`

## 匿名函数

+ `var func = function(...items) {statements,...,return value}`：立即执行的匿名函数可以直接括起来后直接用带参括号执行函数

## 箭头函数

+ `var f = (x,...) => {statements,...,return res}|({property:value,...}); var f = x => expression`：若用于在对象的方法内部建立函数，则`this`始终指向外部对象`obj`，不再需要`var that = this`来获取指向，且调用教头函数的`call(), apply()`的第一个参数`thisArg`将会被自动忽略

## 构造函数

```javascript
function cls(args) {
    this.property = value,
	...
	this.method = function(args){statements},
	... // 不需要 return this
}
var ins = new Cls(args) // 只有通过 new 调用才会成为构造函数，否则只是普通函数且返回 undefined
cls.prototype.func = function(args){statements} // 此方法能够使实例共用该方法，节省内存
// ins.constructor == cls.prototype.constructor == cls
// cls.prototype == ins.__proto__ == Object.getPrototypeOf(cls) == {constructor: f}
// ins instanceof cls == true
```



## 装饰器

```javascript
var old_func = func
func = function(args) {
    var x = args || default	// 通过这个方法可以设置默认参数
    statements
    return old_func.apply(null, arguments)
}
```

## 生成器

```javascript
  function* generator(args) {
      statements
      yield value
      return value
  }
  var g = generator(args) // generator {<suspended>}
  g.next() // {value:val, done:false|true}，更常用的是通过for...of来遍历生成器实例
```

## 闭包

```javascript
function func(args) {
    var x = value[ || default]
    statements
    return {
        variable:value,...
        foo:function(args){statements}	//闭包引用的变量不会被外部访问，且仅在对象的方法被调用时才使用函数变量；返回值也可以是函数，可用于返回减少参数数量的新函数
    }
}
```

# 浏览器

## 浏览器对象

+ `window`
  + `window.innerWidth`：获取浏览器窗口的内部宽度
  + `window.innerHeight`：获取浏览器窗口的内部高度
  + `window.outerWidth`：获取浏览器窗口整体的宽度
  + `window.outerHeight`：获取浏览器窗口整体的高度
+ `navigator`
  + `navigator.appName`：浏览器名称
  + `navigator.appVersion`：浏览器版本
  + `navigator.language`：浏览器设置的语言
  + `navigator.platform`：操作系统类型
  + `navigator.userAgent`：浏览器设定的`User-Agent`字符串
+ `screen`
  + `screen.width`：屏幕宽度，以像素为单位
  + `screen.height`：屏幕高度，以像素为单位
  + `screen.colorDepth`：返回颜色位数，如8、16、24
+ `location`
  + `location.href`：返回当前页面的`href(URL)`
  + `location.protocol`：返回使用的`web`协议`(http:|https:)
  + `location.host|location.hostname`：返回 web 主机的域名
  + `location.port`：返回（当前页面的）互联网主机端口的编号
  + `location.pathname`：返回当前页面的路径或文件名
  + `location.search`：返回传递的请求参数
  + `location.hash`：代表网页中的一个位置
  + `location.assign(url)`：加载新文档
  + `location.reload()`：重新加载网页
+ `document`
  + `document.title`：浏览器窗口标题
  + `document.cookie`：当前页面的`cookie`
  + `document.getElementById(elementId)`：按`id`获得一个`DOM`节点，常利用`p.children`(动态变化量)以遍历子节点，`p.parentElement`可以获得其父节点
  + `document.getElementsByTagName(localName)`：按`tag`名称获得一组`DOM`节点
  + `document.getElementsByClassName(classNames)`：按`class`名称获得一组`DOM`节点
  + `document.querySelector(selectors)`：通过`selector`语法获取节点
  + `document.querySelectorAll(selectors)`：通过`selector`语法获取节点
  + `document.createElement(localName, ?options)`：创建一个新的节点，可以自定义`id, innerText`等属性
+ `history`
  + `history.back()`：返回至前一个网页
  + `history.forward()`：前进到后一个网页

## `DOM`

+ 更新`DOM`
  + `p.innerHTML`：不但可以修改一个DOM节点的文本内容，还可以直接通过HTML片段修改DOM节点内部的子树
  + `p.innerText|dom.textContent`：自动对字符串进行HTML编码，保证无法设置任何`HTML`标签；两者的区别在于读取属性时，`innerText`不返回隐藏元素的文本，而`textContent`返回所有文本
  + `p.style`：下属包括`color, fontSize, peddingTop`等属性
+ 插入`DOM`
  + `p.appendChild(node)`：把一个子节点添加到父节点的最后一个子节点，若节点已经存在于当前的文档树，则这个节点首先会从原先的位置删除，再插入到新的位置
  + `p.insertBefore(node, child)`：把子节点插入到指定的位置
+ 删除`DOM`
  + `p.removeChild(child)`：删除子节点

## 表单

+ `<input type="text">`：文本框，用于输入文本，可通过`p.value`获得输入值或设置值
+ `<input type="password">`：口令框，用于输入口令，可通过`p.value`获得输入值或设置值，若无`name`属性则不会被提交，浏览器默认点击`<button type="submit">`时提交表单，或者用户在最后一个输入框按回车键。`JavaScript`可以以两种方式来处理表单的提交，方式一是通过`<form>`元素的`submit()`方法提交一个表单，第二种方式是响应`<form>`本身的`onsubmit="return func(args)"`事件(对应`<button type="submit">`)，方式二要`return true`来告诉浏览器继续提交，如果`return false`，浏览器将不会继续提交`form`，这种情况通常对应用户输入有误，提示用户错误信息后终止提交`form`
+ `<input type="radio">`：单选框，用于选择一项，可通过`p.value`获得预设值，通过`p.checked`判断是否被选择
+ `<input type="checkbox">`：复选框，用于选择多项，可通过`p.value`获得预设值，通过`p.checked`判断是否被选择
+ `<select>`：下拉框，用于选择一项，可通过`p.value`获得输入值或设置值
+ `<input type="hidden">`：隐藏文本，用户不可见，但表单提交时会把隐藏文本发送到服务器，可通过`p.value`获得输入值或设置值
+ `<input type="date">`：日期框，其`value`将保证是一个有效的`YYYY-MM-DD`格式的日期，或者空字符串
+ `<input type="datetime">`：日期时间框，其`value`将保证是一个有效格式的日期时间，或者空字符串
+ `<input type="datetime-local">`：日期框，其`value`将保证是一个有效格式的本地日期时间，或者空字符串
+ `<input type="color">`：颜色框，其`value`可以用`#xxxxxx`的`RGB`格式表示
+ `<input type="file">`：可以上传文件的唯一控件；当一个表单包含`<input type="file">`时，表单的`enctype`必须指定为`multipart/form-data`，`method`必须指定为`post`，浏览器才能正确编码并以`multipart/form-data`格式发送表单的数据；出于安全考虑，浏览器只允许用户点击`<input type="file">`来选择本地文件，用JavaScript对`<input type="file">`的`value`赋值是没有任何效果的。当用户选择了上传某个文件后，JavaScript也无法获得该文件的真实路径；上传的文件都由后台服务器处理，`JavaScript`可以在提交表单时通过`file.value`获取文件名字符串对文件扩展名做检查，以便防止用户上传无效格式的文件

## `Promise`

+ `setTimeout(handler, ?timeout, args,...)`：延迟指定时间后执行指定任务
+ `var p = new Promise(func)`：创建`Promise`对象
+ `p.then(?onfullfilled, ?onrejected)`：若执行任务成功则执行`then(func)`，否则执行`catch(func)`，允许串联使用，如`p.then(func).catch(func)`，或`p.then(p2).then(p3).catch(handleError)`
+ `Promise.all(promiseArray)`：并行执行任务
+ `Promise.race(promiseArray)`：并行执行任务，但只取最先完成的任务的结果

## `Canvas`

+ `<canvas id=v width=v height=v></canvas>`：画布，可以在上面绘制各种图表、动画等
+ `canvas.getContext('2d'|'webgl')`：获取`2d|3d`绘图对象
+ `ctx.clearRect(x, y, width, height)`：擦除指定位置指定大小的矩形，擦除的意思是把该区域变为透明
+ `ctx.fillStyle`：返回或设置颜色
+ `ctx.strokeStyle`：返回或设置绘图颜色
+ `ctx.shadowColor`：返回或设置阴影颜色
+ `ctx.shadowOffsetX`：返回或设置阴影偏移量
+ `ctx.shadowOffsetY`：返回或设置阴影偏移量
+ `ctx.font`：返回或设置文字字体
+ `ctx.fillRect(x, y, width, height)`：把除指定位置指定大小的矩形涂色
+ `ctx.fillText(words, 20, 40)`：在指定位置绘制文字
+ `ctx.stroke(path)`：将绘图轨迹加入画布
+ `var path=new Path2D()`：返回`Path`对象用以绘制复杂路径
+ `path.moveTo(x, y)`：移动光标
+ `path.arc(x, y, radius, from_angle, to_angle, true|false)`：绘制弧线

# `jQuery`

## 选择器

+ `$(selector|node|func)`：等价于`jQuery()`

+ `$('[tag][#id][.class][prop[^|$]=val][>]child[:filter][,other][ etc]')`：返回搜索到的`DOM`数组，属于`jQuery`对象，可以通过`jq.get(e)|jq[e]`获取需要的`DOM`对象，`filter`可选参数如下

  | args                   | description                                                  |
  | ---------------------- | ------------------------------------------------------------ |
  | `first-child`          | 仅选出`jQuery`中第一个对象                                   |
  | `last-child`           | 仅选出`jQuery`中最后一个对象                                 |
  | `nth-child(n|even|odd` | 选出第`n`个元素，`n`从`1`开始 \| 选出序号为奇数/偶数的元素   |
  | `input`                | 可以选择`<input>`，`<textarea>`，`<select>`和`<button>`      |
  | `file`                 | 可以选择`<input type="file">`，和`input[type=file]`一样      |
  | `checkbox`             | 可以选择复选框，和`input[type=checkbox]`一样                 |
  | `radio`                | 可以选择单选框，和`input[type=radio]`一样                    |
  | `focus`                | 可以选择当前输入焦点的元素，例如把光标放到一个`<input>`上    |
  | `checked`              | 选择当前勾上的单选框和复选框                                 |
  | `enabled`              | 可以选择可以正常输入的`<input>, <select>` 等，也就是没有灰掉的输入 |
  | `disabled`             | 和`:enabled`正好相反，选择那些不能输入的                     |
  | `visible`              | 选择所有可见的元素                                           |
  | `hidden`               | 选择所有隐藏的元素                                           |

+ `jq.find(selector)`：寻找子节点

+ `jq.parent(?selector)`：寻找父节点，可增加过滤条件

+ `jq.next(?selector)`：寻找下一个元素，可增加过滤条件

+ `jq.prev(?selector)`：寻找上一个元素，可增加过滤条件

+ `jq.filter(selector|func)`：过滤节点

+ `jq.map(func)`：遍历节点至函数

+ `jq.first()`：获取首个节点，相当于使用`:first-child`

+ `jq.last()`：获取末尾节点，相当于使用`:last-child`

+ `jq.slice(start, stop)`：对`DOM`数组进行切片

## `DOM`

+ `jq.text(?content)`：无参数则获取节点的文本，有参数则设置文本
+ `jq.html(?content)`：无参数则获取节点的原始`HTML`文本，有参数则设置`HTML`文本
+ `jq.css(prop, val)`：作用于`DOM`节点的`style`属性，具有最高优先级
+ `jq.hasClass(cls)`：判断是由属于指定的类
+ `jq.addClass(cls)`：设置为指定的类
+ `jq.removeClass(cls)`：删除指定的类
+ `jq.show(?time|speed)`：无参数则显示`DOM`，有参数则从左上角逐渐出现，时间以毫秒为单位，也可以是`'slow', fast'`
+ `jq.hide(?time|speed)`：无参数则隐藏`DOM`，有参数则从左上角逐渐消失，时间以毫秒为单位，也可以是`'slow', 'fast'`
+ `jq.toggle(?time|speed)`：根据状态决定`show()`还是`hide()`
+ `jq.slideDown(?time|speed)`：无参数则显示`DOM`，有参数则从上往下逐渐出现，时间以毫秒为单位，也可以是`'slow', fast'`
+ `jq.slideUp(?time|speed)`：无参数则隐藏`DOM`，有参数则从下往上逐渐消失，时间以毫秒为单位，也可以是`'slow', 'fast'`
+ `jq.slideToggle(?time|speed)`：根据状态决定`slideUp()`还是`slideDown()
+ `jq.fadeIn(?time|speed)`：无参数则显示`DOM`，有参数则淡入，时间以毫秒为单位，也可以是`'slow', fast'`
+ `jq.fadeOut(?time|speed)`：无参数则隐藏`DOM`，有参数则淡出，时间以毫秒为单位，也可以是`'slow', 'fast'`
+ `jq.fadeToggle(?time|speed)`：根据状态决定`fadeIn()`还是`fadeOut()`
+ `jq.animate(obj, time, ?func)`：自定义动画，`obj`中列出属性值`opacity, width, height`等，`time`设置执行时间，`func`设置结束后执行的函数
+ `jq.width(?width)`：无参数则获取对象宽度，有参数则设置宽度
+ `jq.height(?height)`：无参数则获取对象高度，有参数则设置高度
+ `jq.attr(attr)`：获取属性值，但处理`'checked'`会返回`'checked'`
+ `jq.prop(prop)`：获取属性值，处理`'checked'`会返回`boolean`
+ `jq.is('checked')`：处理`'checked'`会返回`boolean`
+ `jq.val(?value)`：无参数则获取`value`属性，有参数则设置`value`属性
+ `jq.append(selector|node|jquery)`：添加新的`DOM`节点到子节点最后面，如果要添加的节点已经存在于`HTML`文档中，它会首先从文档移除，然后再添加，也就是说可以用来移动一个节点，`prepend()`类似
+ `jq.prepend(selector|node|jquery)`：添加新的`DOM`节点到子节点最前面
+ `jq.after(selector|node|jquery)`：添加新的`DOM`节点到同级节点后面
+ `jq.before(selector|node|jquery)`：添加新的`DOM`节点到同级节点前面
+ `jq.remove()`：删除节点

## 事件

+ `jq.on(act, func)`：绑定一个事件，需要传入事件名称和对应的处理函数，也可以直接通过`jq.act(?func)`绑定(若`func`为空则视为手动触发事件，仅通过`js`代码改变值无法触发事件)，事件参数如下

  | action       | description                                                  |
  | ------------ | ------------------------------------------------------------ |
  | `click`      | 鼠标单击时触发                                               |
  | `dblclick`   | 鼠标双击时触发                                               |
  | `mouseenter` | 鼠标进入时触发                                               |
  | `mouseleave` | 鼠标移出时触发                                               |
  | `hover`      | 鼠标进入和退出时触发两个函数，相当于`mouseenter`加上`mouseleave` |
  | `mousemove`  | 鼠标在`DOM`内部移动时触发                                    |
  | `keydown`    | 键盘按下时触发，仅作用在当前焦点的`DOM`上                    |
  | `keyup`      | 键盘松开时触发，仅作用在当前焦点的`DOM`上                    |
  | `keypress`   | 按一次键后触发，仅作用在当前焦点的`DOM`上                    |
  | `focus`      | 当`DOM`获得焦点时触发                                        |
  | `blur`       | 当`DOM`失去焦点时触发                                        |
  | `change`     | 当`<input>`、`<select>`或`<textarea>`的内容改变时触发        |
  | `submit`     | 当`<form>`提交时触发                                         |
  | `ready`      | 当页面被载入并且`DOM`树完成初始化后触发，仅作用于`document`对象，自己的初始化代码必须放到`document`对象的`ready`事件中，保证`DOM`已完成初始化；通常这么写：`$(document).ready(func)`，甚至简化为`$(func)` |

+ `jq.off(?act, ?func)`：`act`为空则移除所有绑定事件，`func`不允许是匿名函数，若为空则移除所有同类型绑定事件

## 扩展

+ ```javascript
  $.fn.func = function(options) {
      // 'extend()'参数优先级越往右越高；允许通过'default.prop'设置默认参数
      var opts = $.extend({}, defaults, options)
      statements
      return this
  }
  jq.func(args)
  ```

## 错误处理

+ ```javascript
  try {
      // 可以用 throw 抛出异常，例如 'throw new Error(message)'；实际上，JavaScript允许抛出任意对象
      statements
  } catch(e) {
      statements
  } finally {
      statements
  } // try 和 finally 可以不必同时出现
  ```

# `underscore`

+ `_`：被`underscore`绑定的全局变量
+ `_.map(arr, func)`：遍历代入，返回`Array`
+ `_.mapObject(arr, func)`：遍历代入，返回`Object`
+ `_.every(arr, func(value,key))`：当集合的所有元素都满足条件时返回`true`
+ `_.some(arr, func(value,key))`：当集合的至少一个元素满足条件时返回`true`
+ `_.max(arr|obj)`：返回最大值，若传入`Object`则只作用于`value`
+ `_.min(arr|obj)`：返回最小值，若传入`Object`则只作用于`value`
+ `_.groupBy(arr, func)`：根据`func`返回的`key`进行归类并返回`Object`
+ `_.shuffle(arr)`：随机打乱一个集合
+ `_.sample(arr, ?n)`：随机选择一个或多个元素
+ `_.first(arr)`：返回第一个元素
+ `_.last(arr)`：返回最后一个元素
+ `_.flatten(arr)`：将多嵌套数组变成一维数组
+ `_.zip(arr,arr,...)`：把两个或多个数组的所有元素按索引对齐，然后按索引合并成新数组
+ `_.unzip(arr)`：与`zip()`功能相反
+ `_.object(arr,arr)`：把两个数组的所有元素按索引对齐，然后按索引合并成`Object`
+ `_.range(?start, stop, ?step)`：返回一个整数等差序列
+ `_.bind(obj.func, obj)`：返回绑定了`this`的函数
+ `_.partial(func, args,...)`：返回已固定部分传入参数的函数，可以用`_`作为占位符
+ `_.memorize(func)`：返回可以自动缓存计算结果的函数，在递归调用下能够极大地简化计算
+ `_.once(func)`：返回一个保证仅被执行一次的函数
+ `_.delay(func, time, args,...)`：指定时间后自动执行函数，与`setTimeOut()`类似
+ `_.keys(obj)`：返回`Object`的所有`key`，但不包含从原型链继承下来的
+ `_.allKeys(obj)`：返回`Object`的所有`key`，包含从原型链继承下来的
+ `_.values(obj)`：返回`Object`的所有`value`，但不包含从原型链继承下来的
+ `_.invert(obj)`：把`Object`的每个`key-value`交换，`key`变成`value`，`value`变成`key`
+ `_.extend(obj,...)`：把多个`Object`的`key-value`合并到第一个`Object`并返回，如果有相同的key，后面的`Object`的`value`将覆盖前面的`Object`的`value`
+ `_.clone(obj)`：返回`Object`的浅拷贝
+ `_.isEqual(obj, obj)`：对两个object进行深度比较，如果内容完全相同，则返回`true
+ `_.chain(arr)`：把对象包装至能够进行链式调用

# `Node.js`

+ `filename.js`：`JavaScript`文件，通过`node filename.js`可以运行，或通过`node`直接进入交互模式，常在开头使用`'use strict'`，也可以通过`node --use_strict filename.js`直接在严格模式运行
+ `module.exports = variable`：把变量`variable`作为模块的输出暴露出去，使其他模块能够使用`variable`；也可以使用`exports.prop = value`逐个添加属性值
+ `require(module_name)`：引用模块，不需要加上`JavaScript`文件后缀，当前路径下文件可用`./filename`
+ `global`：类似于`window`的服务器端的全局变量，通过`typeof(window)=='undefined'`可以判断运行平台
+ `process`：代表当前`Node.js`进程，等价于`global.process`
  + `process.version`：获取版本
  + `process.platform`：获取操作系统名称
  + `process.arch`：获取操作系统位数
  + `process.cwd()`：获取当前工作目录
  + `process.chdir(path)`：切换当前工作目录
  + `process.nextTick(?act, func)`：在下一次事件响应中执行`func`，事件缺省时默认响应`process`进程
+ `fs`：文件系统模块
  + `require('fs')`：获取文件读写对象
  + `fs.readFile(filepath, ?encoding, func(err, data))`：异步读文件，`encoding`缺省则`data`为`buffer`(二进制数组)，未缺省则为`string`；`buf.toString(encoding)`将`buffer`转换为`string`，`Buffer.from(str, encoding)`将`string`转换为`buffer`；异步读文件常用`err`判断异常，无异常则为`null`
  + `fs.readFileSync(filepath, ?encoding)`：同步读文件，常用`try...catch...`判断异常
  + `fs.writeFile(filepath, data, func(err))`：异步写文件，若`data`为`string`则默认按`utf-8`写入，若为`buffer`则写入二进制文件
  + `fs.writeFileSync(filepath, data)`：同步写文件
  + `fs.stat(filepath, func(err, stat))`：异步获取文件信息；`stat`有多个属性，如`isFile(), isDirectory(), size, birthtime, mtime`，分别为判断是否文件或目录，文件大小，创建时间和修改时间
  + `fs.createReadStream(filepath, encoding)`：获取可读文件流对象，对象具有`rs.on('data', func(chunk)), rs.on('end', func()), rs.on('error', func(err))`；`data`事件表示流的数据已经可以读取了，`end`事件表示这个流已经到末尾了，没有数据可以读取了，`error`事件表示出错了；`data`事件可能会有多次，每次传递的`chunk`是流的一部分数据；同时还能通过`rs.pipe(ws)`将可读文件流的数据直接通过管道输入可写文件流，若不希望写入完毕后自动关闭管道，可以传入参数，如`rs.pipe(ws, {end:false})`
  + `fs.createWriteStream(filepath, encoding)`：获取可写文件流对象，对象具有`ws.write(string|buffer), ws.end()`等方法
+ `crypto`
  + `const crypto = require('crypto')`：获取`crypto`对象
  + `crypto.createHash('md5|sha1')`：获得`Hash`加密对象，通过`hash.update(string|buffer)`加密数据，默认为`utf-8`，通过`hash.digest('hex')`获得加密结果
  + `crypto.createHmac('md5|sha1|sha256', secret_key)`：获得`Hmac`加密对象，需要密匙