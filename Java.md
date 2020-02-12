[TOC]

# 数据类型

说明：

+ 数组定义使用`type[] v = new type[size]`或`type[] v = new type[] {...}`或`type[] v = {...}`
+ 从`java10`开始，可以使用`var`关键字初始化变量
+ 变量若未初始化，值总为`0`
+ 逻辑运算遵循短路运算
+ 引用类型的变量可以是空值`null`

| 类型      | 占用字节 | 备注                                                         |
| --------- | -------- | ------------------------------------------------------------ |
| `byte`    | `1B`     |                                                              |
| `short`   | `2B`     |                                                              |
| `int`     | `4B`     | 整数除以`0`会抛出错误                                        |
| `long`    | `8B`     |                                                              |
| `char`    | `2B`     | 可以表示`unicode`字符，通过使用`\u####`指定字符              |
| `float`   | `4B`     | `0/0`产生`NaN`，`+/0`产生`Infinity`，`-/0`产生`-Infinity`    |
| `double`  | `8B`     | `0/0`产生`NaN`，`+/0`产生`Infinity`，`-/0`产生`-Infinity`    |
| `boolean` | `1B/4B`  | 单个布尔元素占用`4B`，布尔数组中单个布尔元素占用`1B`         |
| `String`  | `NaN`    | 长度不可变，可用`null`初始化（与`""`不同），应使用`equals()`判断相等 |

# 流程控制

+ 包括`if-else, while, do-while, switch-case, break, continue`，用法类似C语言
+ 从`java11`开始，`for`增加了`for each`形式，能够直接迭代可迭代对象的元素，格式为`for (type element : iterator) {...}`
+ 从`java12`开始，增加了`switch`表达式，格式为`switch(val) { case k1(,...) -> v1; ... }`，能够返回对应值；若对应的为语句块，则可以通过使用`yield`返回对应值；`switch`表达式不需要`break`
+ `Lambda`表达式可替代单方法接口，格式为`([参数表]) -> {...}`，若只有一句返回语句，简化为`([参数表]) -> value`，参数表中不需要指定数据类型，单方法接口可用注解`@FunctionalInterface`标记
+ 如果某个方法签名和接口恰好一致，就可以直接传入方法引用，通过`::`引用

# 面向对象编程

## 类

+ `class`
  + 用`final`修饰的类不能被继承
+ 类内部定义的嵌套类需要将当前类实例化后才能实例化嵌套类
  
+ `field`：
  + `private`仅能被类内部访问，`protected`可以把访问权限控制在继承树内部，`public`可以任意访问
  + 可通过`set/get`方法提供接口给外部修改`field`
  + 若`field`为引用类型，外部修改只会改变引用对象的指向，即浅拷贝
  + 未初始化的`field`，值类型默认为 `0`，引用类型默认为 `null`
  + 用`final`修饰的字段必须在创建对象时初始化，随后不可修改
  + `static`字段由类和实例共享同一内存，可以通过`[实例].field`或`[类名].field`访问，推荐后者
  + 用`final`修饰局部变量可以阻止被重新赋值
+ `method`：
  + `private`则仅在内部可访问
  + `method`的参数可以用可变参数`public [类名](type ... v) {...}`，可避免传参时构造数组的麻烦
  + 用`final`修饰的`method`不能被派生类`Override`
  + `static`方法只能访问静态成员
+ `constructor`
  + 构造器的定义方式为`public [类名]([参数表]) {...}`，不需要返回值，调用需要用`new`
  + 构造函数能够重载，默认生成一个空构造函数（如果未定义），若已经定义则不再自动生成
  + 在执行构造器之前先初始化`field`字段，再执行构造函数
  + 可以在一个构造方法内部通过`this([参数表])`调用另一个构造方法，便于代码复用
  + 派生类的构造函数中必须要通过`super([参数表])`调用基类某个构造函数，不写则默认为`super()`

## 转型

+ 向上转型(upcasting)始终是安全的，能够直接利用派生类构造函数创建基类对象
+ 向下转型(downcasting)需要根据当前对象真实类型信息在继承链向上遍历，若发现新对象类型在继承链，则成功转型
+ 可以通过`instanceof`关键字进行类型判断，再进行向下转型

## 多态

+ 针对某个类型的方法调用，真正执行的方法取决于运行时期实际类型的方法
+ 通过`@Override`进行方法签名与基类相同的方法的覆写
+ 如果要调用父类的被覆写的方法，可以通过`super`来调用

## 抽象类

+ 通过`abstract class [类名] {...}`声明抽象类
+ 抽象类中必须存在`[public/protected] abstract [返回值] [函数]([参数表]);`的抽象方法声明
+ 抽象类通过`extend`继承，一次继承只能继承一个抽象类

## 接口

+ 接口与抽象类类似，一般不能在接口定义字段，但能定义`static`字段，且只能是`public static final`类型，因此也可以省略该声明，直接通过`[类型] [变量名] = [参数值]`进行定义
+ 接口通过`interface [类名] {...}`声明，通过`implements`实现
+ 接口能够同时实现多个，而抽象类不能
+ 接口的方法必须为`public`。默认即为`public`，故不需要指明访问权限
+ 子类接口通过`extend`继承基类接口
+ 从`java8`开始，能够在接口定义默认方法，格式为`default [返回值] [函数] {...}`，子类可以进行选择性覆盖

## 包

+ 通过`package [包名];`开头表示打包
+ 包作用域包括用`public`、`protected`、`private`修饰的字段和方法
+ 位于同一个包的类，可以访问包作用域的字段和方法
+ 通过`import [类路径]`引入外部类，使用`.`分隔路径
+ 默认自动`import`当前`package`的其他`class`和`java.lang.*`
+ 包名推荐使用倒置的域名
+ 打包`.jar`时，根目录直接放置包，内含`.class`文件

## 模块

+ 从`java9`开始，JDK引入了模块（Module），模块以`.jmod`扩展名标识，可在`$JAVA_HOME/jmods`目录下找到

+ 模块类似工程，包括`bin`和`src`等文件夹，其中`src`中有一个`module-info.java`文件，内容如下

  ```java
  module [模块名] {
  	requires java.base;	// 可不写，任何模块都会自动引入java.base
  	requires [依赖包];
      exports [导出包];    // 只有声明的导出的包，外部代码才被允许访问
  }
  ```

# 异常处理

+ Java使用异常来表示错误，并通过`try ... catch`捕获异常，可增加`finally`语句块，无论是否正确都执行该语句块

+ 若声明了可能抛出的异常，可以不写`catch`语句块

+ 多个`catch`语句的匹配顺序非常重要，子类必须放在前面

+ 一个`catch`语句也可以匹配多个非继承关系的异常，使用`|`连接多个异常类，如`catch(E1|E2 e) {...}`

+ Java的异常是`class`，并且从`Throwable`继承

+ `Error`是无需捕获的严重错误，`Exception`是应该捕获的可处理的错误

+ `RuntimeException`无需强制捕获，非`RuntimeException`（Checked Exception）需强制捕获，或者用`throws`声明

+ 当某个方法抛出了异常时，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个`try ... catch`被捕获为止

+ 通过`printStackTrace()`可以打印出方法的调用栈

+ 在`catch`中抛出异常，不会影响`finally`的执行。JVM会先执行`finally`，然后抛出异常

+ 当`catch`和`finally`都抛出了异常时，`catch`的异常将被屏蔽

+ 通常不要在`finally`中抛出异常，如果在`finally`中抛出异常，应该原始异常加入到原有异常中，调用方可通过`Throwable.getSuppressed()`获取所有添加的`Suppressed Exception`

+ 抛出异常时，尽量复用JDK已定义的异常类型

+ 自定义异常体系时，推荐从`RuntimeException`派生“根异常”，再派生出业务异常，同时应提供多种构造方法

+ 通过`assert expression`或`assert expression : message`断言，若为`false`则抛出`AssertionError`，对可恢复的错误不能使用断言，而应该抛出异常，一般只在开发和测试阶段启用断言

+ Java标准库内置`java.util.logging`，包括`severe/warning/info/config/fine/finer/finest`七个等级，可重定向，可定义格式，通过`Logger.getGlobal()`创建实例并使用`[等级](message)`方法进行日志记录

+ Java标准库定义的常用异常包括

  ```ascii
  Exception
  │
  ├─ RuntimeException
  │  │
  │  ├─ NullPointerException
  │  │
  │  ├─ IndexOutOfBoundsException
  │  │
  │  ├─ SecurityException
  │  │
  │  └─ IllegalArgumentException
  │     │
  │     └─ NumberFormatException
  │
  ├─ IOException
  │  │
  │  ├─ UnsupportedCharsetException
  │  │
  │  ├─ FileNotFoundException
  │  │
  │  └─ SocketException
  │
  ├─ ParseException
  │
  ├─ GeneralSecurityException
  │
  ├─ SQLException
  │
  └─ TimeoutException
  ```

# 反射

## Class类

+ JVM为每个加载的`class`及`interface`创建了对应的`Class`实例来保存`class`及`interface`的所有信息
+ 获取一个`class`对应的`Class`实例后，就可以获取该`class`的所有信息
+ 通过Class实例获取`class`信息的方法称为反射（Reflection）
+ JVM总是动态加载`class`，可以在运行期根据条件来控制加载class
+ 通过`[类名].class / [实例].getClass() / Class.forName([类名字符串])`获取`Class`实例，通过`==`判断是否为同一个类，通过`instanceOf`判断是否为后者类或其子类

## 访问字段

`Class`对象包含以下常用方法：

| 方法                           | 说明                                          |
| ------------------------------ | --------------------------------------------- |
| `Field getField(name)`         | 根据字段名获取某个public的field（包括父类）   |
| `Field getDeclaredField(name)` | 根据字段名获取当前类的某个field（不包括父类） |
| `Field[] getFields()`          | 获取所有public的field（包括父类）             |
| `Field[] getDeclaredFields()`  | 获取当前类的所有field（不包括父类）           |

`Field`对象包含以下常用方法（使用需要`import java.lang.reflect.Field`）：

| 方法              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| ``getName()``     | 返回字段名称                                                 |
| `getType()`       | 返回字段类型，也是一个`Class`实例                            |
| `getModifiers()`  | 返回字段的修饰符，它是一个`int`，不同的bit表示不同的含义，通过将其传入`Modifier.isX()`能够判断修饰符，包括`isFinal/isPublic/isProtected/isPrivate/isStatic`等 |
| `get()`           | 传入实例，返回字段值                                         |
| `set()`           | 传入实例和对应类型的字段值，能够重置原来的字段值             |
| `setAccessible()` | 传入`true`，表示不管字段是不是`public`，一律允许访问，该方法可能失败 |

## 调用方法

使用反射调用方法时，仍然遵循多态原则：即总是调用实际类型的覆写方法（如果存在）

`Class`对象包含以下常用方法：

| 方法                                       | 说明                                   |
| ------------------------------------------ | -------------------------------------- |
| ``Method getMethod(name, Class...)``       | 获取某个`public`的`Method`（包括父类） |
| `Method getDeclaredMethod(name, Class...)` | 获取当前类的某个`Method`（不包括父类） |
| `Method[] getMethods()`                    | 获取所有`public`的`Method`（包括父类） |
| `Method[] getDeclaredMethods()`            | 获取当前类的所有`Method`（不包括父类） |

`Method`对象包含以下常用方法（使用需要`import java.lang.reflect.Method`）：

| 方法                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| `getName()`           | 返回方法名称                                                 |
| `getReturnType()`     | 返回方法返回值类型                                           |
| `getParameterTypes()` | 返回方法的参数类型，是一个Class数组                          |
| `getModifiers()`      | 返回方法的修饰符，它是一个`int`，不同的bit表示不同的含义     |
| `invoke()`            | 传入实例和参数值，得到方法调用的返回值，静态方法第一个参数只需传入`null` |
| `setAccessible()`     | 传入`true`，表示不管方法是不是`public`，一律允许访问，该方法可能失败 |

## 调用构造方法

调用非`public`的`Constructor`时，必须通过`setAccessible(true)`设置允许访问，`setAccessible(true)`可能会失败

`Class`对象包含以下常用方法：

| 方法                               | 说明                            |
| ---------------------------------- | ------------------------------- |
| `getConstructor(Class...)`         | 获取某个`public`的`Constructor` |
| `getDeclaredConstructor(Class...)` | 获取某个`Constructor`           |
| `getConstructors()`                | 获取所有`public`的`Constructor` |
| `getDeclaredConstructors()`        | 获取所有`Constructor`           |

`Constructor`对象包含以下常用方法（使用需要`import java.lang.reflect.Constructor`）：

| 方法            | 说明                     |
| --------------- | ------------------------ |
| `newInstance()` | 传入参数，得到实例返回值 |

## 获取继承关系

`Class`对象包含以下常用方法：

| 方法90                    | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `getSuperclass()`         | `Object`的父类是`null`，除`Object`外，其他任何非`interface`的`Class`都必定存在一个父类，对所有`interface`的`Class`调用`getSuperclass()`返回的是`null` |
| `getInterfaces()`         | 返回当前类直接实现的接口类型，不包括其父类实现的接口类型     |
| `isAssignableFrom(Class)` | 判断能否向上转型                                             |

# 泛型

+ 通过`class [类名]<[类型符],...>`定义泛型模板，向上转型时，`[类型符]`不能向上转型，但`[类名]`可以
+ 在定义泛型时，声明具体类型后，`new`以后的类型的尖括号中的具体类型可省略
+ 泛型类型`<T>`不能用于静态方法，对于静态方法，可通过`[访问权限] static<[类型符]> [方法]([参数表]) [...]`定义，此处的类型符与最初的`<T>`已无任何关系，应避免重名
+ 泛型不能是基本类型，不能获取带泛型类型的`Class`，不能通过`instanceOf`判断带泛型类型的类型，不能实例化`<T>`的类型，泛型方法要防止重复定义方法（包括父类）
+ 泛型能够在指定类型符后被继承
+ 使用`extends`通配符表示可读不可写，使用`<T extends [类名]>`通配符表示泛型类型限定为`[类名]`及其子类
+ 使用`super`通配符表示可写不可读，使用`<T super [类名]>`通配符表示泛型类型限定为`[类名]`及其超类
+ 选择`extends / super`遵循`PECS(Producer Extends Consumer Super)`原则
+ 通过`Array.newInstance(Class, int)`创建`T[]`数组，需要强制转型

# 集合

## List

+ `List`有`ArrayList`与`LinkedList`，优先使用前者，需要`import java.util.List/ArrayList`

+ `List`的遍历建议使用`for each`，也可使用`get()`方法或`Iterator<T> it = list.iterator()`进行遍历，并通过`it.hasNext()`判断是否遍历结束

  | 方法                       | 说明                                                         |
  | -------------------------- | ------------------------------------------------------------ |
  | `add()/addAll()`           | 传入元素或下标和元素或集合类加入元素                         |
  | `clear()`                  | 清空元素                                                     |
  | `contains()/containsAll()` | 是否包含给定元素                                             |
  | `equal()`                  | 判断内容是否相同，内部使用元素的`public boolean equals(Object)`方法判断 |
  | `get()`                    | 获取特定下标元素                                             |
  | `indexOf()`                | 获取给定元素位置                                             |
  | `isEmpty()`                | 是否为空                                                     |
  | `iterator()`               | 获取迭代器，实现`Iterable`接口的集合类都可以用`for each`循环来遍历 |
  | `of()`                     | 传入多个同类型元素或数组创建实例                             |
  | `remove`                   | 传入元素或下标删除元素                                       |
  | `size()`                   | 获取元素数量                                                 |
  | `sort()`                   | 传入`Comparator`进行排序                                     |
  | `toArray()`                | 传入数组，将元素复制入数组，若大小不足则开新数组存入，反则剩余置`null` |

## Map

+ 需要`import java.util.Map`，属于键值（key-value）映射表的数据结构，能高效通过`key`快速查找`value`（元素），最常用为`HashMap`，需要`import java.util.HashMap`
+ 通过`entrySet() / keySet() / values()`获得键值对集合（类型为`Map.Entry<K, V>` / 键集合 / 值集合
+ 通过`getKey() / getValue()`获取`Map.Entry<K, V>`类的键和值
+ 通过`containsKey() / containsValue()`判断是否存在给定的键或值
+ 通过`put() / putIfAbsent() / putAll()`添加键值对 / 不存在的键值对 / 多个键值对
+ 通过`get() / getOrDefault()`获取值或给定不存在时的默认值
+ 通过`V remove(K) / boolean remove(K, V)`删除键值对，根据返回值有所不同选择性使用
+ 通过`V replace(K, V) / boolean replace(K, V, V)`更新值
+ 如果键的类型是自定义类，需要正确覆写`int hashCode()`
+ 如果键的类型是`Enum`，可选更高效的`EnumMap`，通过`new EnumMap<>([枚举类].class)`创建实例，，需要`import java.util.EnumMap`
+ 如果需要按照键的排序遍历，可选`TreeMap`，键的类型必须实现`Comparable<K>`接口或传入自定义比较算法`conparator`，需要`import java.util.TreeMap`
+ 如果需要加载配置文件`.props`，可选`Properties`，通过`new Properties()`创建实例，通过`load(Reader/InputStream)`加载文件，通过`getProperties() / setProperties()`读写，可设置默认值，通过`store(Writer/OutputStream, comments)`写入配置文件，使用时需要`import java.util.Properties`，同时`throws IOException`，字节流`InputStream/OutputStream`不能读写中文，而字符流`Reader/Writer`可以

## Set

+ `Set`通过`Map`实现，元素不会重复，包括`HashSet / TreeSet`等，`TreeSet`需要元素类实现`Comparable`接口或传入自定义比较算法`conparator`，需要`import java.util.Set/HashSet/TreeSet`
+ 通过`add()/ remove() / contains()`添加 / 删除 / 判断存在

## Queue

|                    | throw Exception | 返回false或null      |
| :----------------- | :-------------- | -------------------- |
| 添加元素到队尾     | `add(E e)`      | `boolean offer(E e)` |
| 取队首元素并删除   | `E remove()`    | `E poll()`           |
| 取队首元素但不删除 | `E element()`   | `E peek()`           |

+ 避免把`null`添加到队列
+ 优先队列可用`PriorityQueue`，通过`new PriorityQueue<E>(Comparator)`创建实例，`PriorityQueue`默认按元素比较的顺序排序（必须实现`Comparable`接口），也可以通过`Comparator`自定义排序算法
+ 双端队列可用`Deque`，允许两头进两头出，`Deque`是一个接口，它的实现类有`ArrayDeque`和`LinkedList`，通过`addLast() / addFirst() / offerLast() / offerFirst()`添加元素，通过`removeLast() / removeFirst() / pollLast() / pollFirst()`删除元素，通过`getLast() / getFirst() / peekLast() / peekFirst()`获取元素

## Stack

+ 由于存在遗留类名叫`Stack`，出于兼容性考虑，没办法创建`Stack`接口，只能用`Deque`接口来模拟`Stack`
+ 只使用`push() / addFirst()`入栈，使用`pop() / removeFirst()`出栈，使用`peek() / peekFirst()`取栈顶元素

## Iterator

+ 自定义可迭代对象需要实现`Iterable<T>`接口，并覆写`public Iterator<T> iterator()`，其返回值需自定义类实现`Iterator<T>`接口，该自定义类需覆写`public boolean hasNext()`和`public T next()`
+ 只要实现了`Iterable<T>`接口，都能通过`for each`遍历

## Collections

| 方法              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| `addAll()`        | 传入集合类和多个元素，将多个元素加入集合                     |
| `emptyX()`        | 创建空集合，等价于对应类型的`of()`方法                       |
| `singletonX(T)`   | 创建一个单元素不可变集合，无法向其中添加或删除元素           |
| `unmodifiableX()` | 封装成不可变集合，对原始`List`进行增删可影响到封装后的不可变`List` |
| `synchronizedX()` | 封装成线程安全集合，`java5`以后很少再使用                    |
| ``sort()`         | 只能传入可变`List`进行元素排序                               |
| `shuffle()`       | 只能传入可变`List`进行洗牌打乱                               |

# IO

## File

+ 通过`new File(pathname)`创建文件对象，可用相对路径和绝对路径

+ 可以配合使用`Path`对象，它位于`java.nio.file`，通过`Paths.get()`静态方法合并路径，通过`toAbsolutePath() / normalize() / toFile()`转换对象的格式或类型，可以通过`for each`遍历路径（被分隔符`File.separator`分割）

  | 方法                                    | 说明                                                         |
  | --------------------------------------- | ------------------------------------------------------------ |
  | `getPath()`                             | 返回构造方法传入的路径                                       |
  | `getAbsolutePath()`                     | 返回绝对路径                                                 |
  | `getCanonicalPath()`                    | 和绝对路径类似，但返回规范路径                               |
  | `isFile()`                              | 判断是否是一个已存在的文件                                   |
  | `isDirectory()`                         | 判断是否是一个已存在的目录                                   |
  | `canRead() / canWrite() / canExecute()` | 判断是否可读可写可执行，对目录而言，是否可执行表示能否列出它包含的文件和子目录 |
  | `createNewFile() / delete()`            | 创建或删除文件                                               |
  | `createTempFile() / deleteOnExit()`     | 创建临时文件以及在JVM退出时自动删除该文件                    |
  | `list() / listFiles()`                  | 返回文件名数组或文件数组，可能返回`null`，后者可传入`FilenameFilter`对象，通过覆写`public boolean accept(File, String)`过滤文件 |
  | `mkdir() / mkdirs() / delete()`         | 创建目录，或在创建同时也创建必需的父目录，或删除空文件夹（必须为空） |
  | `length()`                              | 获取文件字节大小                                             |

## InputStream/OutputStream

+ `InputStream`就是Java标准库提供的最基本的输入流，位于`java.io`，提供了所有同步IO的功能，`InputStream`并不是一个接口，而是一个抽象类，它是所有输入流的超类，常用的有`FileInputStream / ByteArrayInputStream`
+ 通过``FileInputStream(filename) / ByteArrayInputStream(Byte[])`创建实例
+ 通过`int read()`读取一个字节，或通过`int read(char[] buf) / int read(char[] buf, int off, int len)`一次性读取多个字节
+ 通过`close()`关闭文件，可用`try...finally`关闭，也可用`try(resourse)`自动关闭（`java7`以后可用），需要实现`java.lang.AutoCloseable`接口
+ `OutputStream`多种性质类似`InputStream`，可通过`void write() / void write(byte[])`，调用`close()`时自动清空缓冲区
+ 通过一个“基础”组件再叠加各种“附加”功能组件的模式，称之为Filter模式（或者装饰器模式：Decorator）。它可以让我们通过少量的类来实现各种功能的组合，可通过`extends FilterInputStream`等方式增加类的自定义功能

## Reader/Writer

+ `Reader`是Java的IO库提供的另一个输入流接口，是一个字符流，以`char`为单位读取
+ `FileReader`子类需要额外指定编码方式`StandardCharsets.X`
+ `CharArrayReader`行为类似`ByteArrayInputStream`，通过传入`char[]`创建`Reader`，`StringReader`不需指定编码
+ 通过`InputStreamReader(InputStream, StandardCharsets.X)`能够把`InputStream`转换为`Reader`
+ `Writer`的`write()`方法可接受`write / char[] / String`数据
+ `CharArrayWriter`行为类似`ByteArrayOutputStream`，类似的还有`FileWriter / StringReader`等
+ 通过`OutputStreamWriter(OutputStream, StandardCharsets.X)`能够把`OutputStream`转换为`Writer`

## PrintStream/PrintWriter

+ `PrintStream`和`OutputStream`相比，添加了一组`print()`/`println()`方法，可以打印各种数据类型，还会抛出`IOException`，编写代码时不必捕获`IOException`
+ `System.out`是标准输出，`System.err`是标准错误输出
+ `PrintWriter`扩展了`Writer`接口，它的`print()`/`println()`方法最终输出的是`char`数据，两者使用方法几乎一样

## 序列化

+ 对象要能序列化，必须实现一个特殊的`java.io.Serializable`空接口，称为“标记接口”（Marker Interface）
+ 通过`ObjectOutputStream(ByteArrayOutputStream)`将对象写入字节流，再通过`toByteArray()`写入文件
+ 通过`ObjectInputStream(ByteArrayInputStream)`将字节流写入对象，再通过`readObject()`转为对象
+ 序列化和反序列化不仅针对`Object`，也能针对基本数据类型

# 日期与时间

## Date/Celendar/TimeZone

三者均位于`java.util`，属于旧API，存在一些遗留问题，不建议使用

计算机表示的时间是以整数表示的时间戳存储的，即Epoch Time，Java使用`long`型来表示以毫秒为单位的时间戳，通过`System.currentTimeMillis()`获取当前时间戳

+ `Date`

  + `java.util.Date`是用于表示一个日期和时间的对象，注意与`java.sql.Date`区分，后者用在数据库中

  + 通过`new Date(long)`创建实例（毫秒精度时间戳）

  + 通过`getX()`方法获取年月日时分秒等信息，年份需要加上1900，月份需要加1

  + 通过`SimpleDateFrmat(String)`实例的`format(date)`方法能够格式化时间，使用需要`import java.text.SimpleDateFormat`，常用的预定义字符串如下

  + | 符号    | 意义               | 符号 | 意义               |
    | ------- | ------------------ | ---- | ------------------ |
    | G       | 年代标志符         | y    | 年                 |
    | M       | 月                 | d    | 日                 |
    | h/H/k/K | 时                 | m    | 分                 |
    | s       | 秒                 | S    | 毫秒               |
    | E       | 星期               | D    | 一年中的第几天     |
    | F       | 一月中第几个星期几 | w    | 一年中第几个星期   |
    | `W`     | 一月中第几个星期   | a    | 上午 / 下午 标记符 |
    | k/k     | 时 在一天中 (1~24) | z    | 时区               |

+ `Calendar`

  + `Calendar`可以用于获取并设置年、月、日、时、分、秒，可以做简单的日期和时间运算
  + 通过`Calendar.getInstance()`静态方法获取实例，通过`get(Calendar.X)`获取特定值，返回的年份不必转换，返回的月份仍然要加1，返回的星期要特别注意，`1`~`7`分别表示周日到周六
  + 通过`clear()`能够清除信息并通过`set(Calendar.X, int)`对时间进行重新设置
  + 通过`add(Calendar.X, int)`对时间进行加减
  + 通过`getTime()`能够将一个`Calendar`对象转换成`Date`对象

+ `TimeZone`

  + 通过`String[] TimeZone.getAvailableIDs()`可以列出系统支持的所有ID


## LocalDateTime

从`java 8`开始，`java.time`包提供了新的日期和时间 API，以及新的格式化类型`DateTimeFormatter`

+ 通过`LocalDateTime.now() / LocalDate.now() / LocalTime.now()`能够获取当前时间
+ 通过`LocalDateTime`实例的`toLocalDate() / toLocalTime()`方法能够获取另两者的实例，防止代码运行时间差
+ 通过`DateTimeFormatter.ofPattern(String)`自定义样式，预定义字符串类似`SimpleDateFormat`，并可通过`DateTimeFormatter`实例的`format(LocalDateTime/LocalDate/LocalTime)`方法获得自定义格式的日期
+ 通过`LocalDateTime/LocalDate/LocalTime`的`parse(String, DateTimeFormatter)`静态方法可解析出实例，`parse()`方法默认严格按照 ISO 8601 的格式`yyyy-MM-ddTHH:mm:ss.SSS`及其分支
+ 通过`plusX() / minusX()`方法能够对日期时间进行加减运算，通过`withX()`方法能够重置日期时间，方法能够自动进行合理调节，其中所涉及的`X`可以是`Years/Months/Weeks/Days/Hours/Minutes/Seconds`，`with`的`Days`则应替换为`DayOfMonth/DayOfYear`
+ 通过`isBefore() / isAfter()`方法可以判断两个日期时间的先后关系
+ `LocalDateTime`无法与时间戳进行转换

## ZonedDateTime

+ 可通过`ZonedDateTime.now()`获取系统默认时区的当前日期时间，也可以传入`ZoneId.of(String)`指定时区
+ 可通过`LocalDateTime`实例的`atZone(ZoneId.of(String))`方法获取指定时区的日期时间
+ 可通过`withZoneSameInstant(ZoneId.of(String))`方法获取指定时区的日期时间
+ 通过`toLocalDateTime() / toLocalDate() / toLocalTime`能够获取三者实例，但丢失了时区信息
+ 仍然有`plusX() / minusX() / getX() / withX()`等方法，操作类似`LocalX`
+ 输出格式为`yyyy-MM-ddTHH:mm:ss[[+/-]timezone][locale]`

## DateTimeFormatter

+ 位于`java.time.format`
+ `DateTimeFormatter`不但是不变对象，它还是线程安全的
+ 通过`DateTimeFormatter.ofPattern(String)`创建实例，还可再传入`Locale.X`针对地区语言文字进行格式化
+ `DateTimeFormatter`内部预定义了一些常用的标准格式器

## Instant

+ 通过`Instant.now() / Instant.ofEpochMillis() / Instant.ofEpochSecond()`获取时间戳
+ 通过`toEpochMilli() / getEpochSecond()`获得 毫秒 / 秒 精度的时间戳，`getNano()`获得仅纳秒时间戳
+ 通过`atZone(ZoneId)`指定时区后可以获得`ZonedDateTime`实例

## Duration/Period

+ `Duration`
  + 两个`LocalDateTime`之间差值用`Duration`表示
  + 可通过`parse()`静态方法传入格式串创建实例，格式为`P...T...`，如`P1DT2H3M`代表`1 day, 2 hours, 3 minutes`
  + 可通过`ofX()`静态方法创建实例，其中`X`可以是`Days/Hours/Minutes/Seconds/Millis/Nanos`
  + 可通过`between()`静态方法传入两个`Temporal`对象创建实例或`from()`传入一个`TemporalAmount`对象创建实例

## 转换

```java
// Data -> ZonedDateTime
date.toInstant().atZone(ZoneId)
// Calendar -> ZonedDateTime
calendar.toInstant().atZone(calendar.getTimeZone().toZoneId())
// ZonedDateTime -> Date
new Date(zdt.toEpochSecond() * 1000)
// ZonedDateTime -> Calendar
calendar.clear();
calendar.setTimeZone(TimeZone.getTimeZone(zdt.getZone().getId()));
calendar.setTimeInMillis(zdt.toEpochSecond() * 1000);
```

# 正则表达式

##  匹配规则

| 正则表达式 | 规则                     | 正则表达式   | 规则             |
| :--------- | :----------------------- | ------------ | ---------------- |
| `A`        | 指定字符                 | `\u####`     | 指定Unicode字符  |
| `.`        | 任意字符                 | `\d`         | 数字0~9          |
| `\w`       | 大小写字母，数字和下划线 | `\s`         | 空格、Tab键      |
| `\D`       | 非数字                   | `\W`         | 非\w             |
| `\S`       | 非\s                     | `A*`         | 任意个数字符     |
| `A+`       | 至少1个字符              | `A?`         | 0个或1个字符     |
| `A{3}`     | 指定个数字符             | `A{2,3}`     | 指定范围个数字符 |
| `A{2,}`    | 至少n个字符              | `A{0,3}`     | 最多n个字符      |
| `^`        | 开头                     | `$`          | 结尾             |
| `[ABC]`    | […]内任意字符            | `[A-F0-9xy]` | 指定范围的字符   |
| `[^A-F]`   | 指定范围外的任意字符     | `AB|CD|EF`   | AB或CD或EF       |
| `?`        | 非贪婪匹配               |              |                  |

## 相关方法

需要`import java.util.regex.*`

| 方法                             | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| `str.matches(String)`            | 判断是否匹配                                                 |
| `str.split(String)`              | f=根据匹配字符串切割主串为多个子串                           |
| `str.replaceAll(String, String)` | 传入正则表达式和字符串进行匹配和替换，通过`$\d`对匹配的子串反向引用 |
| `Pattern.compile(String)`        | 获取`Pattern`对象                                            |
| `pattern.matcher(String)`        | 获取`Matcher`对象                                            |
| `matcher.matches()`              | 判断是否匹配                                                 |
| `matcher.group(int)`             | 获取分组匹配子串，`[0]`为字符串本身                          |

# 多线程

+ `new Thread(<? implements Runnable>) / new <? extends Thread>()`：需要正确覆写`void run()`方法，也可用`Lambda`表达式创建，也可通过`new Thread() { public void run() {...} }`创建
+ `Thread.sleep(ms)`：对当前线程暂停一段时间
+ `thread.start()`：开始线程，只能调用一次
+ `thread.join()`：当前线程等待该线程完成再继续执行后续代码
+ `thread.interrupt()`：中断线程，目标线程需要反复检测自身状态是否是interrupted状态，如果是，就结束运行
+ `thread.setPriority(int) `：对线程设定优先级（1-10）， 默认值 5
+ `thread.setDaemon(boolean)`：设置目标线程为守护线程，守护线程不能持有任何需要关闭的资源，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出
+ `volatile`：作为修饰符，针对线程间共享的变量
+ `synchronized`：作为修饰符，表示整个方法都必须用`this`实例加锁，把整个方法变为同步代码块
+ `synchronized(Object) {...}`：对一个对象进行加锁，执行结束后，在`synchronized`语句块结束会自动释放锁，属于可重入锁，即代码块内部能够重新获得锁
+ `Object.wait()`：针对作为锁的对象，释放锁从而使得其他线程能够获得该锁
+ `Object.notify() / Object.notifyAll()`：针对作为锁的对象，唤醒其他等待线程
+ `Executors.newFixedThreadPool(int)`：创建固定大小的线程池，属于`ExecutorService`类，需要`import java.util.concurrent.*`
+ `executorService.submit()`：传入线程到线程池，若传入的线程实现了`Callable`泛型接口，可返回`Future<V>`，调用`Future`对象的`get()`方法，就可以获得异步执行的结果，如果异步任务还没有完成，那么会阻塞至任务完成，可通过`isDone()`判断任务是否已完成，通过`cancel(ture)`取消当前任务，通过`get(long, TimeUnit)`也可以获取结果，但只等待指定的时间
+ `executorService.shutdown() `：等待任务完成关闭线程池，`shutdownNow()`立即关闭，`awaitTermination()`则会等待指定的时间让线程池关闭
+ `Executors.newScheduledThreadPool(int)`：创建固定大小的定时执行的线程池，属于`ScheduledExecutorService`类
+ `scheduledExecutorService.schedule()`：传入一次性线程，可再传入时间长度和时间单位
+ `scheduledExecutorService.scheduleAtFixedRate()`：传入周期线程，可再传入延迟时间、时间长度和时间单位
+ `scheduledExecutorService.scheduleWithFixedDelay()`：传入等间隔线程，可再传入延迟时间、时间长度和时间单位
+ `CompletableFuture.supplyAsync()`：针对`Future`做了改进，可以传入回调对象，当异步任务完成或者发生异常时，自动调用回调对象的回调方法，返回`CompletableFuture<V>`，通过其`thenAccept() / exceptionally()`设置成功和失败对应的回调方法，`thenApplyAsync()`用于串行化另一个`CompletableFuture`，`anyOf()`和`allOf()`两个静态方法用于并行化多个`CompletableFuture`


# 标准库

## String

| 方法                                            | 说明                                               |
| ----------------------------------------------- | -------------------------------------------------- |
| `contains()`                                    | 是否包含某子串                                     |
| `equals()/equalsIgnoreCase()`                   | 字符串内容是否相同，后者忽略大小写                 |
| `isEmpty()/isBlank()`                           | 是否为空或空字符                                   |
| `indexOf()/lastIndexOf()`                       | 返回包含某子串的索引                               |
| `length()`                                      | 返回字符串长度                                     |
| `replace()/replaceAll()/replacFirst()`          | 替换子串为另一个子串                               |
| `startsWith()/endsWith()`                       | 是否以某子串开始或结束                             |
| `split()`                                       | 分割子串为字符串数组                               |
| `substring()`                                   | 获取子串                                           |
| `toCharArray()`                                 | 转换为字符数组                                     |
| `toUpperCase()/toLowerCase()`                   | 转换字母为全大写或小写                             |
| `trim()/strip()/stripLeading()/stripTrailing()` | 移除两端空白字符如`\s\n\r\t`，前者不可移除中文空格 |
| `String.join()`                                 | 在字符串每个字符之间插入另一子串                   |
| `String.valueOf()`                              | 转换其他类型为字符串                               |
| `X.parseX()`                                    | 转换字符串为其他类型                               |

## StringBuilder

| 方法                      | 说明                                         |
| ------------------------- | -------------------------------------------- |
| `append()`                | 在字符串后添加字符串（可以传入多种基本类型） |
| `delete()/deleteCharAt()` | 删除特定子串                                 |
| `indexOf()`               | 返回包含某子串的索引                         |
| `insert()`                | 在特定位置插入字符串                         |
| `reverse()`               | 翻转字符串                                   |
| `toString()`              | 转换为`String`对象                           |

## StringJoiner

使用需要`import java.util.StringJoiner`

| 方法             | 说明                                       |
| ---------------- | ------------------------------------------ |
| `StringJoiner()` | 指定插入分隔符进行实例化，可选指定开头结尾 |
| `add()`          | 在字符串后添加字符串，自动插入分隔符       |
| `toString()`     | 转换为`String`对象                         |

## BigInteger

使用需要`import java.math.BigInteger`

+ `BigInteger`用于表示任意大小的整数，且是不变类，继承自`Number`
+ 将`BigInteger`转换成基本类型时可使用`XValueExact()`等方法保证结果准确
+ 运算只能使用实例方法，如`add/multiply/divide/abs/pow/and/or/not/xor`等

## BigDecimal

使用需要`import java.math.BigDecimal`

+ 通过`new BigDecimal(String)`创建实例
+ 通过`scale()`获取小数位数，若为`-x`表示为整数，且有`x`个 0
+ 通过`stripTrailingZeros()`方法，可格式化为一个相等的，但去掉了末尾0的`BigDecimal`
+ 通过`setScale(int, RoundingMode.HALF_UP/DOWN)`决定精度和截断方式，需要`import java.math.RoundingMode`
+ 加减乘绝对准确，不影响精度，除法则需要指定结果精度和截断方式
+ 做除法的同时求余数可用`divideAndRemainder()`，返回`BigDecimal[]`分别存储商数和余数
+ 比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`，后者还要求精度相同

## Math

常用的包括`pow/sqrt/exp/log/log10/sin/cos/tan/asin/acos/atan/abs/max/min/PI/E/random`等

## Random/SecureRandom

使用需要`import java.util.Random`或`import java.security.SecureRandom`

+ 通过`new Random()`创建实例，再通过`nextX()`获取随机数，若获取随机浮点数，范围为`[0,1)`
+ `Random`用来创建伪随机数，只要给定一个初始的种子，产生的随机数序列是完全一样的
+ `SecureRandom`的安全性是通过操作系统提供的安全的随机种子来生成随机数。这个种子是通过CPU的热噪声、读写磁盘的字节、网络流量等各种随机事件产生的“熵”

## 包装类型

+ 通过`new X(val)`或`X.valueOf(val)`进行初始化
+ 通过`Xvalue()`方法获得对应基础数据类型值
+ 必须使用`equals()`判断相等
+ 直接把基本类型赋值为包装类型的赋值写法称为自动装箱（Auto Boxing），反则称为自动拆箱（Auto Unboxing）
+ 部分包装类型内置有用的常数，如`TRUE/FALSE/MAX_VALUE/MIN_VALUE/SIZE/BYTES`
+ `Integer.valueOf()`可传入整数和字符串，传入字符串后可选指定基数
+ `Integer`实例还有多种`Yvalue()`方法获取多种基础数据类型值
+ 静态方法`X.parseX()`将字符串转换为特定数据类型
+ `Integer`静态方法可用`toString()/toHexString()/toOctalString()/toBinaryString()`等把整数格式化为指定进制的字符串，其中`toString()`可指定进制

| 基本类型  | 对应的引用类型        |
| :-------- | :-------------------- |
| `boolean` | `java.lang.Boolean`   |
| `byte`    | `java.lang.Byte`      |
| `short`   | `java.lang.Short`     |
| `int`     | `java.lang.Integer`   |
| `long`    | `java.lang.Long`      |
| `float`   | `java.lang.Float`     |
| `double`  | `java.lang.Double`    |
| `char`    | `java.lang.Character` |

## 枚举类

+ Java使用`enum`定义枚举类型，它被编译器编译为`final class [枚举类名] extends Enum { … }`
+ 通过`name()`获取常量定义的字符串，注意不要使用`toString()`
+ 通过`ordinal()`返回常量定义的顺序（无实质意义）
+ 通过`private [类名]([参数表])`自定义构造函数（可选），并通过`[枚举名]([参数表])`在类内部初始化
+ `enum`的构造方法要声明为`private`，字段强烈建议声明为`final`
+ 为了可视化可以`@Override`其`toString()`方法
+ `enum`适合用在`switch`语句中

# API

## `java.util.Arrays`

| 函数                 | 作用                         |
| -------------------- | ---------------------------- |
| `toString(type[] v)` | 将数组转为字符串以便输出     |
| `sort(type[] v)`     | 对数组进行默认从小到大的排序 |

## `java.util.Scanner`

| 函数                | 作用                                                         |
| ------------------- | ------------------------------------------------------------ |
| `Scanner(stream s)` | 绑定输入流从而通过控制台获取输入，实例有`nextType()`类型的方法，其中`next()`和`nextLine()`的后者能够读取空格，而前者不行 |

