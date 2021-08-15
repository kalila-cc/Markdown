- [React](#react)
  - [JSX 简介](#jsx-简介)
  - [元素渲染](#元素渲染)
  - [组件 & Props](#组件--props)
  - [State & 生命周期](#state--生命周期)
  - [事件处理](#事件处理)
  - [条件渲染](#条件渲染)
  - [列表 & Key](#列表--key)
  - [生命周期](#生命周期)
  - [高级](#高级)
  - [API](#api)

# React

## JSX 简介

```jsx
const value = initValue;
const element = <tag>{value}</tag>;
```

- 语法
  - `<tag>...</tag>`
  - 假如一个标签里面没有内容，使用`<tag />`来闭合标签
- 嵌入
  - `<tag attr={expr}>{expr}</tag>`
  - 因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 camelCase（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定，例外的情况是`[aria-*, data-*]`属性
  - React DOM 在渲染所有输入内容之前，默认会进行转义，这样可以有效地防止 XSS 攻击
- 转译
  - `React.createElement()`
  - Babel 会把 JSX 转译成一个名为`React.createElement()`函数调用
  - 实际上创建了一个`{ type: "tag", props: {...} }`这样的对象，它们被称为 React 元素，是不可变对象

## 元素渲染

- `ReactDOM.render(element, mountNode)`
  - 将一个 React 元素渲染到特定 DOM 节点

## 组件 & Props

- 函数组件
  - `function Component(props): ReactElement`
  - 函数组件本质上是 JavaScript 函数
- class 组件
  - `class Component extends React.Component { render(): ReactElement }`
  - 需要 ES6 以上使用
- 渲染组件
  - `<Component attr=value>...</Component>`
  - React 元素为用户自定义组件，属性通过`{attr: value}`作为 props 传入，由于 class 是关键字，因此使用 className 替代
  - 所有 React 组件都必须像纯函数一样保护它们的 props 不被更改，将事件处理函数通过 props 传入子组件可以实现变量提升
  - 自定义组件名称必须以大写字母开头，小写字母开头的组件会被视为原生 DOM 标签
  - 需在作用域内使用自定义组件
- 组合组件
  - 组件可以在其输出中引用其他组件，这就可以用同一组件来抽象出任意层次的细节

## State & 生命周期

```jsx
class Component extends React.Componemt {
  constructor(props) {
    super(props);
    // 进行数据初始化，构造函数是唯一可以给 this.state 赋值的地方
    this.state = { key: value };
    // 将事件处理函数声明为 class 中的方法，绑定以在回调中使用 this
    this.handleEvent = this.handleEvent.bind(this);
  }
  // 生命周期函数
  someLifeCycleMethod() {
    ...
  }
  /* 自定义方法进行数据更新
    不能直接修改 this.state.key，这不会重新渲染组件
    State 的更新可能是异步的
    State 的更新会被合并 */
  someMethod() {
    this.setState({
      key: value
    });
  }
  // 自定义事件处理函数
  handleEvent() {
    ....
  }
  // 实验性语法自定义事件处理函数并避免绑定 this
  handleEvent = () => {
    ...
  }
  render() {
    return (
      ...
    );
  }
}
```

## 事件处理

- 绑定
  - 通过类内定义方法并在`constructor`内使用`this.handleEvent = this.handleEvent.bind(this)`以绑定 this
  - 实验性功能是，通过类内以箭头函数`handleEvent = (...) => {}`形式定义方法避免绑定 this
- 传参
  - 一般情况下，通过`onEvent={handleEvent}`传参
  - 若携带参数，箭头函数形式使用`onEvent={(event) => this.handleEvent.bind(this, ...)}`，这有时候会产生性能问题
  - 若携带参数，构造声明形式使用`onEvent={this.handleEvent.bind(this, ...)}`
- 注意
  - React 事件的命名采用小驼峰式（camelCase），而不是纯小写，格式如`onEvent={handleEvent}`
  - 不能通过返回 false 的方式阻止默认行为。必须显式的使用 preventDefault

## 条件渲染

- `if-else`
  - 声明一个变量并使用`if-else`语句进行条件渲染
- `&&`
  - 通过`{expr && ReactElement}`进行条件渲染，若表达式为`false`会被自动忽略
- `?:`
  - 通过三目运算符`expr ? value1 : value2`进行条件渲染
- `null`
  - 通过在`render()`中返回`null`从而不进行任何渲染，返回`null`并不会影响组件的生命周期

## 列表 & Key

- 通过`<tag>{Array<ReactElement>}</tag>`可以直接插入多个 React 元素
- 通过为每个列表元素分配属性`key={string}`保证性能，key 帮助 React 识别哪些元素改变
- 默认使用索引用作为 key，但不建议使用
- 元素的 key 只有放在就近的数组上下文中才有意义（即不要在项目组件内而是项目列表元素上设置 key）
- key 只是在兄弟节点之间必须唯一

## 生命周期

- 挂载
  - `constructor()`：在实现构造函数时，应在其他语句之前前调用`super(props)`，只能在此直接修改`this.state`
  - `static getDerivedStateFromProps(props, state)`：在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容，此方法无权访问组件实例
  - `render()`：class 组件中唯一必须实现的方法
  - `componentDidMount()`：组件挂载后立即调用
- 更新
  - `static getDerivedStateFromProps(props, state)`：在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容，此方法无权访问组件实例
  - `shouldComponentUpdate(nextProps, nextState)`：根据返回值，判断组件的输出是否受当前 state 或 props 更改的影响，默认行为是 state 每次发生变化组件都会重新渲染，默认返回 true，但返回 false 并不会阻止子组件在 state 更改时重新渲染
  - `render()`
  - `getSnapshotBeforeUpdate(prevProps, prevState)`：在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给`componentDidUpdate()`
  - `componentDidUpdate(prevProps, prevState, snapshot)`：更新后立即调用，首次渲染不会执行此方法
- 卸载
  - `componentWillUnmount()`：在组件卸载及销毁之前直接调用
- 错误处理
  - `static getDerivedStateFromError(error)`：在后代组件抛出错误后在渲染阶段被调用。 它将抛出的错误作为参数，并返回一个值以更新 state
  - `componentDidCatch(error, info)`：在后代组件抛出错误后的提交阶段被调用，因此允许执行副作用，其中`info`包含有关组件引发错误的栈信息

## 高级

- Context
  - 通过`const xxx = React.createContext(any)`创建 context，通过`<xxx.Provider value={any}></xxx.Provider>`包裹需要使用 context 的部分，DOM 节点会往上寻找 Provider 得到 context 的 value，通过`this.context`也能访问，可以通过修改`xxx.displayName`来确定 context 要显示的内容，可以创建多个 context 对象嵌套以使用多个 context
- Refs
  - 通过在 DOM 组件使用`ref={string|function|Ref}`，如果使用字符串形式，可以在组件的其他函数使用`this.refs.xxx`引用该组件，如果使用回调函数形式，可以在函数中通过修改`this.xxx`使得之后的生命周期函数能够引用`this.xxx`，如果在`constructor()`中创建`this.xxx = React.createRef()`，可以使用`this.xxx.current`访问引用，如果引用绑定到 DOM 节点，将直接返回 DOM 节点本身，不能在函数组件上使用 ref 属性，但可以声明`const xxx = useRef(null)`并通过`xxx.current`访问引用，还可以在函数内定义类并返回`React.forwardRef((props, ref) => {...})`进行访问
- Fragment
  - 通过`<React.Fragment>...</React.Fragment>`返回多个元素，短语法是`<>...</>`
- Render Props
  - 通过定义一个类并返回形如`<Component render={(state) => element} />`并在 Component 的`render()`内部调用`this.props.render()`可以灵活为 element 绑定特定行为并作为一个新的组件，实际上可以直接用函数组件返回 React 组件实现
- StrictMode
  - 通过`<React.StrictMode>...</React.StrictMode>`包裹需要启用严格模式的部分，目前有助于：识别不安全的生命周期，关于使用过时字符串 ref API 的警告，关于使用废弃的 findDOMNode 方法的警告，检测意外的副作用，检测过时的 context API
- PropTypes
  - 通过`Component.propTypes = { prop: PropTypes.<type> }`可以确定属性类型，`<type>`可取值`[any, bool, number, string, object, symbol, array, func, node, element, elementType]`，可添加后缀`.isRequired`保证提供，并且支持通过函数限定特殊值，包括`[oneOf(Array<any>), oneOfType(Array<PropTypes.type>), shape({key: value}), arrayOf(PropTypes.type), objectOf(PropTypes.type)]`

## API

- React
  - Component
    - 内部
      - `props`：包括被该组件调用者定义的 props，需特别注意`this.props.children`是一个特殊的 prop，通常由 JSX 表达式中的子组件组成，而非组件本身定义
      - `state`：包含了随时可能发生变化的数据，只能在`constructor()`内对其进行修改
      - `setState(updater: object|(state, props) => object, callback?)`：将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件，并不会保证 state 的变更会立即生效，而是批量推迟更新
      - `forceUpdate(callback)`：强制让组件重新渲染，此操作仅跳过该组件的`shouldComponentUpdate()`
    - 外部
      - `defaultProps`：用于为 Class 组件添加默认 props
      - `displayName`：多用于调试消息，通常不需要设置它，因为它可以根据函数组件或 class 组件的名称推断出来
- ReactDOM
  - `render(element, container, callback?)`：在提供的 container 里渲染一个 React 元素，并返回对该组件的引用（或者针对无状态组件返回 null）
  - `hydrate(element, container, callback?)`：与`render()`相同，但它用于在 ReactDOMServer 渲染的容器中对 HTML 的内容进行 hydrate 操作
  - `unmountComponentAtNode(container)`：从 DOM 中卸载组件，会将其事件处理器和 state 一并清除
  - `findDOMNode(component)`：访问底层 DOM 节点，大多数情况下可以绑定一个 ref 到 DOM 节点上，可以完全避免使用，该方法不能用于函数组件
  - `createPortal(child, container)`：提供一种将子节点渲染到 DOM 节点中的方式，该节点存在于 DOM 组件的层次结构之外
- ReactDOMServer
  - `renderToString(element)`：将 React 元素渲染为初始 HTML，React 将返回一个 HTML 字符串，如果在已有服务端渲染标记的节点上调用`ReactDOM.hydrate()`方法，React 将会保留该节点且只进行事件处理绑定，从而让用户有一个非常高性能的首次加载体验
  - `renderToStaticMarkup(element)`：与 renderToString 相似，但此方法不会在 React 内部创建额外 DOM 属性
  - `renderToNodeStream(element)`：将一个 React 元素渲染成其初始 HTML，返回一个可输出 HTML 字符串的可读流，如果在已有服务端渲染标记的节点上调用`ReactDOM.hydrate()`方法，React 将会保留该节点且只进行事件处理绑定，从而让用户有一个非常高性能的首次加载体验
  - `renderToStaticNodeStream(element)`：与 renderToNodeStream 相似，但此方法不会在 React 内部创建额外 DOM 属性
