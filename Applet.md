[TOC]

# 填坑

+ 自定义组件：`https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/`

# 代码组成

## JSON

```json
{
    "prop": value,
    ...
}
```

+ 说明
  + `"prop"`：必须用双引号括起来属性名
  + `value`：只能是`number, string, boolean, boolean, array, object, null`
  + `comments`：不允许有注释
  
+ `sitemap.json`

  ```json
  // 说明：json不允许注释
  // 若没有sitamap.json则默认所有页面都能被索引
  {
      "desc": <string>, // 描述
       "rules": [
           {
               "action": "allow | disallow", // 设置是否被索引
               "page": "path | *", // 设置索引页面
               "params": <array>, // 设置优先索引参数
               "priority": <number>
           }
       ]
  }
  ```

  

## WXML

```html
<!-- comments -->
<tag [prop="value[ value...]" ...]>...{{exp}}...</tag>
...
```

+ 说明

  + `"prop"`：不需要双引号，可以有多个属性
  + `value`：可以有多个值，用空格分隔
  + `comments`：通过`<!-- comments -->`进行注释
  + `<tag></tag>`：标签可多层嵌套
  + `{{exp}}`：数据绑定，可以是常量、变量、表达式等，作用如同执行`.js`代码，表达式中用到的变量应在文`.js`文件中的`data`内声明

+ 逻辑

  + `wx:if="{{condition}}"`：判断是否需要渲染该代码块
  + `wx:elif="{{condition}"`：与`wx:if, wx:else`搭配使用
  + `wx:else="{{condition}}"`：与`wx:if, wx:elif`搭配使用
  + `wx:for="{{array}}"`：使用数组中各项的数据重复渲染该组件。默认数组的当前项的下标变量名默认为`index`，数组当前项的变量名默认为`item`
  + `wx:for-item="itemName"`：指定数组当前元素的变量名
  + `wx:for-index="{{indexName}}"`：指定数组当前下标的变量名
  + `wx:key="prop|this"`：指定列表中项目的唯一的标识符，可以是元素的属性，也可以是元素本身；当数据改变触发渲染层重新渲染的时候，会校正带有`key`的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率

+ 共同属性

  | 属性名         | 类型           | 描述           | 注解                                     |
  | -------------- | -------------- | -------------- | ---------------------------------------- |
  | `id`           | `String`       | 组件的唯一标识 | 整个页面唯一                             |
  | `class`        | `String`       | 组件的样式类   | 在对应的 WXSS 中定义的样式类             |
  | `style`        | `String`       | 组件的内联样式 | 可以动态设置的内联样式                   |
  | `hidden`       | `Boolean`      | 组件是否显示   | 所有组件默认显示                         |
  | `data-*`       | `Any`          | 自定义属性     | 组件上触发的事件时，会发送给事件处理函数 |
  | `bind*/catch*` | `EventHandler` | 组件的事件     |                                          |

## WXSS

```css
{
    "prop": value[ value...];
    ...
}
```

+ 说明

  + 单位

    + 尺寸单位：`px, rpx`，关系：`1rpx = screenPixel/750 px`

  + 引用

    + `@import 'path'`：引用外部的`wxss`文件
    + `<tag style="prop: value[;...]">...</tag>`：内联样式

  + 选择器

    | 类型         | 选择器     | 样例            | 样例描述                                       |
    | ------------ | ---------- | --------------- | ---------------------------------------------- |
    | 类选择器     | `.class`   | `.intro`        | 选择所有拥有`class="intro"`的组件              |
    | `id`选择器   | `#id`      | `#firstname`    | 选择拥有`id="firstname"`的组件                 |
    | 元素选择器   | `element`  | `view checkbox` | 选择所有文档的`view`组件和所有的`checkbox`组件 |
    | 伪元素选择器 | `::after`  | `view::after`   | 在`view`组件后边插入内容                       |
    | 伪元素选择器 | `::before` | `view::before`  | 在`view`组件前边插入内容                       |

  + 优先级：`important(oo) > style(1000) > id(100) > class(10) > element(1)`

# 主体

## 程序

+ `App(obj)`：程序构造器，其中`onLaunch, onShow, onHide`是程序的生命周期函数，通过`getApp()`可以获取当前程序实例

  | 参数属性          | 类型       | 描述                                                         |
  | ----------------- | ---------- | ------------------------------------------------------------ |
  | `onLaunch`        | `Function` | 当小程序初始化完成时，会触发`onLaunch`（全局只触发一次）     |
  | `onShow`          | `Function` | 当小程序启动，或从后台进入前台显示，会触发`onShow`           |
  | `onHide`          | `Function` | 当小程序从前台进入后台，会触发`onHide`                       |
  | `onError`         | `Function` | 当小程序发生脚本错误，或者`API`调用失败时，会触发`onError`并带上错误信息 |
  | `onSaveExitState` | `Function` | 保留页面中的状态，返回值为`{data, expireTimestamp}`          |
  | `其他字段`        | `任意`     | 可以添加任意的函数或数据到`Object`参数中，在`App`实例回调用`this`可以访问 |

+ `onLaunch`和`onShow`所带参数

  | 字段                     | 类型     | 描述                                                      |
  | ------------------------ | -------- | --------------------------------------------------------- |
  | `path`                   | `String` | 打开小程序的页面路径                                      |
  | `query`                  | `Object` | 打开小程序的页面参数`query`                               |
  | `scene`                  | `Number` | 打开小程序的场景值                                        |
  | `shareTicket`            | `String` |                                                           |
  | `referrerInfo`           | `Object` | 当场景为由从另一个小程序或公众号或`App`打开时，返回此字段 |
  | `referrerInfo.appId`     | `String` | 来源小程序或公众号或`App`的`appId`                        |
  | `referrerInfo.extraData` | `Object` | 来源小程序传过来的数据，`scene=1037`或`1038`时支持        |

+ 场景值

  https://developers.weixin.qq.com/miniprogram/dev/reference/scene-list.html

## 配置

### 全局配置

| 属性                             | 类型       | 描述                                   |
| -------------------------------- | ---------- | -------------------------------------- |
| `pages`                          | `string[]` | 必需属性，页面路径列表                 |
| `window`                         | `Object`   | 全局的默认窗口表现                     |
| `tabBar`                         | `Object`   | 底部 `tab` 栏的表现                    |
| `networkTimeout`                 | `Object`   | 网络超时时间                           |
| `debug`                          | `boolean`  | 是否开启`debug`模式，默认关闭          |
| `functionalPages`                | `boolean`  | 是否启用插件功能页，默认关闭           |
| `subpackages`                    | `Object[]` | 分包结构配置                           |
| `worker`                         | `string`   | `Worker` 代码放置的目录                |
| `requiredBackgroundModes`        | `string[]` | 需要在后台使用的能力，如「音乐播放」   |
| `plugins`                        | `Object`   | 使用到的插件                           |
| `preloadRule`                    | `Object`   | 分包预下载规则                         |
| `resizable`                      | `boolean`  | `iPad`小程序是否支持屏幕旋转，默认关闭 |
| `navigateToMiniProgramAppIdList` | `string[]` | 需要跳转的小程序列表                   |
| `usingComponent`                 | `Object`   | 全局自定义组件置                       |
| `permission`                     | `Object`   | 小程序接口权限相关设置                 |
| `sitemapLocation`                | `String`   | 必需属性，指明`sitemap.json`的位置     |
| `style`                          | `String`   | 指定使用升级后的`weui`样式             |

+ `pages`

  + 说明：用于指定小程序由哪些页面组成，每一项都对应一个页面的 路径（含文件名） 信息；文件名不需要写文件后缀，框架会自动去寻找对于位置的 `.json`, `.js`, `.wxml`, `.wxss` 四个文件进行处理；数组的第一项代表小程序的初始页面（首页）；小程序中新增/减少页面，都需要对 pages 数组进行修改

+ `window`

  + 说明：用于设置小程序的状态栏、导航条、标题、窗口背景色

  + 属性：

    | 属性                           | 类型       | 默认值     | 描述                                                         |
    | ------------------------------ | ---------- | ---------- | ------------------------------------------------------------ |
    | `navigationBarBackgroundColor` | `HexColor` | `#000000`  | 导航栏背景颜色                                               |
    | `navigationBarTextStyle`       | `string`   | `white`    | 导航栏标题颜色，仅支持 `black/white`                         |
    | `navigationBarTitleText`       | `string`   |            | 导航栏标题文字内容                                           |
    | `navigationStyle`              | `string`   | `default`  | 导航栏样式，仅支持以下值：`default` 默认样式，`custom` 自定义导航栏，只保留右上角胶囊按钮 |
    | `backgroundColor`              | `HexColor` | `#ffffff`  | 窗口的背景色                                                 |
    | `backgroundTextStyle`          | `string`   | `dark`     | 下拉`loading`的样式，仅支持 `dark/light`                     |
    | `backgroundColorTop`           | `string`   | `#ffffff`  | 顶部窗口的背景色，仅`iOS`支持                                |
    | `backgroundColorBottom`        | `string`   | `#ffffff`  | 底部窗口的背景色，仅`iOS`支持                                |
    | `enablePullDownRefresh`        | `boolean`  | `false`    | 是否开启全局的下拉刷新                                       |
    | `onReachBottomDistance`        | `number`   | `50`       | 页面上拉触底事件触发时距页面底部距离，单位为`px`             |
    | `pageOrientation`              | `string`   | `portrait` | 屏幕旋转设置，支持 `auto/portrait/landscape`                 |

+ `tabBar`

  + 说明：如果小程序是一个多`tab`应用（客户端窗口的底部或顶部有`tab`栏可以切换页面），可以通过`tabBar`配置项指定`tab`栏的表现，以及`tab`切换时显示的对应页面

  + 属性

    | 属性              | 类型       | 必填 | 默认值   | 描述                                          |
    | ----------------- | ---------- | ---- | -------- | --------------------------------------------- |
    | `color`           | `HexColor` | 是   |          | `tab`上的文字默认颜色，仅支持十六进制颜色     |
    | `selectedColor`   | `HexColor` | 是   |          | `tab`上的文字选中时的颜色，仅支持十六进制颜色 |
    | `backgroundColor` | `HexColor` | 是   |          | `tab`的背景色，仅支持十六进制颜色             |
    | `borderStyle`     | `string`   | 否   | `black`  | `tabbar`上边框的颜色， 仅支持 `black/white`   |
    | `list`            | `Array`    | 是   |          | `tab`的列表，最少`2`个、最多`5`个`tab`        |
    | `position`        | `string`   | 否   | `bottom` | `tabBar`的位置，仅支持`bottom/top`            |
    | `custom`          | `boolean`  | 否   | `false`  | 自定义`tabBar`                                |

  + 元素属性
  
    | 属性               | 类型     | 必填 | 说明                                                         |
    | ------------------ | -------- | ---- | ------------------------------------------------------------ |
    | `pagePath`         | `string` | 是   | 页面路径，必须在`pages`中先定义                              |
    | `text`             | `string` | 是   | `tab`上按钮文字                                              |
    | `iconPath`         | `string` | 否   | 图片路径，`icon`大小限制为`40kb`，建议尺寸为`81px * 81px`，不支持网络图片。当`position`为`top`时，不显示`icon` |
    | `selectedIconPath` | `string` | 否   | 选中时的图片路径，`icon`大小限制为`40kb`，建议尺寸为`81px * 81px`，不支持网络图片。当`position`为`top`时，不显示`icon` |
  
+ `networkTimeout`

  + 说明：各类网络请求的超时时间，单位均为毫秒

  + 属性

    | 属性            | 类型     | 必填 | 默认值  | 说明                         |
    | --------------- | -------- | ---- | ------- | ---------------------------- |
    | `request`       | `number` | 否   | `60000` | `wx.request`的超时时间       |
    | `connectSocket` | `number` | 否   | `60000` | `wx.connectSocket`的超时时间 |
    | `uploadFile`    | `number` | 否   | `60000` | `wx.uploadfile`的超时时间    |
    | `downloadFile`  | `number` | 否   | `60000` | `wx.downloadFile`的超时时间  |

+ `debug`

  + 说明：可以在开发者工具中开启 `debug` 模式，在开发者工具的控制台面板，调试信息以 `info` 的形式给出，其信息有 Page 的注册，页面路由，数据更新，事件触发等，可以帮助开发者快速定位问题

+ `functionalPages`

  + 说明：插件所有者小程序需要设置这一项的值为`{"independent": true}`来启用插件功能页

+ `subPackages`

  + 说明：启用分包加载时，声明项目分包结构；在小程序启动时，默认会下载主包并启动主包内页面，当用户进入分包内某个页面时，客户端会把对应分包下载下来，下载完成后再进行展示；整个小程序所有分包大小不超过`8M`，单个分包/主包大小不能超过`2M`

+ `workers`

  + 说明：使用`Worker`处理多线程任务时，设置 `Worker` 代码放置的目录

+ `requiredBackgroundModes`

  + 说明：申明需要后台运行的能力，类型为数组。目前支持：`audio：后台音乐播放；location：后台定位`

+ `plugins`

  + 说明：声明小程序需要使用的插件

+ `preloadRule`

  + 说明：声明分包预下载的规则；预下载分包行为在进入某个页面时触发，通过在 `app.json` 增加 `preloadRule` 配置来控制；`preloadRule` 中，`key` 是页面路径，`value` 是进入此页面的预下载配置，每个配置有以下几项

  + 对象元素属性

    | 字段       | 类型       | 必填 | 默认值 | 说明                                                         |
    | ---------- | ---------- | ---- | ------ | ------------------------------------------------------------ |
    | `packages` | `string[]` | 是   |        | 进入页面后预下载分包的 `root` 或 `name`。`__APP__` 表示主包  |
    | `network`  | `string`   | 否   | `wifi` | 在指定网络下预下载，可选值为： `all: 不限网络  wifi: 仅wifi下预下载` |

    

+ `resizable`

  + 说明：在`iPad`上运行的小程序可以设置支持屏幕旋转；在 `app.json` 的 `window` 段中或在页面 json 文件中通过配置 `"pageOrientation": "auto"`实现旋转

+ `navigateToMiniProgramAppIdList`

  + 说明：当小程序需要使用`wx.navigateToMiniProgram`接口跳转到其他小程序时，需要先在配置文件中声明需要跳转的小程序`appId`列表，最多允许填写`10`个

+ `usingComponents`

  + 说明：在此处声明的自定义组件视为全局自定义组件，在小程序内的页面或自定义组件中可以直接使用而无需再声明

+ `permissinn`

  + 说明：小程序接口权限相关设置。字段类型为 `Object`，结构`{scope.xxx: {"desc:"xxx"}}`

  + `scope`列表

    | `scope`                        | 对应接口                                               | 描述         |
    | ------------------------------ | ------------------------------------------------------ | ------------ |
    | `scope.userInfo`               | `wx.getUserInfo`                                       | 用户信息     |
    | `scope.userLocation`           | `wx.getLocation, wx.chooseLocation`                    | 地理位置     |
    | `scope.userLocationBackground` | `wx.userLocationBackground`                            | 后台定位     |
    | `scope.address`                | `wx.chooseAddress`                                     | 通讯地址     |
    | `scope.invoiceTitle`           | `wx.chooseInvoiceTitle`                                | 发票抬头     |
    | `scope.invoice`                | `wx.chooseInvoice`                                     | 获取发票     |
    | `scope.werun`                  | `wx.getWeRunData`                                      | 微信运动步数 |
    | `scope.record`                 | `wx.startRecord`                                       | 录音功能     |
    | `scope.writePhotosAlbum`       | `wx.saveImageToPhotosAlbum, Wx.saveVideoToPhotosAlbum` | 保存到相册   |
    | `scope.camera`                 | `camera`组件                                           | 摄像头       |

    

+ `sitemapLocation`

  + 说明：指明`sitemap.json`的位置；默认为在`app.json`同级目录下名字的 `sitemap.json` 文件

+ `style`

  + 说明：微信客户端 7.0 开始，UI 界面进行了大改版。小程序也进行了基础组件的样式升级。`app.json`中配置 `"style": "v2"`可表明启用新版的组件样式；本次改动涉及的组件有 `button, icon, radio, checkbox, switch, slider`

### 页面配置

| 属性                           | 类型       | 默认值     | 描述                                                         |
| ------------------------------ | ---------- | ---------- | ------------------------------------------------------------ |
| `navigationBarBackgroundColor` | `HexColor` | `#000000`  | 导航栏背景颜色                                               |
| `navigationBarTextStyle`       | `string`   | `white`    | 导航栏标题颜色，仅支持 `black, white`                        |
| `navigationBarTitleText`       | `string`   |            | 导航栏标题文字内容                                           |
| `navigationStyle`              | `string`   | `default`  | 导航栏样式，仅支持以下值：`default` 默认样式，`custom` 自定义导航栏，只保留右上角胶囊按钮 |
| `backgroundColor`              | `HexColor` | `#ffffff`  | 窗口的背景色                                                 |
| `backgroundTextStyle`          | `string`   | `dark`     | 下拉`loading`的样式，仅支持`dark` / `light`                  |
| `backgroundColorTop`           | `string`   | `#ffffff`  | 顶部窗口的背景色，仅`iOS`支持                                |
| `backgroundColorBottom`        | `string`   | `#ffffff`  | 底部窗口的背景色，仅`iOS`支持                                |
| `enablePullDownRefresh`        | `boolean`  | `false`    | 是否开启当前页面下拉刷新                                     |
| `onReachBottomDistance`        | `number`   | `50`       | 页面上拉触底事件触发时距页面底部距离，单位为`px`             |
| `pageOrientationstring`        | `string`   | `portrait` | 屏幕旋转设置，支持`auto, portrait, landscape`                |
| `disableScroll`                | `boolean`  | `false`    | 设置为`true`则页面整体不能上下滚动。只在页面配置中有效，无法在`app.json`中设置 |
| `disableSwipeBack`             | `boolean`  | `false`    | 禁止页面右滑手势返回                                         |
| `usingComponents`              | `Object`   |            | 页面自定义组件配置                                           |



### sitemap配置

`sitemap.json`结构为`{rules:{}}`，`rules`的对象属性如下

| 属性       | 类型       | 默认值      | 取值              | 取值说明                                                     |
| ---------- | ---------- | ----------- | ----------------- | ------------------------------------------------------------ |
| `action`   | `string`   | `allow`     | `allow, disallow` | 命中该规则的页面是否能被索引                                 |
| `page`     | `string`   |             | `*, <pagePath>`   | 必填；`*` 表示所有页面，不能作为通配符使用                   |
| `params`   | `string[]` | `[]`        |                   | 当`page`字段指定的页面在被本规则匹配时可能使用的页面参数名称的列表（不含参数值） |
| `matching` | `string`   | `inclusive` |                   | 当`page`字段指定的页面在被本规则匹配时，此参数说明`params`匹配方式 |
| `priority` | `Number`   |             |                   | 优先级，值越大则规则越早被匹配，否则默认从上到下匹配         |

`matching`取值说明

| 取值        | 说明                                                   |
| ----------- | ------------------------------------------------------ |
| `exact`     | 当小程序页面的参数列表等于`params`时，规则命中         |
| `inclusive` | 当小程序页面的参数列表包含`params`时，规则命中         |
| `exclusive` | 当小程序页面的参数列表与`params`交集为空时，规则命中   |
| `partial`   | 当小程序页面的参数列表与`params`交集不为空时，规则命中 |



## 页面

+ `Page(obj)`：页面构造器，`onLoad, onReady, onShow, onHide, onUnload`是页面的生命周期函数，`onPullDownRefresh, onReachBottom, onShareAppMessage, onPageScroll`是页面的用户行为

  | 参数属性            | 类型       | 描述                                                         |
  | ------------------- | ---------- | ------------------------------------------------------------ |
  | `data`              | `Object`   | 页面的初始数据                                               |
  | `onLoad`            | `Function` | 生命周期函数--监听页面加载，触发时机早于`onShow`和`onReady`  |
  | `onReady`           | `Function` | 生命周期函数--监听页面初次渲染完成                           |
  | `onShow`            | `Function` | 生命周期函数--监听页面显示，触发事件早于`onReady`            |
  | `onHide`            | `Function` | 生命周期函数--监听页面隐藏                                   |
  | `onUnload`          | `Function` | 生命周期函数--监听页面卸载                                   |
  | `onPullDownRefresh` | `Function` | 页面相关事件处理函数--监听用户下拉动作，需要在`app.json`的`window`选项中或页面配置`page.json`中设置`enablePullDownRefresh`为`true`。当处理完数据刷新后，`wx.stopPullDownRefresh()`可以停止当前页面的下拉刷新 |
  | `onReachBottom`     | `Function` | 页面上拉触底事件的处理函数，可以在`app.json`的`window`选项中或页面配置`page.json`中设置触发距离`onReachBottomDistance`。在触发距离内滑动期间，本事件只会被触发一次 |
  | `onShareAppMessage` | `Function` | 用户点击右上角转发，只有定义了此事件处理函数，右上角菜单才会显示“转发”按钮，在用户点击转发按钮的时候会调用，此事件需要`return`一个`Object`，包含`title`和`path`两个字段，用于自定义转发内容 |
  | `onPageScroll`      | `Function` | 页面滚动触发事件的处理函数，参数为`Object`，包含`scrollTop`字段，表示页面在垂直方向已滚动的距离（单位`px`） |
  | `onResize`          | `Function` | 页面尺寸改变时触发                                           |
  | `onTabItemTap`      | `Function` | 当前是`tab`页时，点击`tab`时触发                             |
  | `其他`              | `Any`      | 可以添加任意的函数或数据，在`Page`实例的其他函数中用`this`可以访问 |

+ 函数

  + `this.setData(data, ?callback)`：传入`object`更新`data`字段的数据并且传递至渲染层
  + `wx.navigateTo(obj)`：传入含有`url`属性的`object`往当前页面栈多推入一个页面，只能打开非`tabBar`页面
  + `wx.navigateBack()`：退出当前页面栈的最顶上页面
  + `wx.redirectTo(obj)`：替换页面栈的最顶上页面为新的页面，`tabBar`页面初始化之后不会被销毁，只能打开非`tabBar`页面
  + `wx.switchTab(obj)`：清空页面栈（除非已经声明为`tabBar`页）然后切到新的页面，只能打开`tabBar`页面
  + `wx.reLaunch(obj)`：重加载，页面全部出栈，只留下新的页面，可以打开任意页面
  
+ 布局

  + 设置容器
    + `display: flex`：声明使用`flex`布局
    + `flex-direction: row(default) | row-reverse | column |column-reverse`：通过设置坐标轴，来设置项目排列方向，默认为主轴向右，交叉轴向下
    + `flex-wrap: nowrap(default) | wrap | wrap-reverse`：设置溢出时是否换行以及向下或向上换行
    + `justify-content: flex-start(default) | flex-end | center |space-between | space-around | space-evenly`：设置项目在主轴方向上对齐方式，以及分配项目之间及其周围多余的空间
    + `align-items: stretch(default) | center  | flex-end | baseline | flex-start`：设置项目在行中的对齐方式，即在主轴单行排列下在交叉轴方向上的对齐方式
    + `align-content: stretch(default) | flex-start | center |flex-end | space-between | space-around | space-evenly`：多行排列时，设置行在交叉轴方向上的对齐方式，以及分配行之间及其周围多余的空间
  + 设置项目
    + `order: 0(default) | <integer>`：设置项目沿主轴方向上的排列顺序，数值越小，排列越靠前
    + `flex-shrink: 1(default) | <number>`：当项目在主轴方向上溢出时，通过设置项目收缩因子来压缩项目适应容器。属性值为项目的收缩因子，属性值取非负数
    + `flex-grow: 0(default) | <number>`：当项目在主轴方向上还有剩余空间时，通过设置项目扩张因子进行剩余空间的分配。属性值为项目的扩张因子，属性值取非负数
    + `flex-basis:auto(default) | <length>`：当容器为`row | row-reverse`时，`flex-basis, width`同时存在，`flex-basis`优先级高于`width`；当容器为`column | column-reverse`时，`flex-basis, height`同时存在，`flex-basis`优先级高于`height`；需要注意的是，当`flex-basis`和`width | height`，其中一个属性值为`auto`时，非`auto`的优先级更高
    + `flex: none | auto | @flex-grow @flex-shrink @flex-basis`：是`flex-grow, flex-shrink, flex-basis`的简写方式。值设置为`none`，等价于`0 0 auto`。值设置为`auto`，等价于`1 1 auto`
    + `align-self: auto(default) | flex-start | flex-end |center | baseline | stretch`：设置项目在行中交叉轴方向上的对齐方式，用于覆盖容器的`align-items`，这么做可以对项目的对齐方式做特殊处理。默认属性值为`auto`，继承容器的`align-items`值，当容器没有设置`align-items`时，属性值为`stretch`

## 组件

+ `<template name|is>`：模板；通过`<template name></template>`创建，`<template is data/>`引用，其中`data`为对象，模板内部使用对象的属性名进行模板创建

+ `<script src>`：引用；通过`<script src/>`引入外部`.js`文件，组件内可以插入`.js`代码

+ `<import src>`：引用；通过`<import src/>`进行引用，只会`import`目标文件中定义的`template`，而不会`import`目标文件中`import`的`template`，简言之就是 import 不具有递归的特性

+ `<include src>`：引用；将目标文件中除`<template/>, <wxs/>`外的整个代码引入，相当于拷贝到`include`位置

+ `<wxs module src>`：创建`js`代码，在内部通过`module.exports`指定输出数据，或通过`src`指定`.wxs`文件

+ `<image src mode lazy-load binderror bindload>`：图片；各属性如下

  | 属性名      | 类型          | 默认值          | 描述                                                         |
  | ----------- | ------------- | --------------- | ------------------------------------------------------------ |
  | `src`       | `String`      |                 | 图片资源地址                                                 |
  | `mode`      | `String`      | `"scaleToFill"` | 图片裁剪、缩放的模式                                         |
  | `lazy-load` | `Boolean`     | `false`         | 图片懒加载。只针对`page`与`scroll-view`下的`image`有效       |
  | `binderror` | `HandleEvent` |                 | 当错误发生时触发事件，事件对象`event.detail = {errMsg: 'xxx'}` |
  | `bindload`  | `HandleEvent` |                 | 当图片载入完毕时触发事件，事件对象`event.detail = {height:'xxxpx', width:'xxxpx'}` |
  
+ `<video>`：视频原生组件

+ `<map>`：地图原生组件

+ `<canvas>`：画布原生组件

+ `<picker>`：弹出式选择器原生组件

## 反馈

+ 触摸反馈
  + `hover-class="class"`：该属性用来改变触摸时的样式
  + `loading="{{boolean}}"`：该属性用来指定是否显示加载状态

## 通信

+ `wx.request({url, data, header, method, dataType, success, fail, complete})`：往服务器传递数据或者从服务器拉取信息，详细参数如下

  | 参数名     | 类型            | 必填 | 默认值 | 描述                                                         |
  | ---------- | --------------- | ---- | ------ | ------------------------------------------------------------ |
  | `url`      | `String`        | 是   |        | 开发者服务器接口地址                                         |
  | `data`     | `Object/String` | 否   |        | 请求的参数                                                   |
  | `header`   | `Object`        | 否   |        | 设置请求的`header`，`header`中不能设置`Referer`，默认`header['content-type'] = 'application/json'` |
  | `method`   | `String`        | 否   | `GET`  | （需大写）有效值：`OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT` |
  | `dataType` | `String`        | 否   | `json` | 回包的内容格式，如果设为`json`，会尝试对返回的数据做一次 `JSON`解析 |
  | `success`  | `Function`      | 否   |        | 收到开发者服务成功返回的回调函数，其参数是一个`Object`       |
  | `fail`     | `Function`      | 否   |        | 接口调用失败的回调函数                                       |
  | `complete` | `Function`      | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |

  收到回包参数

  | 参数名       | 类型            | 描述                                     |
  | ------------ | --------------- | ---------------------------------------- |
  | `data`       | `Object/String` | 开发者服务器返回的数据                   |
  | `statusCode` | `Number`        | 开发者服务器返回的`HTTP`状态码           |
  | `header`     | `Object`        | 开发者服务器返回的`HTTP Response Header` |

## 登录

+ `wx.login({success, fail, complete})`：获取一个带有时效性的微信登陆凭证，回包`res.code`用以登录，在利用`wx.request({url, data:{code}, success, fail, complete})`登录后得到回包，通过`res.data.sessionId`可以得到开发者服务器和微信服务器之间的会话密钥

# API

## `wx.on*`

## `wx.get*`

+ `wx.getSystemInfoSync()`：同步获取系统信息，返回对象包含以下属性

  | 属性              | 说明             |
  | ----------------- | ---------------- |
  | `brand`           | 手机品牌         |
  | `model`           | 手机型号         |
  | `platform`        | 客户端平台       |
  | `system`          | 操作系统版本     |
  | `version`         | 微信版本号       |
  | `SDKVersion`      | 小程序基础库版本 |
  | `language`        | 微信设置的语言   |
  | `pixelRatio`      | 设备像素比       |
  | `screenWidth`     | 屏幕宽度         |
  | `screenHeight`    | 屏幕高度         |
  | `windowWidth`     | 可使用窗口宽度   |
  | `windowHeight`    | 可使用窗口高度   |
  | `fontSizeSetting` | 用户字体大小设置 |

+ `wx.getStorage/wx.getStorageSync(key[, success, fail, complete])`：获取本地数据存储，通过回包的`res.data`获取数据，其中的`key`是本地缓存中指定的`key`

+ `wx.getNetworkType({success, fail, complete})`：获取网络状态，通过回包的`res.networkType`能够得到网络状态，网络状态包括`wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)`

+ `wx.getUserInfo`

+ `wx.getLocation`

+ `wx.getWeRunData`

## `wx.set*`

+ `wx.setStorage/wx.setStorageSync(key, data[, success, fail, complete])`：写入本地数据存储，通过回包的`res.data`获取数据，其中的`key`是本地缓存中指定的`key`，`data`是需要存储的内容，可以是`Object/String`

## `wx.show*`

+ `wx.showModal({title, content, confirmText, cancelText, success, fail, complete})`：显示模态对话框
+ `wx.showToast({title, icon, duration})`：显示弹窗提示

## `wx.choose*`

+ `wx.chooseLocation`
+ `wx.chooseAddress`
+ `wx.chooseInvoiceTitle`
+ `wx.chooseInvoice`

## `wx.start*`

+ `wx.startRecord`

## `wx.save*`

+ `wx.saveImageToPhotosAlbum`
+ `wx.saveVideoToPhotosAlbum`

## 其他接口

- `wx.canIUse('API|method|param|options|component|attribute|option')`：判断接口组件在当前宿主环境是否可用

- `wx.scanCode({success, fail, complete})`：获取二维码数据，通过回包的`res.result`能够获取二维码的信息

- `wx.navigateToMiniProgram(obj)`：打开另一个小程序；在开发者工具上调用此 API 并不会真实的跳转到另外的小程序，但是开发者工具会校验本次调用跳转是否成功

  | 属性         | 类型       | 说明                                                         |
  | ------------ | ---------- | ------------------------------------------------------------ |
  | `appId`      | `string`   | 要打开的小程序`appId`；必填                                  |
  | `path`       | `string`   | 打开的页面路径，如果为空则打开首页。path 中 ? 后面的部分会成为`query`，在小程序的 `App.onLaunch, App.onShow, Page.onLoad` 的回调函数或小游戏的`wx.onShow`回调函数、[wx.getLaunchOptionsSync](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/life-cycle/wx.getLaunchOptionsSync.html) 中可以获取到`query`数据 |
  | `extraData`  | `object`   | 需要传递给目标小程序的数据，目标小程序可在 `App.onLaunch, App.onShow` 中获取到这份数据 |
  | `envVersion` | `string`   | 要打开的小程序版本。仅在当前小程序为开发版或体验版时此参数有效。如果当前小程序是正式版，则打开的小程序必定是正式版，合法值包括`develop, trial, release(default)` |
  | `success`    | `function` | 接口调用成功的回调函数                                       |
  | `fail`       | `function` | 接口调用失败的回调函数                                       |
  | `complete`   | `function` | 接口调用结束的回调函数（调用成功、失败都会执行）             |

- `wx.userLocationBackground`

- `wx.authorize`

# 事件

## 事件类型

说明：以下属于冒泡事件，能够被`bind | catch`捕获，`bind`事件绑定不会阻止冒泡事件向上冒泡，`catch`事件绑定可以阻止冒泡事件向上冒泡；无特殊声明得一般都是非冒泡事件，如`submit, input, scroll`等

| 类型                 | 触发条件                                                     |
| -------------------- | ------------------------------------------------------------ |
| `touchstart`         | 手指触摸动作开始                                             |
| `touchmove`          | 手指触摸后移动                                               |
| `touchcancel`        | 手指触摸动作被打断，如来电提醒，弹窗                         |
| `touchend`           | 手指触摸动作结束                                             |
| `tap`                | 手指触摸后马上离开                                           |
| `longpress`          | 手指触摸后，超过`350ms`再离开，如果指定了事件回调函数并触发了这个事件，`tap`事件将不被触发 |
| `longtap`            | 手指触摸后，超过`350ms`再离开（推荐使用`longpress`事件代替） |
| `transitionend`      | 会在`WXSS transition`或`wx.createAnimation`动画结束后触发    |
| `animationstart`     | 会在一个`WXSS animation`动画开始时触发                       |
| `animationiteration` | 会在一个`WXSS animation`一次迭代结束时触发                   |
| `animationend`       | 会在一个`WXSS animation`动画完成时触发                       |
| `touchforcechange`   | 在支持`3D Touch`的`iPhone`设备，重按时会触发                 |

## 事件对象属性

| 属性             | 类型      | 说明                                                         |
| ---------------- | --------- | ------------------------------------------------------------ |
| `type`           | `String`  | 事件类型                                                     |
| `timeStamp`      | `Integer` | 页面打开到触发事件所经过的毫秒数                             |
| `mark`           | `Object`  | 类似`dataset`，但`mark` 会包含从触发事件的节点到根节点上所有的 `mark:` 属性值 |
| `target`         | `Object`  | 触发事件的组件的一些属性值集合                               |
| `currentTarget`  | `Object`  | 当前组件的一些属性值集合                                     |
| `detail`         | `Object`  | 自定义事件所携带的数据，如表单组件的提交事件会携带用户的输入，媒体的错误事件会携带错误信息，详见[组件](https://developers.weixin.qq.com/miniprogram/dev/component)定义中各个事件的定义 |
| `touches`        | `Array`   | 触摸事件，当前停留在屏幕中的触摸点信息的数组                 |
| `changedTouches` | `Array`   | 触摸事件，当前变化的触摸点信息的数组                         |

+ `target`和`currentTarget`对象属性

  | 属性      | 类型     | 说明                                          |
  | --------- | -------- | --------------------------------------------- |
  | `id`      | `String` | 当前组件的`id`                                |
  | `tagName` | `String` | 当前组件的类型                                |
  | `dataset` | `Object` | 当前组件上由`data-`开头的自定义属性组成的集合 |

+ `touch`和`changedTouches`对象属性

  | 属性               | 类型     | 说明                                                         |
  | ------------------ | -------- | ------------------------------------------------------------ |
  | `identifier`       | `Number` | 触摸点的标识符                                               |
  | `pageX, pageY`     | `Number` | 距离文档左上角的距离，文档的左上角为原点 ，横向为`X`轴，纵向为`Y`轴 |
  | `clientX, clientY` | `Number` | 距离页面可显示区域（屏幕除去导航条）左上角距离，横向为`X`轴，纵向为`Y`轴 |

## 事件绑定与冒泡捕获

+ `bind*`：可绑定事件有`contact, errror, getphonenumber, getuserinfo, longtap, tap, touchcancel, touchend, touchmove, touchstart`
+ `catch*`：可绑定事件有`longtap, tap, touchcancel, touchend, touchmove, touchstart`

# 开发

## 分工

| 成员           | 权限分配                           |
| -------------- | ---------------------------------- |
| 项目管理组成员 | 拥有所有权限                       |
| 开发组成员     | 开发者权限 / 体验者权限 / 数据分析 |
| 产品组成员     | 体验者权限 / 数据分析              |
| 测试组成员     | 体验者权限                         |

## 权限

| 权限         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| 开发者权限   | 可使用小程序开发者工具及开发版小程序进行开发                 |
| 体验者权限   | 可使用体验版小程序                                           |
| 登录         | 可登录小程序管理后台，无需管理员确认                         |
| 数据分析     | 使用小程序数据分析功能查看小程序数据                         |
| 开发管理     | 小程序提交审核、发布、回退                                   |
| 开发设置     | 设置小程序服务器域名、消息推送及扫描普通链接二维码打开小程序 |
| 暂停服务设置 | 暂停小程序线上服务                                           |

## 版本

| 版本       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 开发版本   | 使用开发者工具，可将代码上传到开发版本中。 开发版本只保留每人最新的一份上传的代码。点击提交审核，可将代码提交审核。开发版本可删除，不影响线上版本和审核中版本的代码 |
| 体验版本   | 可以选择某个开发版本作为体验版，并且选取一份体验版           |
| 审核中版本 | 只能有一份代码处于审核中。有审核结果后可以发布到线上，也可直接重新提交审核，覆盖原审核版本 |
| 线上版本   | 线上所有用户使用的代码版本，该版本代码在新版本代码发布后被覆盖更新 |

