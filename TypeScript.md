- [TypeScript](#typescript)
  - [基础类型](#基础类型)
  - [接口](#接口)
  - [类](#类)
  - [函数](#函数)
  - [泛型](#泛型)
  - [枚举](#枚举)
  - [高级类型](#高级类型)

# TypeScript

## 基础类型

- `[let|var|const] variable: [symbol|boolean|number|string|object|undefined|null|any|void] = [initValue];`
  - `null`和`undefined`可赋值给任意类型，除非指定`--strictNullChecks`
  - 返回`never`的函数必须存在无法达到的终点
- `[let|var|const] variable: [Array<dtype>|dtype[]] = [initValue];`
- `<dtype>variable | variable as dtype`
  - 类型断言中，`as`语法断言是被允许的

## 接口

- `interface iname { [readonly] prop[?]: dtype; }`
  - 编译器只会检查那些必需的属性是否存在，并且其类型是否匹配
  - 可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误
  - `ReadonlyArray<T>`与`Array<T>`相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改
- `interface iname { [readonly] [prop: number|string]: dtype; }`
  - 索引签名的`number`类型索引值必须为`string`类型索引值的类型的同类或子类，因为`number`会发生类型转换
  - 若存在其他普通属性，其对应类型会受索引签名影响
  - 将索引签名设置为只读可以防止给索引赋值
- `interfalce iname { (arg: dtype[, ...]): dtype; }`
  - 函数接口
- `interface iname [extends iname[, ...]] { prop: dtype, method(arg: dtype); }`
  - 通过`class cname implements iname {...}`实现接口
  - 一个接口可以继承多个接口

## 类

```typescript
[abstract] class cname [extends cname] {
  [public|protected|private] [static] [readonly] prop: dtype[ = initValue];
  [public|protected] constructor(arg: dtype, ...) {...}
  [abstract] [public|protected|private] [static] [readonly] method(arg: dtype, ...) {...}
  get prop(): dtype {...}
  set prop(value): dtype {...}
  ...
}
```

- `constructor`内调用`this`前必须调用`super()`
- `protected`成员在派生类中仍然可以访问
- 构造函数也可以被标记成`protected`。 这意味着这个类不能在包含它的类外被实例化，但是能被继承
- 只读属性必须在声明时或构造函数里被初始化
- 存取器`get/set`可用于私有属性的访问控制
- 只带有`get`不带有`set`的存取器自动被推断为`readonly`
- `static`属性 通过`cname.prop`访问，且需要在声明时进行初始化
- 抽象类的抽象方法 需要在派生类中实现

## 函数

- `func[: (arg[?]: dtype[ = initValue], ...[, ...rest]) => dtype] = function(arg: dtype, ...): dtype {};`
  - 只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确
  - 如果在赋值语句的一边指定了类型但是另一边没有类型的话，编译器会自动识别出类型
  - 可选参数必须跟在必须参数后面
  - 当用户没有传递这个参数或传递的值是`undefined`时，可以为参数提供一个默认值
  - 带默认值的参数不需要放在必须参数的后面，如果带默认值的参数出现在必须参数前面，必须明确传入`undefined`来获得默认值

```typescript
/* 重载 */
function func(arg1: dtype1, ...): dtype1;
function func(arg2: dtype2, ...): dtype2;
function func(arg): any {...} // 不是重载列表的一部分
```

## 泛型

```typescript
/* 泛型函数 */
function func<T>(arg: [T|T[]|Array<T>|...]): dtype {...};
func<T>(arg);
func(arg); //自动推断类型
f: <U>(arg: dtype) => dtype = func;
/* 泛型类 */
class cname<T> {...}
new cname<T>(...);
/* 泛型约束 */
function func<T extends iname|cname>(arg: dtype, ...): dtype {...}
```

- 可以创建泛型接口和泛型类，但无法创建泛型枚举和泛型命名空间
- 类的静态属性不能使用泛型类型
- 定义一个接口来描述约束条件，可以保证正确访问参数的属性

## 枚举

- `[const] enum ename {tag, ...}`
  - 枚举值默认从`0`开始递增，可以自定义初始化器，但不带初始化器的要在它们之前先被定义
  - 枚举值具有不同数据类型时，称为异构枚举
  - 常量枚举只能使用常量枚举表达式，它们在编译阶段会被删除，常量枚举成员在使用的地方会被内联进来
- `ename[ename.tag] == name_of_tag`
  - 反向映射，从枚举值到枚举名字，但不会为字符串枚举成员生成反向映射

## 高级类型

- `<T & U>variable`
  - 交叉类型的对象同时拥有了这三种类型的成员
- `variable: T|U`
  - 如果一个值是联合类型，只能访问此联合类型的所有类型里共有的成员
  - 通过类型断言`<T>variable.prop`访问不共有的成员
  - 通过定义一个返回类型谓词的类型保护函数`function func(arg: T | U, ...) arg is T {...}`可以确定类型
  - 通过`typeof variable === ["number"|"string"|"boolean"|"symbol"]`也可以被识别为类型保护
  - 通过`variable instanceof T`也可以被识别为类型保护
- `variable!.prop`
  - 添加`!`后缀从类型里可以排除`null`和`undefined`
  - 通过`variable == null`可以识别`null`
  - 可选参数会自动加上`|undefined`
- `type tname[<T>] = dtype`
  - 类型别名可以作用于任何类型
  - 类型别名可以使用泛型和交叉类型
  - 可以在被赋值对象的属性值中引用类型别名本身，但不能直接作为泛型
  - 通过`type tname = str1/num1 | str2/num2 | ...`可以指定字符串或数字给字面量类型
- `method(arg, ...): this {...}`
  - 通过`this`类型能够在继承的同时保持接口的连贯性，新的类可以直接使用之前的方法
- `keyof T`
  - 使用索引类型，编译器就能够检查使用了动态属性名的代码
  - 对于任何类型`T`，`keyof T`的结果为`T`上已知的公共属性名的联合
  - 通过`K entends keyof T`可以保证正确使用`T[K]`
  - 通过`[K in keyof T]`可以在类型定义中去定义每个属性
- 预定义的有条件类型（TypeScript 2.8 | lib.d.ts）
  - `Exclude<T, U>` -- 从`T`中剔除可以赋值给`U`的类型（`T-U`）
  - `Extract<T, U>` -- 提取`T`中可以赋值给`U`的类型（`T&U`）
  - `NonNullable<T>` -- 从`T`中剔除`null`和`undefined`
  - `ReturnType<T>` -- 获取函数返回值类型
  - `InstanceType<T>` -- 获取构造函数类型的实例类型
