- [jQuery](#jquery)
  - [引入](#引入)
  - [语法](#语法)
  - [事件](#事件)
  - [效果](#效果)
  - [HTML](#html)
  - [遍历](#遍历)
  - [选择器](#选择器)
  - [AJAX](#ajax)
  - [DOM 元素](#dom-元素)

# jQuery

## 引入

- 下载：[jQuery](https://jquery.com/download/)
- CDN：[Google CDN](http://ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js)、[Microsoft CDN](http://ajax.microsoft.com/ajax/jquery/jquery-1.8.0.min.js)

## 语法

- `$(selector|html|HTMLElement).action(function)`
  - `selector`可以是某个元素本身(`this`)，也可以是`html`字符串
  - 多个`action`可以链式调用
  - `$(document).ready(function)`为文档就绪函数
- `jQuery.noConflict()`
  - 可自定义变量名避免`$`冲突问题
  - 可通过`jQuery(document).ready(function($){...})`在函数内部使用`$`符号，而外部仍然只能使用`jQuery`，这适用于外部存在`$`的情况

## 事件

| 方法 | 描述 |
|:-|:-|
| `bind()` | 向匹配元素附加一个或更多事件处理器 |
| `blur()` | 触发、或将函数绑定到指定元素的 blur 事件 |
| `change()` | 触发、或将函数绑定到指定元素的 change 事件 |
| `click()` | 触发、或将函数绑定到指定元素的 click 事件 |
| `dblclick()` | 触发、或将函数绑定到指定元素的 double click 事件 |
| `delegate()` | 向匹配元素的当前或未来的子元素附加一个或多个事件处理器 |
| `die()` | 移除所有通过 live() 函数添加的事件处理程序。 |
| `error()` | 触发、或将函数绑定到指定元素的 error 事件 |
| `event.isDefaultPrevented()` | 返回 event 对象上是否调用了 event.preventDefault()。 |
| `event.pageX` | 相对于文档左边缘的鼠标位置。 |
| `event.pageY` | 相对于文档上边缘的鼠标位置。 |
| `event.preventDefault()` | 阻止事件的默认动作。 |
| `event.result` | 包含由被指定事件触发的事件处理器返回的最后一个值。 |
| `event.target` | 触发该事件的 DOM 元素。 |
| `event.timeStamp` | 该属性返回从 1970 年 1 月 1 日到事件发生时的毫秒数。 |
| `event.type` | 描述事件的类型。 |
| `event.which` | 指示按了哪个键或按钮。 |
| `focus()` | 触发、或将函数绑定到指定元素的 focus 事件 |
| `keydown()` | 触发、或将函数绑定到指定元素的 key down 事件 |
| `keypress()` | 触发、或将函数绑定到指定元素的 key press 事件 |
| `keyup()` | 触发、或将函数绑定到指定元素的 key up 事件 |
| `live()` | 为当前或未来的匹配元素添加一个或多个事件处理器 |
| `load()` | 触发、或将函数绑定到指定元素的 load 事件 |
| `mousedown()` | 触发、或将函数绑定到指定元素的 mouse down 事件 |
| `mouseenter()` | 触发、或将函数绑定到指定元素的 mouse enter 事件 |
| `mouseleave()` | 触发、或将函数绑定到指定元素的 mouse leave 事件 |
| `mousemove()` | 触发、或将函数绑定到指定元素的 mouse move 事件 |
| `mouseout()` | 触发、或将函数绑定到指定元素的 mouse out 事件 |
| `mouseover()` | 触发、或将函数绑定到指定元素的 mouse over 事件 |
| `mouseup()` | 触发、或将函数绑定到指定元素的 mouse up 事件 |
| `one()` | 向匹配元素添加事件处理器。每个元素只能触发一次该处理器。 |
| `ready()` | 文档就绪事件（当 HTML 文档就绪可用时） |
| `resize()` | 触发、或将函数绑定到指定元素的 resize 事件 |
| `scroll()` | 触发、或将函数绑定到指定元素的 scroll 事件 |
| `select()` | 触发、或将函数绑定到指定元素的 select 事件 |
| `submit()` | 触发、或将函数绑定到指定元素的 submit 事件 |
| `toggle()` | 绑定两个或多个事件处理器函数，当发生轮流的 click 事件时执行。 |
| `trigger()` | 所有匹配元素的指定事件 |
| `triggerHandler()` | 第一个被匹配元素的指定事件 |
| `unbind()` | 从匹配元素移除一个被添加的事件处理器 |
| `undelegate()` | 从匹配元素移除一个被添加的事件处理器，现在或将来 |
| `unload()` | 触发、或将函数绑定到指定元素的 unload 事件 |

## 效果

- `hide|show|toggle(speed?: number|string, callback?: function)`
  - `speed`：取值为`["slow", "fast", "normal", <ms>]`
  - `callback`：隐藏或显示完成后所执行的函数
- `fadeIn|fadeOut|fadeToggle|fadeTo(speed?: number|string[, opacity?], callback?: function)`
  - 只有`fadeTo()`允许渐变为给定的不透明度
- `slideDown|slideUp|slideToggle(speed?: number|string, callback?: function)`
- `animate(params: object, speed?: number|string, callback?: function)`
  - `params`：CSS 的属性键值对
  - 使用 Camel 标记法书写所有的属性名
  - 通过`[+=, -=]`可以定义相对值
  - 属性的动画值可以设置为`"[show", "hide", toggle"]`
  - 多个`animate()`调用会创建内部队列。然后逐一运行
- `stop(stopAll, goToEnd)`
  - `stopAll`：是否应该清除动画队列。默认是`false`，即仅停止活动的动画，允许任何排入队列的动画向后执行
  - `goToEnd`：是否立即完成当前动画。默认是`false`

## HTML

- 添加
  - `append(e: string|HTMLElement, ...)`：在被选元素的结尾插入文本或元素，插入数量不限
  - `appendTo(selector: string)`：在被选元素的结尾插入指定内容
  - `prepend(e: string|HTMLElement, ...)`：在被选元素的开头插入文本或元素，插入数量不限
  - `prependTo(selector: string)`：在被选元素的开头插入指定内容
  - `after(e: string|HTMLElement, ...)`：在被选元素之后插入文本或元素，插入数量不限
  - `insertAfter(selector: string)`：在被选元素之后插入 HTML 标记或已有的元素，如果该方法用于已有元素，这些元素会被从当前位置移走，然后被添加到被选元素之后
  - `before(e: string|HTMLElement, ...)`：在被选元素之前插入文本或元素，插入数量不限
  - `insertBefore(selector: string)`：在被选元素之前插入 HTML 标记或已有的元素，如果该方法用于已有元素，这些元素会被从当前位置移走，然后被添加到被选元素之前
- 删除
  - `remove(selector?: string)`：删除被选元素及其子元素，支持通过`selector`筛选以得到需要移除的元素列表
  - `empty()`：删除被选元素的子元素
- 复制
  - `clone(includeEvents: boolean = false)`：生成被选元素的副本，包含子节点、文本和属性，可选是否复制元素的所有事件处理
- 文档操作
  - `text(text?: string|(index: number, originalText: string) => string)`：设置或返回所选元素的文本内容，支持通过回调函数进行修改
  - `html(html?: string|(index: number, originalHtml: string) => string)`：设置或返回所选元素的内容（包括 HTML 标记），支持通过回调函数进行修改
- 属性操作
  - `val(value?: string|(index: number, originalValue: string) => string)`：设置或返回表单字段的值，支持通过回调函数进行修改
  - `addClass(className: string)`：添加 class 属性
  - `removeClass(className: string)`：删除 class 属性
  - `toggleClass(className: string)`：对被选元素进行添加/删除类的切换操作
  - `hasClass(className: string)`：检查被选元素是否包含指定的类
  - `attr(attrName: string, value?: string|(index: number, originalValue: any) => string) | attr(params :object)`：设置或返回标签的属性值，支持通过回调函数进行修改，支持同时设置多个属性
  - `removeAttr(attrName: string)`：从被选元素中移除属性
- CSS 操作
  - `css(porpName: string|object, value?: string)`：设置或返回被选元素的一个或多个样式属性
  - `width()`：设置或返回元素的宽度（不包括内边距、边框或外边距）
  - `height()`：设置或返回元素的高度（不包括内边距、边框或外边距）
  - `innerWidth()`：返回元素的宽度（包括内边距）
  - `innerHeight()`：返回元素的高度（包括内边距）
  - `outerWidth(includeOuterMargin?: boolean = false)`：返回元素的宽度（包括内边距和边框），若`includeOuterMargin`则还包括外边距
  - `outerHeight(includeOuterMargin?: boolean = false)`：返回元素的高度（包括内边距和边框），若`includeOuterMargin`则还包括外边距
  - `offset(value?: object|(index: number, oldOffset: object) => object)`：返回或设置匹配元素相对于文档的偏移（位置），该方法返回的对象格式为`{top, left}`，以像素计。此方法只对可见元素有效
  - `offsetParent()`：返回最近的祖先定位元素，定位元素指的是元素的 CSS 的`position`属性被设置为`relative, absolute, fixed`的元素
  - `position()`：返回或匹配元素相对于父元素的位置（偏移），该方法返回的对象格式为`{top, left}`，以像素计。此方法只对可见元素有效
  - `scrollLeft(position?: number)`：返回或设置第一个匹配元素的滚动条的水平滚动条位置
  - `scrollTop(offset?: number)`：返回或设置匹配元素的滚动条的垂直滚动条位置，该方法对于可见元素和不可见元素均有效，当用于获取值时，该方法只返回第一个匹配元素的`offset`，当用于设置值时，该方法设置所有匹配元素的`offset`
- 包裹
  - `wrap(wrapper: string|HTMLElement)`：把每个被选元素放置在指定的 HTML 内容或元素中，已存在的元素不会被移动，只会被复制，并包裹被选元素
  - `wrapAll(wrapper: string|HTMLElement)`：在指定的 HTML 内容或元素中放置所有被选的元素，已存在的元素不会被移动，只会被复制，并包裹被选元素
  - `wrapInner(wrapper: string|HTMLElement)`：使用指定的 HTML 内容或元素，来包裹每个被选元素中的所有内容，已存在的元素不会被移动，只会被复制，并包裹被选元素
  - `unwrap()`：删除被选元素的父元素

## 遍历

- 祖先
  - `parent()`：返回被选元素的直接父元素
  - `parents(selector?: string)`：从父元素开始返回被选元素的所有祖先元素，一直到文档的根元素`<html>`
  - `closest(selector: string)`：从当前元素开始返回被选元素的符合条件的祖先元素
  - `parentUntil(e?: string)`：返回介于两个给定元素之间的所有祖先元素
- 后代
  - `children(selector?: string)`：返回被选元素的所有直接子元素，可以使用可选参数来过滤对子元素的搜索
  - `find(selector: string)`：返回被选元素的所有符合条件的后代元素
  - `contents()`：获得匹配元素集合中每个元素的子节点，包括文本和注释节点
- 同胞
  - `siblings(selector?: string)`：返回被选元素的所有同胞元素，可以使用可选参数来过滤对同胞元素的搜索
  - `next()`：返回被选元素的下一个同胞元素
  - `nextAll()`：返回被选元素的所有之后的同胞元素
  - `nextUntil(selector: string)`：返回介于两个给定参数之间的所有之后的同胞元素
  - `prev()`：返回被选元素的上一个同胞元素
  - `prevAll()`：返回被选元素的所有之前的同胞元素
  - `prevUntil()`：返回介于两个给定参数之间的所有之前的同胞元素
- 过滤
  - `flrst()`：返回被选元素的首个元素
  - `last()`：返回被选元素的最后一个元素
  - `eq(index: number)`：返回被选元素中带有指定索引号的元素
  - `filter(selector: string)`：匹配的元素会被返回，不匹配的元素会被删除
  - `not(selector: string)`：不匹配的元素会被返回，匹配的元素会被删除
  - `has(selector: string)`：将匹配元素集合缩减为拥有匹配指定选择器或 DOM 元素的后代的子集
  - `slice(start?: number = 0, end?: number)`：把匹配元素集合缩减为指定的指数范围的子集，索引支持负数
  - `end()`：结束当前链条中的最近的筛选操作，并将匹配元素集还原为之前的状态
- 替换
  - `replaceWith(content: string|HTMLElement)`：用指定的 HTML 内容或元素替换被选元素
  - `replaceAll(selector: string|HTMLElement)`：用指定的 HTML 内容或元素替换被选元素
- 集合
  - `add(selector: string|html|HTMLElement, context?: )`：将元素添加到匹配元素的集合中
  - `andSelf()`：合并指定元素与集合本身
- 迭代
  - `map(callback: (index: number, element: HTMLElement) => any)`：把每个元素通过函数传递到当前匹配集合中，生成包含返回值的新的 jQuery 对象
  - `each(callback: (index: number, element: HTMLElement) => any)`：为每个匹配元素规定运行的函数
- 其他
  - `is(selector: string)`：如果至少有一个元素匹配则返回`true`

## 选择器

| 选择器 | 选取 |
|:-|:-|
| `*` | 所有元素 |
| `#foo` | `id="foo"`的元素 |
| `.foo` | 所有`class="foo"`的元素 |
| `foo` | 所有`<foo>`元素 |
| `.foo.bar` | 所有`class="foo"`且`class="bar"`的元素 |
| `:first` | 第一个符合的元素 |
| `:last` | 最后一个符合的元素 |
| `:even` | 所有偶数元素 |
| `:odd` | 所有奇数元素 |
| `:eq(i)` | 索引为`i`的元素 |
| `:gt(i)` | 所有索引大于`i`的元素 |
| `:lt(i)` | 所有索引小于`i`的元素 |
| `:not(selector)` | 所有不符合的元素 |
| `:header` | 所有标题元素(`<h1>...<h6>`) |
| `:animated` | 所有动画元素 |
| `:contains(text)` | 所有包含指定字符串的元素 |
| `:empty` | 所有无子元素的元素 |
| `:hidden` | 所有隐藏的元素 |
| `:visible` | 所有可见的元素 |
| `s1,s2` | 所有带有匹配选择的元素 |
| `[foo]` | 所有带有`foo`属性的元素 |
| `[foo=bar]` | 所有`foo`属性的值等于`bar`的元素 |
| `[foo!=bar]` | 所有`foo`属性的值不等于`bar`的元素 |
| `[foo$=bar]` | 所有`foo`属性的值以`bar`结尾的元素 |
| `:input` | 所有`<input>`元素 |
| `:text` | 所有`<input type="text">`元素 |
| `:password` | 所有`<input type="password">`元素 |
| `:radio` | 所有`<input type="radio">`元素 |
| `:checkbox` | 所有`<input type="checkbox">`元素 |
| `:submit` | 所有`<input type="submit">`元素 |
| `:reset` | 所有`<input type="reset">`元素 |
| `:button` | 所有`<input type="button">`元素 |
| `:image` | 所有`<input type="image">`元素 |
| `:file` | 所有`<input type="file">`元素 |
| `:enabled` | 所有激活的`<input>`元素 |
| `:disabled` | 所有禁用的`<input>`元素 |
| `:selected` | 所有被选取的`<input>`元素 |
| `:checked` | 所有被选中的`<input>`元素 |

## AJAX

- `load(url: string, data?: object, callback?: (response, status, xhr) => any)`：从服务器加载数据，并把返回的数据放入被选元素中，可以把选择器添加到 URL 参数末尾并以空格分隔
- `serialize()`：序列化表单值，创建 URL 编码文本字符串，只会将成功的控件序列化为字符串，如果要表单元素的值包含到序列字符串中，元素必须使用 name 属性
- `serializeArray()`序列化表单值来创建对象数组（名称和值）
- `$.ajaxSetup(settings: object)`：为所有 AJAX 请求设置默认 URL 和 success 函数
- `$.ajaxStart(callback: function)`：在 AJAX 请求发送前执行函数，它是一个 Ajax 事件
- `$.ajaxSend((event, xhr, options) => any)`：在 AJAX 请求开始时执行函数，它是一个 Ajax 事件
- `$.ajaxStop(function)`：在 AJAX 请求结束时执行函数，它是一个 Ajax 事件
- `$.ajaxSuccess((event, xhr, options) => any)`：在 AJAX 请求成功时执行函数，它是一个 Ajax 事件
- `$.ajaxError((event, xhr, options, exception) => any)`：在 AJAX 请求发生错误时执行函数，它是一个 Ajax 事件
- `$.ajaxComplete((event, xhr, options) => any)`：在 AJAX 请求完成时执行函数，它是一个 Ajax 事件
- `$.get(url: string, data?: object, callback?: (response, status, xhr) => any, dataType?: string)`：通过`GET`请求从服务器上请求数据
- `$.post(url: string, data?: object, callback?: (response, status, xhr) => any, dataType?: string)`：通过`POST`请求从服务器上请求数据
- `$.param(param: object|Array<object>, traditional?: false)`：创建对象或对象数组的序列化表示，通过`traditional=true`可模拟 jQuery 1.4 之前版本的行为
- `$.getJSON(url: string, data?: object, callback?: (response, status, xhr) => any)`：通过`GET`请求从服务器上请求载入 JSON 数据，该函数是简写的 Ajax 函数
- `$.getScript(url: string, callback?: (response, status) => any)`：通过`GET`请求从服务器上请求载入并执行 JavaScript 文件，该函数是简写的 Ajax 函数
- `$.ajax(settings: obejct)`：通过 HTTP 请求加载远程数据
  - `async?: boolean = true`：是否异步请求
  - `type?: string = "GET"`：请求方式，还包括`["POST", "PUT", "DELETE"]`，但除了`["GET", "POST"]`之外其他方式部分浏览器可能不支持
  - `url?: string`：发送请求的地址，默认为当前页地址
  - `data?: string`：发送到服务器的数据。将自动转换为请求字符串格式
  - `contentType?: string = "application/x-www-form-urlencoded"`：发送信息至服务器时内容编码类型，默认值适合大多数情况
  - `dataType?: string`：预期服务器返回的数据类型，如果不指定将智能判断，
  - `success?: function`：请求成功后的回调函数
  - `error?: (xhr, errMsg, errObj) => any`：请求失败时调用此函数，如果发生了错误，错误信息可能得到`[null, "timeout", "error", "notmodified", "parsererror"]`
  - `complete?: (xhr, ts) => any`：请求完成后回调函数 (请求成功或失败之后均调用)，参数是`XMLHttpRequest`对象和一个描述请求类型的字符串
  - `timeout?: number`：设置请求超时时间（毫秒）。此设置将覆盖全局设置
  - `username?: string`：用于响应 HTTP 访问认证请求的用户名
  - `password?: string`：用于响应 HTTP 访问认证请求的密码
  - `beforeSend?: (xhr) => any`：发送请求前可修改`XMLHttpRequest`对象的函数，这是一个 Ajax 事件，返回 false 可以取消本次 Ajax 请求
  - `cache?: boolean`：是否缓存页面，默认为`true`（除了`dataType`为`script/jsonp`）
  - `context?: object`：用于设置 Ajax 相关回调函数的上下文，让回调函数内 this 指向这个对象
  - `dataFilter: (data, type) => any`：给 Ajax 可用类型为`xml/html/script/json/jsonp/text`
  - `global?: boolean = true`：是否触发全局 AJAX 事件
  - `ifModified?: boolean = false`：是否仅在服务器数据改变时获取新数据
  - `jsonp?: string`：在一个 jsonp 请求中重写回调函数的名字
  - `jsonpCallback?: string`：为 jsonp 请求指定一个回调函数名
  - `processData?: boolean = true`：是否转换通过`data`选项传递进来的数据为字符串
  - `scriptCharset?: string`：只有当请求时 dataType 为 `[jsonp, "script"]`，并且 type 是 GET 才会用于强制修改 charset
  - `traditional?: boolean = false`：是否用传统的方式来序列化数据
  - `xhr?: () => XMLHttpRequest`：用于重写或者提供一个增强的 XMLHttpRequest 对象。这个参数在 jQuery 1.3 以前不可用

## DOM 元素

- `get(index: number)`：获得由选择器指定的 DOM 元素
- `index()`：返回指定元素相对于其他指定元素的 index 位置
- `size()`：返回被选择器匹配的元素的数量，也可以用`.length`获取
- `toArray()`：以数组的形式返回选择器匹配的元素
