[TOC]

# 从C到C++

## 数据类型

+ 整型：`(unsigned) char/short/int/long/long long, bool`
+ 浮点型：`float/double`
+ 字符串：`string`
+ 自定义类型：`struct/class/union/enum`

## 命名空间

```c++
namespace [命名空间名] {
    // 除了 main()，其他都能定义
}
// 若存在冲突，需要 [命名空间名]::[变量/函数/类] 进行值的访问，其中 '::' 称为域解析符
// 使用 using 指定命名空间，指定后，命名空间所有的量都不用再加上'::'
using [命名空间名];
// 使用 using 指定命名空间中的变量、函数和类等，其中的'::'称为域解析操作符，指定后的量在使用时不用再加上'::'
using [命名空间名]::[变量/函数/类];
```

## 运算符

+ `new, delete`

  | 运算符            | 作用                                                         |
  | ----------------- | ------------------------------------------------------------ |
  | `new/new[]`       | 在堆内存动态分配内存以创建对象或对象数组，若创建对象数组则只能默认调用无参构造函数 |
  | `delete/delete[]` | 释放由 `new/new[]` 分配的空间                                |

+ `typeid`

  + `typeid(type/expression)` 返回 `type_info` 对象的引用，`type_info` 类位于`typeinfo`头文件
  + 一个类型不管使用了多少次，编译器都只为它创建一个对象，所有`typeid`都返回这个对象的引用
  + `name()`返回一个能表示类型名称的字符串。但是C++标准并没有规定这个字符串是什么形式的
  + `before()`判断一个类型是否位于另一个类型的前面，但是C++标准并没有规定类型的排列顺序
  + `raw_name()`用来返回名字编码（Name Mangling）算法产生的新名称
  + `hash_code()`用来返回当前类型对应的`hash`值，`hash`值是一个可以用来标志当前类型的整数
  + `operator==()/operator!=()`判断两个类型是否相同

  ```c++
  class type_info {
  public:
      virtual ~type_info();
      int operator==(const type_info& rhs) const;
      int operator!=(const type_info& rhs) const;
      int before(const type_info& rhs) const;
      const char* name() const;
      const char* raw_name() const;
  private:
      void *_m_data;
      char _m_d_name[1];
      type_info(const type_info& rhs);
      type_info& operator=(const type_info& rhs);
  };
  ```

+ `static_cast, dynamic_cast, const_cast, reinterpret_cast`

  + 格式：`xxx_cast<[新类型]>([表达式])`

  | 关键字             | 说明                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | `static_cast`      | 用于在编译期间进行良性转换，一般不会导致意外发生，风险很低；如原有的自动类型转换、`void`指针和具体类型指针之间的转换、转换构造函数或者类型转换函数的类与其它类型之间的转换；但不能进行两个具体类型指针之间的转换与`int`和指针之间的转换 |
  | `const_cast`       | 用于`const`与非`const`、`volatile`与非`volatile`之间的转换；通常`const`值在编译期间就会被替换为常量，使用`const_cast`可能会出现意外情况 |
  | `reinterpret_cast` | 高度危险的转换，这种转换仅仅是对二进制位的重新解释，不会借助已有的转换规则对数据进行调整，但是可以实现最灵活的 C++ 类型转换；如两个具体类型指针之间的转换与`int`和指针之间的转换 |
  | `dynamic_cast`     | 借助 RTTI，用于类型安全的向下转型（Downcasting）；向上转型是无条件的，不会进行任何检测，所以都能成功；向下转型的前提必须是安全的，要借助 RTTI 进行检测，所有只有一部分能成功；每个类都会在内存中保存一份类型信息，编译器会将存在继承关系的类的类型信息使用指针“连接”起来，从而形成一个继承链；总起来说，`dynamic_cast`会在程序运行过程中遍历继承链，如果途中遇到了要转换的目标类型，那么就能够转换成功，如果直到继承链的顶点（最顶层的基类）还没有遇到要转换的目标类型，那么就转换失败 |

  

## 函数

+ 内联函数

  格式：`inline type func(params) { //TODO }`

  说明：

  + 必须在定义函数时候加上`inline`关键字，声明处加没用
  + 不能滥用内联函数，因为内联展开会导致程序的代码体积变大
  + 实际上，编译器会自己判断是否需要进行内联展开，程序员的内联申请不一定被编译器同意执行
  + 类内部定义的成员函数将会自动默认为内联函数

+ 含默认参数的函数

  说明：C++的函数允许使用默认参数，但是所有默认参数必须在非默认参数之后

+ 重载函数

  说明：只要函数命名相同，且存在函数参数的种类或数量上的不同或返回值不同，就会构成函数的重载，重载实际上是编译器内部进行函数的重命名

## 引用

格式：`type &var = obj`

说明：

+ 引用本质上是指针的简化操作，需要利用已存在的对象对引用进行初始化，且在定义时就必须进行初始化
+ 不能用创建临时对象再赋值的方式初始化
+ 当函数返回引用时，函数调用后能够作为左值
+ 在传入标准数据类型时使用引用，能够直接对传入的参数进行修改，同时也比传入指针更加直观
+ 在传入对象时使用引用，能够提高程序的运行效率，因为参数初始化不需要调用复制拷贝函数
+ 特别地，C++还有右值引用`type &&var = obj`，能够对非左值进行引用

## 匿名函数

格式：`[外部变量访问方式说明符[参数列表]] ([参数列表]) -> [返回值类型] {}`

说明：

+ `[外部变量访问方式说明符]`可以是`=`或`&`，表示`{}`中用到的、定义在`{}`外面的变量在`{}`中是否允许被改变。`=`表示不允许，`&`表示允许
+ 在`{}`中也可以不使用定义在外面的变量。`-> [返回值类型]`可以省略
+ `[外部变量访问方式说明符[参数列表]]`中的`[参数列表]`不需要带数据类型，列表中出现的变量应当是外部变量，而非参数列表中出现过的参数，它们的访问方式与主访问方式相反

## `auto`和`decltype`关键字

| 关键字     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| `auto`     | 用`auto`关键字定义变量，编译器会自动判断变量的类型，使用格式为`auto var = value` |
| `decltype` | 用于求表达式的类型，在 C++11 中，函数返回值若为`auto`，则需要和`decltype`配合使用。在 C++14 中，则可以不用`decltype`，使用格式为`decltype(exp)` |

# 类与对象

## 结构

+ `struct` 被继承时，若继承方式缺省，默认为 `public` 继承，若限定符缺省，默认为 `public` 限定；不能使用模板
+ `class` 被继承时，若继承方式缺省，默认为 `private` 继承，若限定符缺省，默认为 `private` 限定
+ 限定符只作用到结尾或下一个限定关键字的出现
+ 类内部的静态变量需要在类外初始化，类外不再需要 `static` 声明
+ 成员函数若在类外定义，使用以下的代码形式进行定义
+ 若成员函数声明为`const`，由于不存在实例，也就没有`this`指针，故不可以再声明为`static`
+ `static`成员可以由类和实例访问，但共用一块内存
+ `const`成员不可修改，`const`对象也是
+ 友元函数和友元类在声明之后，能够访问当前类的所有成员，访问权限是单向不可传递的
+ 在以某种方式继承基类后，派生类对基类的任意成员的访问权限最高不超过继承权限
+ 通过对象访问成员时，使用`.`访问，通过对象指针访问成员时，使用`->`访问
+ 若继承的成员变量重名，可以通过`::`进行限定；继承的成员函数可以重载

```c++
struct/class [类名][: [virtual] [继承方式] [类名],...],... {
[限定符]:
    // 成员变量/成员函数的声明或定义、友元函数声明等
};
type [类名]::[变量] = [值];
[返回值类型] [类名]::[函数]([参数列表]) [const] {
    // TODO
}
```

## 构造函数与析构函数

### 普通构造函数

格式：`[类名]([参数列表]): [基类名](参数列表),...[常量](参数值),...[成员变量](参数值),...{}`

说明：

+ 基本上使用 `public/protected` 限定构造函数，若用 `private` 则外部无法访问，只能利用基类的 `static` 成员函数进行构造函数的调用以返回基类对象
+ 基类的构造函数只能由构造函数调用，即使有默认参数也不能省略，若基类存在无参构造，系统会自动调用基类的无参构造函数
+ 类内部的 `const` 常量只能在初始化列表进行初始化
+ 初始化成员变量的顺序只由定义时的顺序决定
+ 多个基类的构造函数只由类定义时的顺序决定
+ 若存在虚继承的类，需要由该类调用祖基类的构造函数，在派生类中只保留一份间接基类（虚基类）的成员，虚继承的几个类需要`virtual`继承虚基类
+ 构造函数不能有返回值，但可以使用`return ;`

### 拷贝构造函数

格式：`[类名](const [类名] &Obj) {}`

说明：

+ 在未显式定义拷贝构造函数的时候，系统会生成一个默认拷贝构造函数，仅进行成员变量的浅拷贝
+ 拷贝构造函数与赋值运算符的重载不同，在初始化时使用时，调用的是拷贝构造函数，而非赋值运算符的重载

### 转换构造函数

格式：`[类名]([单参数]): [基类名](参数列表),...[常量](参数值),...[成员变量](参数值),...{}`

说明：

+ 转换构造函数只有一个参数，利用其它类型进行当前类的实例化需要借助转换构造函数
+ 转换构造函数可以重载多个
+ 以`[类名] [变量] = [单参数]`形式初始化时，编译器先利用参数创建匿名对象，再调用拷贝构造函数创建对象
+ 以``[类名] [变量]([单参数])`形式初始化时，直接创建对象，不经过拷贝构造函数
+ 为了获得目标类型，编译器会“不择手段”，综合使用内置的和用户自定义的转换规则，并且会进行多级类型转换

### 类型构造函数

格式：`operator type() {}`

说明：

+ 类型转换函数没有参数，也不需要写返回值
+ 类型转换函数不更改当前对象，可以声明为`const`
+ 一个类虽然可以有多个类型转换函数（类似于函数重载），但是如果多个类型转换函数要转换的目标类型本身又可以相互转换（类型相近），那么有时候就会产生二义性错误
+ 即使没有重载某些运算符号，编译器也会尽可能使用类型构造函数进行符号运算

##

### 析构函数

+ 析构函数在对象的生存周期结束后，由系统自动调用，派生类最先调用基类构造函数，但最后调用基类析构函数
+ 析构函数不能重载，也没有返回值，但可以使用`return ;`
+ 析构函数常声明为`virtual`，否则，如果通过基类指针`delete`派生类实例，系统不会自动调用派生类的析构函数

```c++
struct/class [类名][: [virtual][继承方式] [类名],...],... {
[限定符]:
    ~[类名]() {
        // TODO
    }
    // TODO
};
```

## 多态

说明：

+ 多态可以分为编译时的多态和运行时的多态。前者主要是指函数的重载（包括运算符的重载）、对重载函数的调用，在编译时就能根据实参确定应该调用哪个函数，因此叫编译时的多态；而后者则和继承、虚函数等概念有关
+ 借助多态，基类指针和基类引用也能全方位访问派生类中的基类成员
+ 实现多态的基类成员函数需要声明为`virtual`，派生类继承的虚函数默认也为`virtual`，因此最好在继承时也声明

## 抽象类

格式：`virtual [返回值类型] [函数]([参数列表]) = 0; // 纯虚函数，只能通过虚函数进行定义`

说明：包含纯虚函数的类称为抽象类，抽象类不可实例化，常作为基类约束派生类功能，实现多态，属于设计范畴

## 函数对象

说明：如果一个类将`()`运算符重载为成员函数，这个类就称为函数对象类，这个类的对象就是函数对象。函数对象是一个对象，但是使用的形式看起来像函数调用，实际上也执行了函数调用

```c++
template <typename T, ...>
class [类名] {
public:
    [返回值] [函数]([参数列表]) {}
};
```

# 运算符重载

+ 运算符重载由成员函数形式和友元函数形式，需要全局使用则常定义为全局友元函数形式
+ 运算符重载为成员函数时，函数的参数个数就是运算符的操作数个数减一，运算符的操作数有一个成为函数作用的对象，其余的成为函数的实参
+ 自增、自减运算符各有两种重载方式，用于区别前置用法和后置用法，含形式参数即为后置形式，常返回引用
+ 一元运算符常定义为成员函数，`[], (), ->, =`必须定义为成员函数
+ `()`常用于对象数组元素的初始化或定义函数对象，`=`用于定义赋值运算符
+ `operator type()`用于定义类型转换函数，不需要写返回值
+ `new/new[]/delete/delete[]`也能重载，用于自定义内存释放操作
+ `<<, >>`常用于输入输出流，且返回流引用，使之能够成为左值，从而进行连续的输入输出
+ 以下运算符不能被重载：`., .*, ::, ?:, sizeof`
+ C++ 规定，运算符重载不改变运算符的优先级

# 模板

+ 函数模板

  格式：`template <typename/class T,...> [返回值类型] [函数]([参数列表]);`

  说明：参数不可省略，否则编译器无法匹配生成新的函数

+ 类模板

  格式：`template <typename/class T,...> class [类名]... {...}`

  说明：

  + 与函数模板不同，类模板在实例化时（定义处和构造处）必须显式地指明数据类型`[类名]<type,...>`
  + 类模板外定义成员函数都需要加上模板头`template <typename/class T,...>`

# 异常处理

+ 结构：

  ```c++
  try {
      // TODO
  } catch(type &err) {
      // TODO
  } ... {
  } catch(type &err) {
      // TODO
  }
  ```

+ 说明：
  + catch 块可以连续多个，按顺序捕获不同数据类型（完全匹配）的异常，一旦捕获便不再继续匹配或向上抛出
  + 若需要捕获任意异常，使用`catch(...)`
  + 若无 catch 块能够捕获该异常，将向上抛出给上一级处理
  + 可以在函数参数后指定异常规范，如`func(params) throw(type,...)`，若空白则默认捕获所有异常，由于不能顾及所有异常，异常规范不建议使用
  + 可以在类内部实现异常类进行抛出
  + 派生类的成员函数的异常规范不能比基类成员函数宽松

+ `exception`类

  `exception`类的直接派生类

  | 异常名称            | 说明                                                         |
  | ------------------- | ------------------------------------------------------------ |
  | `logic_error`       | 逻辑错误                                                     |
  | `runtime_error`     | 运行时错误                                                   |
  | `bad_alloc`         | 使用`new`或`new[]`分配内存失败时抛出的异常                   |
  | `bad_typeid`        | 使用`typeid`操作一个`NULL`指针，而且该指针是带有虚函数的类   |
  | `bad_cast`          | 使用`dynamic_cast`转换失败时抛出的异常                       |
  | `ios_base::failure` | io 过程中出现的异常                                          |
  | `bad_exception`     | 这是个特殊的异常，如果函数的异常列表里声明了`bad_exception`异常，当函数内部抛出了异常列表中没有的异常时，如果调用的`unexpected()`函数中抛出了异常，不论什么类型，都会被替换为`bad_exception`类型 |

# STL(Standard Template Library)

## 顺序容器

+ 说明：元素在容器中的位置同元素的值无关，即容器不是排序的

+ 迭代器：

  + 说明：能够进行`++, --, += -=, <, <=, >, >=, ==, !=, -`等操作

  + 迭代器类型

    | 类型                                  | 说明                                     |
    | ------------------------------------- | ---------------------------------------- |
    | `[container]::iterator`               | 正向迭代器，++操作顺序遍历               |
    | `[container]::const_iterator`         | 常量正向迭代器，++操作顺序遍历，不可更改 |
    | `[container]::reverse_iterator`       | 反向迭代器，++操作逆序遍历               |
    | `[container]::const_reverse_iterator` | 常量反向迭代器，++操作逆序遍历，不可更改 |

  + 迭代器的辅助函数(包含在`<algorithm>`头文件)

    | 辅助函数                           | 说明                                                         |
    | ---------------------------------- | ------------------------------------------------------------ |
    | `advance(iterator p, int n)`       | 使迭代器 p 向前或向后移动 n 个元素                           |
    | `distance(iterator p, iterator q)` | 计算两个迭代器之间的距离，即迭代器 p 经过多少次 ++ 操作后和迭代器 q 相等。如果调用时 p 已经指向 q 的后面，则这个函数会陷入死循环 |
    | `iter_swap(p, q)`                  | 用于交换两个迭代器 p、q 指向的值                             |

+ 容器

  1. `vector`

     | 成员函数                                                     | 作用                                                         |
     | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | `vector()`                                                   | 无参构造函数，将容器初始化为空                               |
     | `vector(int n)`                                              | 将容器初始化为有 n 个元素                                    |
     | `vector(int n, const T &val)`                                | 假定元素的类型是 T，此构造函数将容器初始化为有 n 个元素，每 个元素的值都是 val |
     | `vector(iterator first, iterator last)`                      | first 和 last 可以是其他容器的迭代器。一般来说，本构造函数初始化的结果就是将 vector 容器的内容变成与其他容器上的区间 [first, last) —致 |
     | `void clear()`                                               | 删除所有元素                                                 |
     | `bool empty()`                                               | 判断容器是否为空                                             |
     | `void pop_back()`                                            | 删除容器末尾的元素                                           |
     | `void push_back(const T & val)`                              | 将 val 添加到容器末尾                                        |
     | `int size()`                                                 | 返回容器中元素的个数                                         |
     | `T& front()`                                                 | 返回容器中第一个元素的引用                                   |
     | `T& back()`                                                  | 返回容器中最后一个元素的引用                                 |
     | `iterator insert(iterator i, const T &val)`                  | 将 val 插入迭代器 i 指向的位置，返回 i                       |
     | `iterator insert(iterator i, iterator first, iterator last)` | 将其他容器上的区间 [first, last) 中的元素插入迭代器 i 指向的位置 |
     | `iterator erase(iterator i)`                                 | 删除迭代器 i 指向的元素，返回值是被删元素后面的元素的迭代器  |
     | `iterator erase(iterator first, iterator last)`              | 删除容器中的区间 [first, last)                               |
     | `void swap(vector & v)`                                      | 将容器自身的内容和另一个同类型的容器 v 互换                  |
     | `T& operator[](int n)`                                       | 返回下标为 n 的元素的引用                                    |

  2. `deque`

     类似`vector`，但多了`void push_front(const T &val)`和`void pop_front()`

  3. `list`

     未列出的还有`begin(), rbegin(), end(), rend(), clear(), empty(), erase(), insert(), front(), back(), push_back(), pop_back()`

     | 成员函数                                                     | 作用                                                         |
     | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | `void push_front(const T &val)`                              | 将 val 插入链表最前面                                        |
     | `void pop_front()`                                           | 删除链表最前面的元素                                         |
     | `void sort()`                                                | 将链表从小到大排序，因 list 不能随机存取，故内置了 sort 算法 |
     | `void remove (const T &val)`                                 | 删除和 val 相等的元素                                        |
     | `void remove_if(pred)`                                       | 删除符合某种条件的元素，参数为函数或函数对象                 |
     | `void unique()`                                              | 删除所有和前一个元素相等的元素                               |
     | `void merge(list<T> &x)`                                     | 将链表 x 合并进来并清空 x。要求链表自身和 x 都是有序的       |
     | `void splice(iterator i, list<T> &x, iterator first, iterator last)` | 在位置 i 前面插入链表 x 中的区间 [first, last)，并在链表 x 中删除该区间。链表自身和链表 x 可以是同一个链表，只要 i 不在 [first, last) 中即可 |

## 关联容器

说明：关联容器内的元素是排序的。插入元素时，容器会按一定的排序规则将元素放到适当的位置上，因此插入元素时不能指定位置，关联容器一般是用平衡二叉树实现

1. `set`

   定义：

   ```c++
   template < class Key, class Pred = less<Key>, class A = allocator<Key> >
   class set {};
   ```

   函数：

   未列出的常用函数还有`begin()/rbegin()/end()/rend()/empty()/clear()`等

   | 成员函数                                             | 作用                                                         |
   | ---------------------------------------------------- | ------------------------------------------------------------ |
   | `pair<iterator, bool> insert(const T &val)`          | 返回值是 pair 模板类对象 x，如果 x.second 为 true，则说明插入成功，此时 x.first 就是指向被插入元素的迭代器；如果 x.second 为 false，则说明要插入的元素已在容器中，此时 x.first 就是指向原有那个元素的迭代器 |
   | `pair<iterator, iterator> equal_range(const T &val)` | 返回值对象中的 first 就是 lower_bound 的值，second 就是 upper_bound 的值 |
   | `size_t count(const T &val) const`                   | 计算等于某个值的元素个数                                     |
   | `iterator find(const T &val) const`                  | 查找某个值，未找到则返回`end()`                              |
   | `iterator lower_bound(const T &val) const`           | 查找某个下界，使得插入后不影响有序性                         |
   | `iterator upper_bound(const T &val) const`           | 查找某个上界，使得插入后不影响有序性                         |

2. `multiset`

   定义：

   ```c++
   template <class Key, class Pred = less<Key>, class B = allocator<Key> >
   class multiset {};
   ```

   函数：

   未列出的常用函数还有`begin()/rbegin()/end()/rend()/empty()/clear()`等

   | 成员函数                                             | 作用                                                         |
   | ---------------------------------------------------- | ------------------------------------------------------------ |
   | `iterator insert(const T &val)`                      | 将 val 插入容器中并返回其迭代器                              |
   | `void insert(iterator first, iterator last)`         | 将区间 [first, last) 中的元素插人容器                        |
   | `pair<iterator, iterator> equal_range(const T &val)` | 返回值对象中的 first 就是 lower_bound 的值，second 就是 upper_bound 的值 |
   | `size_t count(const T &val) const`                   | 计算等于某个值的元素个数                                     |
   | `iterator find(const T &val) const`                  | 查找某个值，未找到则返回`end()`                              |
   | `iterator lower_bound(const T &val) const`           | 查找某个下界，使得插入后不影响有序性                         |
   | `iterator upper_bound(const T &val) const`           | 查找某个上界，使得插入后不影响有序性                         |
   | `iterator erase(iterator it)`                        | 删除 it 指向的元素，返回其后面的元素的迭代器                 |
   | `iterator erase(iterator first, iterator last)`      | 删除区间 [first, last)，返回 last                            |

3. `map`

   定义：

   ```c++
   template < class Key, class T, class Pred = less<Key>, class A = allocator<T> >
   class map {
       ...
       typedef pair< const Key, T > value_type;
       ...
   };
   ```

   说明：map 的每个元素都分为关键字和值两部分，容器中的元素是按关键字排序的，并且不允许有多个元素的关键字相同

   函数：

   未列出的常用函数还有`begin()/rbegin()/end()/rend()/empty()/clear()`等

   | 成员函数                                        | 作用                                                         |
   | ----------------------------------------------- | ------------------------------------------------------------ |
   | `iterator insert (const pair<Key, T> &p)`       | 将 pair 对象 p 插入容器中并返回其迭代器                      |
   | `void insert(iterator first, iterator last)`    | 将区间 [first, last) 中的元素插人容器                        |
   | `pair equal_range(const Key &val)`              | 返回值对象中的 first 就是 lower_bound 的值，second 就是 upper_bound 的值 |
   | `iterator find(const T &val) const`             | 查找某个值，未找到则返回`end()`                              |
   | `iterator lower_bound(const Key &val) const`    | 查找某个下界，使得插入后不影响有序性                         |
   | `iterator upper_bound(const Key &val) const`    | 查找某个上界，使得插入后不影响有序性                         |
   | `iterator erase(iterator it)`                   | 删除 it 指向的元素，返回其后面的元素的迭代器                 |
   | `iterator erase(iterator first, iterator last)` | 删除区间 [first, last)，返回 last                            |
   | `T& operator[](Key k)`                          | 返回 first 值为 k 的元素的 second 部分的引用，如果容器中没有元素的 first 值等于 k，则自动添加一个 first 值为 k 的元素。如果该元素的 second 成员变量是一个对象，则用无参构造函数对其初始化 |

4. `multimap`

   定义：

   ```c++
   template < class Key, class T, class Pred = less<Key>, class A = allocator<T> >
   class multimap {
       ...
       typedef pair <const Key, T> value_type;
       ...
   };
   ```

   函数：

   未列出的常用函数还有`begin()/rbegin()/end()/rend()/empty()/clear()`等

   | 成员函数                                        | 作用                                                         |
   | ----------------------------------------------- | ------------------------------------------------------------ |
   | `iterator insert (const pair &p)`               | 将 pair 对象 p 插入容器中并返回其迭代器                      |
   | `void insert(iterator first, iterator last)`    | 将区间 [first, last) 中的元素插人容器                        |
   | `pair equal_range(const Key &val)`              | 返回值对象中的 first 就是 lower_bound 的值，second 就是 upper_bound 的值 |
   | `size_t count(const T &val) const`              | 统计有多少个元素的关键字和 val 相等                          |
   | `iterator find(const T &val) const`             | 查找某个值，未找到则返回`end()`                              |
   | `iterator lower_bound(const Key &val) const`    | 查找某个下界，使得插入后不影响有序性                         |
   | `iterator upper_bound(const Key &val) const`    | 查找某个上界，使得插入后不影响有序性                         |
   | `iterator erase(iterator it)`                   | 删除 it 指向的元素，返回其后面的元素的迭代器                 |
   | `iterator erase(iterator first, iterator last)` | 删除区间 [first, last)，返回 last                            |

## 容器适配器

说明：在顺序容器的基础上实现的，屏蔽了顺序容器的一部分功能，突出或增加了另外一些功能

1. `stack`

   定义：

   ```c++
   template < class T, class Cont = deque <T> >
   class stack {};
   ```

   说明：

   + 栈是一种后进先出的元素序列，访问和删除都只能对栈顶的元素（即最后一个被加入栈的元素）进行，并且元素也只能被添加到栈顶
   + 栈内的元素不能访问。如果一定要访问栈内的元素，只能将其上方的元素全部从栈中删除，使之变成栈顶元素才可以
   + 在默认情况下用 deque 实现，也可以指定用 vector 或 list 实现

   函数

   | 成员函数                | 作用                                                         |
   | ----------------------- | ------------------------------------------------------------ |
   | `size_t size()`         | 返回栈内元素个数                                             |
   | `bool empty()`          | 返回栈是否为空                                               |
   | `void clear()`          | 清空栈内元素                                                 |
   | `void pop()`            | 弹出（即删除）栈顶元素                                       |
   | `void push(const T &x)` | 将 x 压入栈顶                                                |
   | `T& top()`              | 返回栈顶元素的引用；通过此函数可以读取栈顶元素的值，也可以修改栈顶元素 |

2. `queue`

   定义：

   ```c++
   template < class T, class Cont = deque<T> >
   class queue {};
   ```

   说明：

   + 队列是先进先出，队头的访问和删除操作只能在队头进行，添加操作只能在队尾进行。不能访问队列中间的元素
   + 在默认情况下用 deque 实现，也可以指定用 list 实现

   函数

   | 成员函数                | 作用                                                         |
   | ----------------------- | ------------------------------------------------------------ |
   | `size_t size()`         | 返回队列内元素个数                                           |
   | `bool empty()`          | 返回队列是否为空                                             |
   | `void clear()`          | 清空队列元素                                                 |
   | `void pop()`            | 弹出（即删除）队首元素                                       |
   | `void push(const T &x)` | 将 x 压入队尾                                                |
   | `T& front()`            | 返回队首元素的引用；通过此函数可以读取队首元素的值，也可以修改队首元素 |
   | `T& back()`             | 返回队尾元素的引用；通过此函数可以读取队尾元素的值，也可以修改队尾元素 |

3. `priority_queue`

   定义：

   ```c++
   template < class T, class Container = vector<T>, class Compare = less<T> >
   class priority_queue {};
   ```

   说明：

   + priority_queue 是优先队列，它和普通队列的区别在于，优先队列的队头元素总是最大的
   + 执行 pop 操作时，删除的总是最大的元素；执行 front 操作时，返回的是最大元素的引用
   + 使用“堆排序”技术实现的，其内部并非完全有序，但却能确保最大元素总在队头。因此，priority_queue 特别适用于“不停地在一堆元素中取走最大的元素”这种情况
   + priority_queue 插入和删除元素的复杂度都是 O(log(n))。虽然用 set/multiset 也能完成此项工作，但是优先队列比它们略快一些

## 模板类

1. `string`

   - [ ] | 函数                                    | 说明                                                         |
     | --------------------------------------- | ------------------------------------------------------------ |
     | `string()`                              | 无参构造                                                     |
     | `string(s)`                             | 利用`const char *` 或`string`进行构造                        |
     | `string(s, pos, n)`                     | 利用字符串的`[pos, pos + n)`子串进行构造                     |
     | `string(n, ch)`                         | 利用 n 个 ch 字符进行字符串初始化                            |
     | `operator=()/assign()`                  | 使用类似构造函数                                             |
     | `length()/size()`                       | 返回字符串的长度                                             |
     | `append()`                              | 使用类似构造函数，进行字符串的拼接                           |
     | `operator+=()`                          | 拼接后返回对象的引用                                         |
     | `operstor+()`                           | 使用类似构造函数，拼接后返回新对象                           |
     | `compare(pos, n, ...)`                  | 用于比较字符串，还可以用 `<, <=, ==, !=, >=, > `运算符比较，前三个参数用于对自身切片，可省略，后续参数类似构造函数的参数 |
     | `substr(m, n)`                          | 获取子串的函数，第二个参数可省略，表示从下标 m 切片到末尾    |
     | `swap(s)`                               | 交换两个`string`对象的内容                                   |
     | `find()/rfind()`                        | 从前往后 / 从后往前查找子串或字符出现的位置                  |
     | `find_first_of(s)/find_first_not_of(s)` | 从前往后查找何处出现另一个字符串中包含 / 未包含的字符        |
     | `find_last_of(s)/find_last_not_of(s)`   | 从后往前查找何处出现另一个字符串中包含 / 未包含的字符        |
     | `replace(pos, n, ...)`                  | 用于替换字符串，前三个参数用于对自身切片，不可省略，后续参数类似构造函数的参数 |
     | `erase(m, n)`                           | 删除子串的函数，第二个参数可省略，表示从下标 m 删除到末尾    |
     | `insert(pos, ...)`                      | 插入子串的函数，第一个参数表示插入下标后续参数类似构造函数的参数 |

2. `bitset`

   格式：`bitset<size_t n> var;`

   说明：

   + 由若干个位（bit）组成，它提供一些成员函数，不必通过位运算就能很方便地访问、修改其中的任意一位

   + 通过无符号整型作为`<>`中的参数

   + 重载了多个运算符，包括`&, |, ^, <<, >>, &=, |=, ^=, <<=, >>=, [], ~, ==, !=`

   + 其他成员函数

     | 成员函数                                      | 作用                           |
     | --------------------------------------------- | ------------------------------ |
     | `bitset<N>& set()`                            | 将所有位全部设成 1             |
     | `bitset<N>& set(size_t pos, bool val = true)` | 将第 pos 位设为 val            |
     | `bitset<N>& reset()`                          | 将所有位全部设成 0             |
     | `bitset<N>& reset(size_t pos)`                | 将第 pos 位设成 0              |
     | `bitset<N>& flip()`                           | 将所有位翻转（0变成1，1变成0） |
     | `bitset<N>& flip(size_t pos)`                 | 翻转第 pos 位                  |
     | `unsogned long to_ulong() const`              | 将对象中的0、1串转换成整数     |
     | `string to_string() const`                    | 将对象中的0、1串转换成字符串   |
     | `size_t count() const`                        | 计算 1 的个数                  |
     | `size_t size() const`                         | 返回总位数                     |
     | `bool test(size_t pos) const`                 | 测试第 pos 位是否为 1          |
     | `bool all() const`                            | 判断是否全部为 1               |
     | `bool any() const`                            | 判断是否有某位为 1             |
     | `bool none() const`                           | 判断是否全部为 0               |

3. `pair`

   ```c++
   // pair 模板类的定义
   template <class _Tl, class _T2>
   struct pair {
       _T1 first;
       _T2 second;
       pair(): first(), second() {}
       pair(const _T1 &__a, const _T2 &__b): first(__a), second(__b) {}
       template <class_U1, class_U2>
       pair(const pair <_U1, _U2> &__p): first(__p.first), second(__p.second) {}
   };
   // 返回 pair 对象
   template <class _Tl, class _T2>
   pair<_T1, _T2> make_pair(const _T1 &__a, const _T2 &__b);
```
   
4. 函数对象类模板

   | 函数对象类模板      | 功能             |
   | ------------------- | ---------------- |
   | `plus<T>`           | `return x + y;`  |
   | `minus<T>`          | `return x - y;`  |
   | `multiplies<T>`     | `return x * y;`  |
   | `divides<T>`        | `return x / y;`  |
   | `modulus<T>`        | `return x % y;`  |
   | `equal_to<T>`       | `return x == y;` |
   | `not_equal_to<T>`   | `return x != y;` |
   | `greater<T>`        | `return x > y;`  |
   | `less<T>`           | `return x < y;`  |
   | `greater_equal <T>` | `return x >= y;` |
   | `less_equal<T>`     | `return x <= y;` |
   | `logical_and<T>`    | `return x && y;` |
   | `logical_or<T>`     | `return x || y;` |
   | `negate<T>`         | `return -x;`     |
   | `logical_not<T>`    | `return !x;`     |

## 算法

说明：

+ STL 中的大部分常用算法都在头文件`<algorithm>`中定义，头文件`<numeric>`中也有一些算法
+ 在未排序的区间上进行的算法，通常使用`==`进行判断，而在排好序的区间上操作的算法，通常只使用`<`进行判断

函数

1. `iterator find(iterator p, iterator q, const T& val)`：在迭代器 first、last 指定的容器的一个区间 [first, last) 中，按顺序查找和 val 相等的元素。如果找到，就返回该元素的迭代器；如果找不到，就返回 last；find 模板使用`==`运算符判断元素是否相等。因此，如果 [first, last) 区间中存放的是对象，则`==`运算符应该被适当重载，使得两个对象可以用`==`运算符比较
2. `void sort(iterator first, iterator last)`：对区间 [first, last) 从小到大进行排序

# 头文件

## 流

+ `<iostream>`

  说明：继承于`istream`和`ostream`，是用于 IO 的头文件

  常量：

  | 常对象  | 说明                                                         |
  | ------- | ------------------------------------------------------------ |
  | `cin`   | 标准流对象，能够重定向，`cin >> var`返回值可直接当作`bool`类型判断是否读取结束，表示读取结束可用`Ctrl+Z+Enter` |
  | `cout`  | 标准流对象，能够重定向                                       |
  | `cerr`  | 不使用缓冲区，直接向显示器输出信息                           |
  | `clog`  | 先被存放到缓冲区，缓冲区满或者刷新时才输出到屏幕             |
  | `endl`  | 输出回车，刷新缓冲区                                         |
  | `ends`  | 输出空字符，刷新缓冲区                                       |
  | `flush` | 刷新缓冲区，不附加任何额外字符                               |

  函数：

  `freopen(const char * file, const char * mode, stream);`：将标准输入输出重定向为指定文件

  `putchar()`：与`<stdio.h>`中的函数相同，C++保留了该函数在`<iostream>`

  `cin.get()`：此函数从输入流中读入一个字符，返回值就是该字符的 ASCII 码。如果碰到输入的末尾，则返回值为 `EOF`，`EOF`是在`iostream`类中定义的一个整型常量，值为 -1；`get()`函数不会跳过空格、制表符、回车等特殊字符，所有的字符都能被读入；接受字符时应使用`int`类型变量，否则可能误判为`EOF`

  `cin.getline(char* buf, int bufSize, char delim='\n')`：从输入流中读取`bufSize-1`个字符到缓冲区`buf`，`\n`或`delim`都不会被读入`buf`，但会被从输入流中取走

  `cin.ignore(int n=1, int delim=EOF)`：跳过输入流中的 n 个字符，或跳过`delim`及其之前的所有字符，哪个条件先满足就按哪个执行

  `cin.peek()`：返回输入流中的下一个字符，但是并不将该字符从输入流中取走

  `cin.tie(ostream *tiestr)`：若不带参数，返回指向绑定的输出流的指针，如果对象未关联到流，则返回空指针；若带参数，将`tiestr`指向的输出流绑定到该对象上，并返回上一个绑定的输出流指针

  `cin`成员函数

  | 成员函数  | 说明                                                         |
  | --------- | ------------------------------------------------------------ |
  | `good()`  | 操作成功，没有发生任何错误                                   |
  | `eof()`   | 到达输入末尾或文件尾                                         |
  | `fail()`  | 发生某些意外情况（如读取的数据类型不匹配）                   |
  | `bad()`   | 发生严重的意外（如磁盘读故障）                               |
  | `clear()` | 当流发生错误时进行错误恢复。将流从 fail() 状态转移到其他状态，从而可以继续从中读取字符 |

  `cout.put(char c)`：用于输出单个字符，返回值是 cout 的引用，因此能够连续作为左值继续调用该函数

  `cout`成员函数

  | 成员函数       | 对应流操纵算子        | 说明                                                         |
  | -------------- | --------------------- | ------------------------------------------------------------ |
  | `precision(n)` | `setprecision(n)`     | 设置输出浮点数的精度为 n                                     |
  | `width(w)`     | `setw(w)`             | 指定输出宽度为 w 个字符                                      |
  | `fill(c)`      | `setfill (c)`         | 在指定输出宽度情况下，输出宽度不足时用字符 c 填充（默认空格） |
  | `setf(flag)`   | `setiosflags(flag)`   | 将某个输出格式标志置为 1                                     |
  | `unsetf(flag)` | `resetiosflags(flag)` | 将某个输出格式标志置为 0                                     |

+ `<iomanip>`

  说明：用于控制 IO 格式化的头文件

  | 流操纵算子                              | 作用                                                         |
  | --------------------------------------- | ------------------------------------------------------------ |
  | `*dec/hex/oct`                          | 以十进制/十六进制/八进制形式输出整数                         |
  | `fixed/scientific`                      | 以普通小数形式/科学计数法形式输出浮点数                      |
  | `left/*right`                           | 左对齐/右对齐，即在宽度不足时将填充字符添加到右边/左边       |
  | `setbase(b)`                            | 设置输出整数时的进制，取值为 8、10 或 16                     |
  | `setw(w)`                               | 指定输出宽度为 w 个字符，或输人字符串时读入 w 个字符，仅作用一次 |
  | `setfill(c)`                            | 在指定输出宽度情况下，输出的宽度不足用字符 c 填充（默认空格填充） |
  | `setprecision(n)`                       | 在使用非`fixed`且非`scientific`方式输出的情况下，`n`即为有效数字最多的位数，如果有效数字位数超过`n`，则小数部分四舍五人，或自动变为科学计数法输出并保留一共`n`位有效数字 |
  | `setiosflags(flag)/resetiosflags(flag)` | 将某个输出格式标志置为 1 / 0                                 |
  | `boolalpha/*noboolalpha`                | 把 true 和 false 输出为字符串 / 0和1                         |
  | `showbase/*noshowbase`                  | 输出/不输出表示数值的进制的前缀                              |
  | `showpoint/*noshowpoint`                | 总是输出小数点/只有当小数部分存在时才显示小数点              |
  | `showpos/*noshowpos`                    | 在非负数值中显示/不显示 +                                    |
  | `*skipws/noskipws`                      | 输入时跳过/不跳过空白字符                                    |
  | `uppercase/*nouppercase`                | 十六进制数中使用`A~E/a-e`。若输出前缀，则前缀输出`0X/0x`，科学计数法中输出`E/e` |
  | `internal`                              | 数值的符号（正负号）在指定宽度内左对齐，数值右对 齐，中间由填充字符填充 |

  `setiosflags()`算子：是一个库函数，它以一些标志作为参数，这些标志实际上都是仅有某比特位为 1，而其他比特位都为 0 的整数；多个标志可以用`|`运算符连接，表示同时设置

  | 标志                       | 作用                                                         |
  | -------------------------- | ------------------------------------------------------------ |
  | `ios::left/right/internal` | 输出数据在本域宽范围内向左对齐 / 向右对齐 / 数值的符号位在域宽内左对齐，数值右对齐，中间由填充字符填充 |
  | `ios::dec/oct/hex`         | 设置整数的基数为 10 / 8 / 16                                 |
  | `ios::showbase`            | 强制输出整数的基数（八进制数以 0 开头，十六进制数以 0x 打头） |
  | `ios::showpoint`           | 强制输出浮点数的小数点和尾数 0                               |
  | `ios::uppercase`           | 在以科学记数法格式 E 和以十六进制输出字母时以大写表示        |
  | `ios::showpos`             | 对正数显示“+”号                                              |
  | `ios::fixed/scientific`    | 浮点数以定点格式（小数形式）输出 / 以科学记数法格式输出      |
  | `ios::unitbuf`             | 每次输出之后刷新所有的流                                     |
  | `ios::stdio`               | 每次输出之后清除`stdout, stderr`                             |

+ `<istream>/<ostream>`

  说明：继承于`ios`，用于输入输出的流类，`cin/cout`分别是它们的对象

+ ``ifstream/ofstream`

  说明：分别继承于`istream, ostream`，是用于从文件读取/写入数据的类

+ `fstream`

  说明：继承于`iostream`，是既能从文件读取数据，又能向文件写入数据的类，文件流对象也有`get()/put()`成员函数，作用类似`<iostream>`中的对应成员函数
  
  成员函数：
  
  `open(const char *filename, int mode)`：同时属于`ifstream/ofstream/fstream`类的成员函数，文件打开模式标记如下表所示，可以通过`|`设置多个标志；流类的构造函数作用类似该函数
  
  | 模式标志                      | 适用对象                    | 作用                                                         |
  | ----------------------------- | --------------------------- | ------------------------------------------------------------ |
  | `ios::in`                     | `ifstream/fstream`          | 打开文件用于读取数据。如果文件不存在，则打开出错             |
  | `ios::out`                    | `ofstream/fstream`          | 打开文件用于写入数据。如果文件不存在，则新建该文件；如果文件原来就存在，则打开时清除原来的内容 |
  | `ios::app`                    | `ofstream/fstream`          | 打开文件，用于在其尾部添加数据。如果文件不存在，则新建该文件 |
  | `ios::ate`                    | `ifstream`                  | 打开一个已有的文件，并将文件读指针指向文件末尾；如果文件不存在，则打开出错 |
  | `ios::trunc`                  | `ofstream`                  | 单独使用时与`ios::out`相同                                   |
  | `ios::binary`                 | `ifstream/ofstream/fstream` | 以二进制方式打开文件。若不指定此模式，则以文本模式打开       |
  | `ios::in|ios::out`            | `fstream`                   | 打开已存在的文件，既可读取其内容，也可向其写入数据。文件刚打开时，原有内容保持不变。如果文件不存在，则打开出错 |
  | `ios::in|ios::out`            | `ofstream`                  | 打开已存在的文件，可以向其写入数据。文件刚打开时，原有内容保持不变。如果文件不存在，则打开出错 |
  | `ios::in|ios::out|ios::trunc` | `fstream`                   | 打开文件，既可读取其内容，也可向其写入数据。如果文件本来就存在，则打开时清除原来的内容；如果文件不存在，则新建该文件 |
  
  `read(char* buffer, int count)`：从文件中读取`count`个字节的内容，存放到`buffer`所指向的内存缓冲区中，返回值是对函数所作用的对象的引用，如果想知道一共成功读取了多少个字节（读到文件尾时，未必能读取`count`个字节），可以在`read()`执行后立即调用文件流对象的`gcount()`成员函数，其返回值就是最近一次`read`函数执行时成功读取的字节数
  
  `write(char* buffer, int count)`：将内存中`buffer`所指向的`count`个字节的内容写入文件，返回值是对函数所作用的对象的引用
  
  | 指针函数                      | 适用对象           | 说明                 |
  | ----------------------------- | ------------------ | -------------------- |
  | `seekp(int offset, int mode)` | `ofstream/fstream` | 设置文件写指针的位置 |
  | `seekg(int offset, int mode)` | `ifstream/fstream` | 设置文件读指针的位置 |
  | `tellp()`                     | `ofstream/fstream` | 返回文件写指针的位置 |
  | `tellg()`                     | `ifstream/fstream` | 返回文件读指针的位置 |
  
  | 指针设置模式 | 说明                                                         |
  | :----------- | ------------------------------------------------------------ |
  | `ios::beg`   | 让文件读指针（或写指针）指向从文件开始向后的`offset`字节处。`offset`等于 0 即代表文件开头。在此情况下，`offset`只能是非负数 |
  | `ios::cur`   | `offset`为负数则表示将读指针（或写指针）从当前位置朝文件开头方向移动`offset`字节，为正数则表示将读指针（或写指针）从当前位置朝文件尾部移动`offset`字节，为 0 则不移动 |
  | `ios::end`   | 让文件读指针（或写指针）指向从文件结尾往前的`offset`的绝对值字节处，在此情况下，`offset`只能是非正数 |
  
  `close()`：关闭流对象打开的文件
  
+ `<sstream>`

  包含`istringstream, ostringstream`类，可以使用`<<, >>`进行流输入输出，通过`str()`成员函数能将输出到 ostringstream 对象中的内容提取出来

## C库

`<cstdio>`

说明：基本与 C 语言的`<stdio.h>`内容相同，但是其内部定义的变量可能在 `std` 命名空间中也存在

`cstdlib>`

说明：基本与 C 语言的`<stdlib.h>`内容相同

`<ctime>`

说明：基本与 C 语言的`<time.h>`内容相同

`<cmath>`

说明：基本与 C 语言的`<math.h>`内容相同

## C++11

`<memory>`

智能指针

格式：`shared_ptr<T> ptr(T *p)`

说明：

+ 多个`shared_ptr`对象可以共同托管一个指针 `p`，当所有曾经托管`p`的`shared_ptr`对象都解除了对其的托管时，就会执行`delete p`
+ `shared_ptr`对象能像指针`p`—样使用
+ 定义时不能同时定义两个托管指针都指向同一块内存，否则无法正常计数
+ `shared_ptr`对象有`get()/reset(T *p)`成员函数，用以获取指针或设置托管新指针