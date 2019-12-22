[TOC]

# 关键字

| 关键字     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| `True`     | 布尔类型的值，表示真，与 False 相反                          |
| `False`    | 布尔类型的值，表示假，与 True 相反                           |
| `None`     | `None`比较特殊，表示什么也没有，它有自己的数据类型`NoneType` |
| `and`      | 用于表达式运算，逻辑与操作                                   |
| `as`       | 用于类型转换                                                 |
| `assert`   | 断言，用于判断变量或者条件表达式的值是否为真                 |
| `break`    | 中断循环语句的执行                                           |
| `class`    | 用于定义类                                                   |
| `continue` | 跳出本次循环，继续执行下一次循环                             |
| `def`      | 用于定义函数或方法                                           |
| `del`      | 删除变量或序列的值                                           |
| `elif`     | 条件语句，与`if、else`结合使用                               |
| `else`     | 条件语句，与`if、elif`结合使用。也可用于异常和循环语句       |
| `except`   | `except`包含捕获异常后的操作代码块，与`try、finally`结合使用 |
| `finally`  | 用于异常语句，出现异常后，始终要执行`finally`包含的代码块。与`try、except`结合使用 |
| `for`      | `for`循环语句                                                |
| `from`     | 用于导入模块，与`import`结合使用                             |
| `global`   | 定义全局变量                                                 |
| `if`       | 条件语句，与`else、elif`结合使用                             |
| `import`   | 用于导入模块，与`from`结合使用                               |
| `in`       | 判断变量是否在序列中                                         |
| `is`       | 判断变量是否为某个类的实例                                   |
| `lambda`   | 定义匿名函数                                                 |
| `nonlocal` | 用于标识外部作用域的变量                                     |
| `not`      | 用于表达式运算，逻辑非操作                                   |
| `or`       | 用于表达式运算，逻辑或操作                                   |
| `pass`     | 空的类、方法或函数的占位符                                   |
| `raise`    | 异常抛出操作                                                 |
| `return`   | 用于从函数返回计算结果                                       |
| `try`      | `try`包含可能会出现异常的语句，与`except、finally`结合使用   |
| `while`    | `while`循环语句                                              |
| `with`     | 简化`Python`的语句                                           |
| `yield`    | 用于从函数依次返回值                                         |

# 数据类型

## 数字(`number`)

+ 整数(`integer`)
  - `int(number, base=10)`：将`number`转化为整数
  - `bin(number)`：将`number`转化为二进制数
  - `oct(number)`：将`number`转化为八进制数
  - `hex(number)`：将`number`转化为十六进制数
  - `divmod(a, b)`：返回`a//b, a%b`
+ 浮点数(`float`)
  + `float(number)`：将`number`转化为浮点数
  + `round(number, precision=None)`：返回`number`的四舍五入值（`0.5`也会舍去），指定`precision`可控制精度
+ 复数(`complex`)
  + `complex(number)`：将`number`转化为复数
  + `z.conjugate()`：返回`z`的共轭复数
  + `z.imag`：返回`z`的虚部
  + `z.real`：返回`z`的实部

## 字符串(`string`)

+ 相关操作
  + 建立：`"content", 'content', str()`
  + 切片：`var[start:end:step]`
  + 索引：`var[i]`
+ 相关函数
  + `str(var)`：将`var`转化为字符串
  + `eval(str)`：将`str`去掉双引号后计算表达式的值
  + `ord(char)`：返回`char`的`ascii`码
  + `chr(n)`：返回`n`对应的`ascii`字符
+ 相关方法
  + `'_{:[char][align][width][.accuracy]type}_'.format(parameters)`：格式化字符串，可引用`(*list)(**dict)`
  + `s.encode(encoding='UTF-8', error='strict')`：返回编码后的二进制字符串
  + `s.decode(encoding='UTF-8', error='strict')`：返回解码后的字符串
  + `s.join(iterable)`：将`s`插入到所有元素为`str`的可迭代对象之间形成`str`
  + `s.strip([chars])`：移除`s`左右两边的在`chars(default \s\n\t)`中出现过的所有字符
  + `s.lstrip([chars])`：移除`s`左边的在`chars(default \s\n\t)`中出现过的所有字符
  + `s.rstrip([chars])`：移除`s`右边的在`chars(default \s\n\t)`中出现过的所有字符
  + `s.split([sep[,maxsplit])`：从左以`sep(default \s\n\t)`分割`maxspilt`次并除去中间的`sep`
  + `s.rsplit([sep[,maxsplit])`：从右以`sep(default \s\n\t)`分割`maxspilt`次并除去中间的`sep`
  + `s.find(sub[,start[,end]])`：从`start`到`end-1`中从左找`sub`并返回索引位置，无则返回`-1`
  + `s.rfind(sub[,start[,end]])`：从`start`到`end-1`中从右找`sub`并返回索引位置，无则返回`-1`
  + `s.index(sub[,start[,end]])`：从`start`到`end-1`中从左找`sub`并返回索引位置，无则抛出`ValueError`
  + `s.rindex(sub[,start[,end]])`：从`start`到`end-1`中从右找`sub`并返回索引位置，无则抛出`ValueError`
  + `s.replace(ole,new[,count])`：从左将`old`替换为`new`进行`count`次
  + `s.partition(sep)`：从左的第一个`sep`开始将`s`切为`(str1,sep,str2)`返回`tuple`
  + `s.rpartition(sep)`：从右的第一个`sep`开始将`s`切为`(str1,sep,str2)` 返回`tuple`
  + `s.lower()`：返回字母小写的字符串
  + `s.upper()`：返回字母大写的字符串
  + `s.title()`：返回所有单词首字母大写的字符串
  + `s.capitalize()`：返回首个单词首字母大写的字符串
  + `s.swapcase()`：返回字母大小写转换后的字符串
  + `s.casefold()`：返回各种字母小写的字符串
  + `s.isdigit()`：判断是否都是数字
  + `s.isalpha()`：判断是否都是字母
  + `s.isalnum()`：判断是否都是数字或字母
  + `s.islower()`：判断其中的字母是否都是小写
  + `s.isupper()`：判断其中的字母是否都是大写
  + `s.istitle()`：判断每个单词是否首字母大写
  + `s.isspace()`：判断是否都是空白符号`(include space not include empty)`
  + `s.isprintable()`：判断是否都是可打印符号`(include space and empty)`
  + `s.isidentifier()`：判断是否满足标识符定义
  + `s.center(width[,fillchar])`：给定宽度居中填充，默认为`space`
  + `s.ljust(width[,fillchar])`：给定宽度左对齐填充，默认为`space`
  + `s.rjust(width[,fillchar])`：给定宽度右对齐填充，默认为`space`
  + `s.zfill(width)`：给定宽度左填充`0`，跳过首个`+/-` 
  + `s.endswith(suffix[,start[,end]])`：判断从`start`到`end-1`是否以`suffix`结尾
  + `s.startswith(prefix[,start[,end]])`：判断从`start`到`end-1`是否以`prefix`开头
  + `s.expandtabs(N=8)`：把字符串中的 `\t`转为空格
  + `s.maketrans(inttab,outtab)`：以`in:out`创建翻译表`dict` 
  + `s.translate(table[,ignore])`：以`table`为翻译表转换字符，忽略`ignore`内的字符
  + `s.splitlines([keepends=True])`：以`\n\r`分割，若`keepends=True`则保留所有`\n\r`放至分割位置左侧符号结尾

## 布尔类型（`boolean`）

- `pass`

## 列表(`list`)

- 相关操作
  - 建立：`[elements], list()`
  - 切片：`var[start:end:step]`
  - 索引：`var[i]`
  - 删除：`del var[i]`
  - 列表推导式：`[expression for var in iter[ if condition]] | [expression[ if condition else condition] for var in iter]`
  - 列表生成式：`(expression for var in iter[ if condition]) | (expression[ if condition else condition] for var in iter)`
- 相关函数
  - `list(iter)`：将`lter`转化为`list`
  - `sorted(list, key=func, reverse=bool)`：返回按要求排序后的`list`
- 相关方法
  - `l.append(var)`：添加元素至末尾
  - `l.remove(var)`：删除元素
  - `l.insert(index,var)`：指定位置插入元素
  - `l.sort(iter, key=func, reverse=bool)`：按要求排序`list`
  - `l.pop(index=-1)`：删除并返回`index`所在元素（如果`list`为空，会引发`KeyError`）
  - `l.extend(iter)`：将`iter`拼接至末尾
  - `l.reverse()`：反转`list`

## 元组(`tuple`)

- 相关操作
  - 建立：`(elements), tuple()`
  - 切片：`var[start:end:step]`
  - 索引：`var[i]`
- 相关函数
  - `tuple(iter)`：将`iter`转化为`tuple`
- 相关方法
  - `pass`

## 字典(`dictionary`)

- 相关操作
  - 建立：`{keys:values}, dict()`
  - 索引：`var[key]`
  - 删除：`del var[key]`
  - 字典推导式：`{key:value for key, value in zip(keys, values)[ if condition]} | {key:value[ if condition else condition] for key, value in zip(keys, values)}`
- 相关函数
  - `dict(iter(keys, values))`：将多个`(keys, values)`组成的可迭代对象转化为`tuple`
- 相关方法
  - `d.get(key)`：返回`d[key]`，若不存在则返回`None`
  - `d.setdefault(key,default=None)`：返回`d[key]`，若不存在则返回`default`
  - `d.keys()`：返回`keys`的`dict_keys`类型列表
  - `d.values()`：返回`values`的`dict_values`类型列表
  - `d.items()`：返回`(key,value)s`的`dict_items`类型列表
  - `d.pop(key)`：删除`d[key]`
  - `d.popitem()`：删除某个键值对并返回`(key:value)`
  - `d.copy()`：返回`dict`的浅拷贝
  - `d.clear()`：清除所有键值对
  - `d.fromkeys(iter,default=None)`：返回以`iter`为`keys`，`default`为默认`values`的字典
  - `d.update(dict)`：将新`dict`的键值对更新至原来的`dict`

## 集合(`set`)

- 相关操作
  - 建立：`{elements}, set(), frozenset()`
  - 子集：`A > B`
  - 并集：`A | B`
  - 交集：`A & B`
  - 差集：`A - B`
  - 对称差：`A ^ B`
  - 集合推导式：`{expression for var in iter[ if condition]} | {expression[ if condition else condition] for var in iter}`
- 相关函数
  - `set(iter)`：将`iter`转化为`set`
  - `frozenset(iter)`：将`iter`转化为不可变`set`
- 相关方法
  - `s.add(var)`：向`set`中添加元素
  - `s.pop()`：删除并返回任意一个元素（如果`set`为空，会引发`KeyError`）
  - `s.remove(var)`：删除`set`中的`var`（如果不存在，会引发`KeyError`）
  - `s.discard(var)`：删除`set`中的`var`（如果不存在，则不执行任何操作）
  - `s.intersection(set)`：将两个`set`的交集作为一个新`set`返回
  - `s.union(set)`：将`set`的并集作为一个新`set`返回
  - `s.difference(set)`：将两个或多个`set`的差集作为一个新`set`返回　
  - `s.symmetric_difference(set)`：将两个`set`的对称差作为一个新`set`返回
  - `s.update(set)`：用自己和另一个的并集来更新这个`set` 
  - `s.intersection_update(set)`：用自己和另一个的交集来更新这个`set`
  - `s.difference_update(set)`：从这个`set`中删除另一个`set`的所有元素
  - `s.symmetric_difference_update(set)`：用自己和另一个的对称差来更新这个`set`
  - `s.isdisjoint(set)`：如果两个`set`交集为空集，返回`True`
  - `s.issubset(set)`：如果另一个集`set`包含这个`set`，返回`True`
  - `s.issuperset(set)`：如果这个`set`包含另一个`set`，返回`True`

## 可迭代对象(`iterable`)

+ 包含
  + 字符串`str`，列表`list`，元组`tuple`，集合`set`

+ 相关操作
  + 索引：`iter[i](except set)`：允许正向索引`0,1,2,...`和反向索引`-1,-2,-3,...`
  + 切片：`iter[start:end:step](except set)`：获得子列
  + 判断：`var in iter`
+ 相关函数
  + `len(iter)`：返回`iter`的长度
  + `max(iter)`：返回`iter`的最大值
  + `min(iter)`：返回`iter`的最小值
  + `sum(iter)`：返回`iter(except str)`元素总和
  + `zip(iters)`：平行遍历多个`iter`
  + `map(func, iter)`：利用`func`并行处理`iter`
  + `all(iter)`：如果`iter`中的所有元素都是`True`（或者`iter`为空），则返回`True`
  + `any(iter)`：如果`iter`中的任意一个元素是`True`，则返回`True`；如果`iter`为空，则返回`False`
  + `enumerate(iter)`：返回一个枚举对象，其中包含了`iter`中所有元素对应的`(index, value)`
  + `filter(func, iter)`：过滤序列
+ 相关方法
  + `iter.count(sub)`：返回`sub`在`iter(except set/dict)`中出现的次数
  + `iter.index(var)`：返回`var`在`iter(except set)`中第一次出现的位置
  + `iter.copy()`：返回`iter(except str/tuple)`的浅拷贝
  + `iter.clear()`：清空`iter(except str/tuple)`

# 文件读写

+ 读写模式

  | 模式  | 打开方式           | 指针位置                                   |
  | ----- | ------------------ | ------------------------------------------ |
  | `r`   | 只读（默认模式）   | 指针在文件开头，文件不存在则报错           |
  | `rb`  | 二进制格式只读     | 指针在文件开头，文件不存在则报错           |
  | `r+`  | 可读可写           | 指针在文件开头，文件不存在则报错           |
  | `rb+` | 二进制格式可读可写 | 指针在文件开头，文件不存在则报错           |
  | `w`   | 只写               | 文件存在，则覆盖；不存在，则创建           |
  | `wb`  | 二进制格式只写     | 文件存在，则覆盖；不存在，则创建           |
  | `w+`  | 可读可写           | 文件存在，则覆盖；不存在，则创建           |
  | `wb+` | 二进制格式可读可写 | 文件存在，则覆盖；不存在，则创建           |
  | `a`   | 追加               | 文件存在，指针在文件尾追加；不存在，则创建 |
  | `ab`  | 二进制格式追加     | 文件存在，指针在文件尾追加；不存在，则创建 |
  | `a+`  | 可读可写           | 文件存在，指针在文件尾追加；不存在，则创建 |
  | `ab+` | 二进制格式可读可写 | 文件存在，指针在文件尾追加；不存在，则创建 |

+ 相关字段

  | 字段         | 含义         |
  | ------------ | ------------ |
  | `f.name`     | 文件名       |
  | `f.encoding` | 编码格式     |
  | `f.mode`     | 读写模式     |
  | `f.errors`   | 错误处理模式 |
  | `f.closed`   | 关闭状态     |

+ 相关函数

  + `open(file, mode, buffering, encoding, errors, newline, closefd)`：打开文件，若不存在抛出`IOError`
  + `file(name, mode=None, buffering=None)`：打开文件，`buffering`如果为`0`表示不缓冲,如果为`1`表示进行行缓冲，大于`1`为缓冲区大小

+ 相关方法

  + `f.close()`：关闭`<_io.TextIOWrapper>`文件对象
  + `f.read(n)`：若无参数则默认一次性读取文件的全部内容，否则每次最多读取`n`个字节的内容
  + `f.readline(linit)`：每次读取一行内容
  + `f.readlines(hint)`：一次读取所有内容并按行返回`list`
  + `f.readable()`：返回i文件是否可读
  + `f.write(s)`：向文件写入数据
  + `f.writelines(lines)`：传入一个字符串列表参数，将该字符串列表写入文件
  + `f.writable()`：返回i文件是否可写
  + `f.flush()`：修改文件内容后，提交更新
  + `f.tell()`：返回文件指针位置
  + `f.seek(offset, whence)`：当`whence = 0`把文件指针从文件头部向后（不能向前）移动`offset`个字节；当`whence = 1`把文件指针从当前位置向后（不能向前）移动`offset`个字节；当`whence = 0`把文件指针从文件尾部向前移动`offset`个字节

# 错误调试

+ 异常处理

  ```python
  try:
      pass
  except[ Exception as e]:
      [raise[ e]]
      pass
  finally:
      pass
  ```

# 类、对象与实例

+ 类的构造、继承与多态，实例的创建

  ```python
  class Name(object):
      self.property = value # 外部可访问
      self._property = value # 外部可访问，但尽量别随意访问
      self.__property = value # 私有变量，不可外部访问
      # self.__property__ 不可这样命名，这一类是特殊变量
      def __init__(self, *args):
          self.args = args
          pass
      def method(self, *args):
          pass
  obj = Name(args)
  ```
  
  
  
+ 预定义函数

  + `__init__(self, *args)`：在`instance`被创建时自动调用该初始化函数
  + `__lt__(self, other)`：通过`A < B`调用
  + `__le__(self, other)`：通过`A <= B`调用
  + `__gt__(self, other)`：通过`A > B`调用
  + `__ge__(self, other)`：通过`A >= B`调用
  + `__eq__(self, other)`：通过`A == B`调用
  + `__ne__(self, other)`：通过`A != B`调用
  + `__add__(self, other)`：通过`A + B`调用，反向`__radd__`，复合赋值`__iadd__`
  + `__sub__(self, other)`：通过`A - B`调用，反向`__rsub__`，复合赋值`__isub__`
  + `__mul__(self, other)`：通过`A * B`调用，反向`__rmul__`，复合赋值`__imul__`
  + `__truediv__(self), other`：通过`A / B`调用，反向`__rtruediv__`，复合赋值`__itruediv__`
  + `__floordiv__(self, other)`：通过`A // B`调用，反向`__rfloordiv__`，复合赋值`__ifloordiv__`
  + `__mod__(self, other)`：通过`A % B`调用，反向`__rmod__`，复合赋值`__imod__`
  + `__pow__(self, power)`：通过`A ** power `调用，反向`__rpow__`，复合赋值`__ipow__`
  + `__and__(self, other)`：通过`A & B`调用，反向`__rand__`，复合赋值`__iand__`
  + `__or__(self, other)`：通过`A | B`调用，反向`__ror__`，复合赋值`__ior__`
  + `__xor__(self, other)`：通过`A ^ B`调用，反向`__rxor__`，复合赋值`__ixor__`
  + `__lshift__(self, other)`：通过`A << B`调用，反向`__rlshift__`，复合赋值`__ilshift__`
  + `__rshift__(self, other)`：通过`A >> B`调用，反向`__rrshift__`，复合赋值`__irshift__`
  + `__neg__(self)`：通过`-A`调用
  + `__pos__(self)`：通过`+A`调用
  + `__invert__(self)`：通过`~A`调用
  + `__contains__(self, item)`：通过`item in A`调用
  + `__getitem__(self, item)`：通过`A[item|slice]`调用
  + `__setitem__(self, key, value)`：通过`A[item|slice] = value`调用
  + `__delitem__(self, item)`：通过`del A[item|slice]`调用
  + `__call__(self, *args, **kwargs)`：通过`A(*args, **kwargs)`调用
  + `__func__(self)`：通过`func()`调用，`func`的可选函数有：`abs, bool, ceil, float, floor, hash, hex, int, len, oct, repr, str`
  
+ 装饰器

  + `@classmethod`：使得方法不需要实例化也能直接使用
  + `@property`：使得`__property`能够被读写，被修饰的函数名需要与`property`同名

# 线程与进程

+ `multiprocessing`
  + 相关字段
    + `p.daemon`：默认值为`False`，如果设为`True`，代表`p`为后台运行的守护进程，当父进程终止时，`p`也随之终止，并且设定为`True`后，`p`不能创建自己的新进程，必须在`start()`之前设置
    + `p.name`：进程的名称
    + `p.pid`：进程的`pid`
    + `p.exitcode`：进程在运行时为`None`、如果为`–N`，表示被信号`N`结束
    + `p.authkey`：进程的身份验证键,默认是由`os.urandom()`随机生成的32字符的字符串。这个键的用途是为涉及网络连接的底层进程间通信提供安全性，这类连接只有在具有相同的身份验证键时才能成功
  + 相关函数
    + `multiprocessing.Process(group=None, target=None, name=None, *args, **kwargs)`：由该类实例化得到的对象，表示一个子进程中的任务（尚未启动），需要使用关键字的方式来指定参数；`group`参数未使用，值始终为`None`，`target`表示调用对象，即子进程要执行的任务；`args`表示调用对象的位置参数元组；`kwargs`表示调用对象的字典；`name`为子进程的名称
    + `multiprocessing.Pool(numprocess=None, initializer=None, initargs=None)`：创建进程池`numprocess`:要创建的进程数，如果省略，将默认使用`cpu_count()`的值；`initializer`：是每个工作进程启动时要执行的可调用对象，默认为`None`；`initargs`：是要传给`initializer`的参数组
  + 相关方法
    + `p.start()`：启动进程，并调用该子进程中的`p.run()`
    + `p.run()`：进程启动时运行的方法，正是它去调用`target`指定的函数，自定义的类中要实现该方法
    + `p.terminate()`：强制终止进程，不会进行任何清理操作，如果创建了子进程，该子进程就成了僵尸进程，如果保存了一个锁那么也将不会被释放，进而导致死锁
    + `p.is_alive()`：如果进程仍在运行，返回`True`
    + `p.join(timeout=None)`：主线程等待p终止，`timeout`是可选的超时时间，需要强调的是，`p.join()`只能`join`住`start`开启的进程，而不能`join`住`run`开启的进程
    + `pool.apply(func, *args, **kwargs)`：在一个池工作进程中执行`func(*args,**kwargs)`然后返回结果。需要强调的是：此操作并不会在所有池工作进程中并执行`func`函数。如果要通过不同参数并发地执行`func`函数，必须从不同线程调用`pool.apply()`函数或者使用`pool.apply_async()`
    + `pool.apply_async(func, *args, **kwargs)`：在一个池工作进程中执行`func(*args,**kwargs)`然后返回结果。此方法的结果是`AsyncResult`类的实例，`callback`是可调用对象，接收输入参数。当`func`的结果变为可用时，将理解传递给`callback`。`callback`禁止执行任何阻塞操作，否则将接收其他异步操作中的结果
    + `pool.close()`：关闭进程池，防止进一步操作。如果所有操作持续挂起，它们将在工作进程终止前完成
    + `pool.join()`：等待所有工作进程退出，此方法只能在`close()`或`teminate()`之后调用
+ `subprocess`
  + 相关字段
    + `sp.returncode`：可通信的子进程的`main`返回值代码
  + 相关函数
    + `subprocess.call(command_list)`
    + `subprocess.Popen(command_list, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)`：返回可通信的子进程
  + 相关方法
    + `sp.communicate(args_str)`：与可通信的子进程进行通信，返回`stdout`和`stderr`
+ `threading`
  + 相关函数
    + pass
    + `threading.Thread(target=func, name=None)`：创建线程
    + `threading.Lock()`：返回`Lock`对象
  + 相关方法
    + pass
    + `t.start()`：启动线程
    + `t.join()`：等待线程执行完，需要在`start()`被调用后才能调用
    + `lock.acquire()`：若`lock`被其他线程`acquire`了就会暂停直至进程完成释放`lock`
    + `lock.release()`：释放`lock`

# 常用内置函数

- `abs(x)`：返回`x`的绝对值
- `callable(object)`：返回函数是否可调用
- `dir(object)`：不带参数时，返回当前范围内的变量、方法和定义的类型列表；带参数时，返回参数的属性、方法列表。如果参数包含方法`__dir__()`，该方法将被调用。如果参数不包含`__dir__()`，该方法将最大限度地收集参数信息
- `execfile(filename, globalsNone, locals=None)`：执行文件
- `getattr(object, name, default=None)`：返回一个对象属性值，如果不提供该参数，在没有对应属性时将触发 `AttributeError`
- `hasattr(object, name)`：判断对象是否包含对应的属性
- `hash(str)`：返回`str`的哈希值，为整数
- `help(args)`：`args`可以是整个库，可以是函数、字段，可以是类和对象
- `id(object)`：获取`object`的内存地址
- `input(prompt)`：输出`prompt`提示语并获得字符串输入值
- `isinstance(object, classinfo)`：如果`object`的类型与`classinfo`相同则返回`True`，否则返回`False`，会认为子类是一种父类类型，考虑继承关系
- `issubclass(class, classinfo)`：判断`class`是否是`classinfo`的子类
- `locals()`：以字典类型返回当前位置的全部局部变量
- `next(iterator, default=None)`：返回迭代器的下一个项目，如果不设置`default`又没有下一个元素则会触发 `StopIteration`异常
- `pow(x, y)`：返回`x^y`，等价于`x**y`
- `print(values, sep=' ', end='\n', file=None, flush=None)`：输出字符串
- `range(start=0, stop, step=1)`：返回整数等差数列生成器
- `repr(object)`：将对象转化为供解释器读取的形式，返回一个对象的`string`格式
- `slice(start=0, stop, step=1)`：返回切片对象
- `super()`：调用父类(超类)
- `type(object)`：返回`object`的类型
- `vars(object)`：返回`object`的属性和属性值的字典对象，如果没有参数，就打印当前调用位置的属性和属性值

# 常用库

## `os`

| 函数                           | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `os.getcwd()`                  | 返回当前工作目录                                             |
| `os.listdir(path)`             | 返回`path`指定的文件夹包含的文件或文件夹的名字的列表         |
| `os.makedirs(path[, mode])`    | 递归文件夹创建函数。像`mkdir()`, 创建的文件夹允许包含子文件夹 |
| `os.mkdir(path[, mode])`       | 以`mode`创建一个名为`path`的文件夹.默认的`mode`是`0777`(八进制) |
| `os.open(file, flags[, mode])` | 打开一个文件，并且设置需要的打开选项，`mode`参数是可选的     |
| `os.remove(path)`              | 删除路径为`path`的文件。如果是文件夹，将抛出`OSError`        |
| `os.rmdir(path)`               | 删除`path`指定的空目录，如果目录非空，则抛出一个`OSError`    |
| `os.removedirs(path)`          | 递归删除目录                                                 |
| `os.rename(src, dst)`          | 重命名文件或目录，从`src`到`dst`                             |
| `os.renames(old, new)`         | 递归地对目录进行更名，也可以对文件进行更名                   |
| `os.stat(path)`                | 获取`path`指定的路径的信息                                   |
| `os.system(command)`           | 基于当前目录执行`shell`命令，并返回命令的执行结果            |

## `os.path`

| 函数                                  | 说明                                            |
| ------------------------------------- | ----------------------------------------------- |
| `os.path.abspath(path)`               | 返回绝对路径                                    |
| `os.path.basename(path)`              | 返回文件名                                      |
| `os.path.dirname(path)`               | 返回文件路径                                    |
| `os.path.exists(path)`                | 路径存在则返回`True`,路径损坏返回`False`        |
| `os.path.join(path1[, path2[, ...]])` | 把目录和文件名合成一个路径                      |
| `os.path.isabs(path)`                 | 判断是否为绝对路径                              |
| `os.path.isfile(path)`                | 判断路径是否为文件                              |
| `os.path.isdir(path)`                 | 判断路径是否为目录                              |
| `os.path.split(path)`                 | 把路径分割成`dirname`和`basename`，返回一个元组 |
| `os.path.splitext(path)`              | 分割路径，返回路径名和文件扩展名的元组          |
| `os.path.samefile(path1, path2)`      | 判断目录或文件是否相同                          |
| `os.path.sameopenfile(fp1, fp2)`      | 判断fp1和fp2是否指向同一文件                    |
| `os.path.getatime(path)`              | 返回最近访问时间（浮点型秒数）                  |
| `os.path.getmtime(path)`              | 返回最近文件修改时间                            |
| `os.path.getctime(path)`              | 返回文件创建时间                                |
| `os.path.getsize(path)`               | 返回文件大小，如果文件不存在就返回错误          |

## `re`

+ 匹配字符

  | 字符  | 说明                                    | 字符    | 说明                        |
  | ----- | --------------------------------------- | ------- | --------------------------- |
  | `\d`  | 匹配一个数字                            | `\w`    | 匹配字母数字下划线          |
  | `\D`  | 与`\d`相反                              | `\W`    | 与`\w`相反                  |
  | `\s`  | 匹配任何不可见字符,等价于`[\f\n\r\t\v]` | `A|B`   | 匹配`A`或`B`                |
  | `\S`  | 与`\s`相反                              | `()`    | 用于分组或分隔              |
  | `+`   | 匹配至少一个字符                        | `*`     | 匹配任意个字符（包括`0`个） |
  | `?`   | 匹配`0`个或`1`个字符，禁止贪婪匹配      | `.`     | 匹配任意字符                |
  | `{n}` | 匹配`n`个字符                           | `{n,m}` | 匹配`n-m`个字符             |
  | `^`   | 匹配开头                                | `$`     | 匹配结尾                    |

+ 相关字段

  | flag                 | 说明                                                         |
  | -------------------- | ------------------------------------------------------------ |
  | `re.A|re.ASCII`      | 使用`ASCII`字符集进行匹配(不常用)                            |
  | `re.I|re.IGNORECASE` | 忽略大小写匹配                                               |
  | `re.L|re.LOCALE`     | 使用当前预定字符类`\w\W\b\B\s\S`取决于当前区域设定(不常用)   |
  | `re.U|re.UNICODE`    | 使用Unicode字符类`\w\W\b\B\s\S\d\D`取决于`unicode`定义的字符属性(不常用) |
  | `re.M|re.MULTILINE`  | 多行匹配，使`^、$`可以在每一行中都进行匹配                   |
  | `re.S|re.DOTALL`     | 使 `.` 可以匹配换行符`\r、\n`                                |
  | `re.X|re.VERBOSE`    | 去掉正则表达式中的所有空格符(不常用)                         |

+ 相关函数

  | 方法                                               | 说明                                                         |
  | -------------------------------------------------- | ------------------------------------------------------------ |
  | `re.compile(pattern, flags=0)`                     | 返回一个`re.Pattern`对象                                     |
  | `re.findall(pattern,string,flags=0)`               | 按照规则匹配整个字符串，返回匹配结果的列表                   |
  | `re.finditer(pattern, string, flags=0)`            | 按照规则匹配整个字符串，返回匹配结果的迭代器                 |
  | `re.search(pattern, string, flags=0)`              | 只返回第一个匹配到的结果，成功返回`re.Match`对象，否则返回`None` |
  | `re.match(pattern, string, flags=0)`               | 从开始处进行匹配，成功返回`re.Match`对象，否则返回`None`     |
  | `re.sub(pattern,repl,string,count=0,flags=0)`      | `sub`按照给定的规则将`string`字符串中的相应的片段替换为`repl`，`count` 为最多替换的次数，`count=0`默认为全部替换，返回替换后的字符串 |
  | `re.subn(pattern, repl, string, count=0, flags=0)` | 与`sub`相似，返回一个元组，下标`0`为替换后的字符串，下标`1`成功替换的次数 |
  | `re.split(pattern, string, maxsplit=0, flags=0)`   | 通过给定规则，将`string`进行切割，`maxsplit`为最多切割次数，`maxsplit=0`默认全部切割，返回列表 |
  | `<re.Match>.group(N=0)`                            | 返回第N组括号匹配的字符，当`N=0`返回匹配的字符串             |
  | `<re.Match>.groups()`                              | 以`tuple`格式返回所有括号匹配的字符                          |

## `math`

+ 相关字段
  + `math.e`：自然对数的底数`e = 2.718281828459045`
  + `math.pi`：圆周率`pi = 3.141592653589793`
+ 相关函数
  + `math.exp(x)`：返回 $e^x$
  + `math.expm1(x)`：返回 $e^x-1$
  + `math.ceil(x)`：取大于等于`x`的最小的整数值
  + `math.floor(x)`：取小于等于`x`的最大的整数值
  + `math.copysign(x, y)`：把`y`的正负号加到`x`前面
  + `math.fabs(x)`：返回`x`的绝对值
  + `math.frexp(x)`：返回`(m, i)`，满足 $x=m·2^i$
  + `math.ldexp(x, i)`：返回 $x·2^i$
  + `math.trunc(x)`：返回`x`的整数部分
  + `math.factorial(x)`：取`x`的阶乘的值，`x`为负数或非数值则报错
  + `math.gcd(x, y)`：返回`x`和`y`的最大公约数
  + `math.hypot(x, y)`：返回以`x`和`y`为直角边的斜边长
  + `math.isinf(x)`：如果`x`是正无穷大或负无穷大，则返回`True`，否则返回`False`
  + `math.isfinite(x)`：如果`x`不是正无穷大或负无穷大，则返回`True`，否则返回`False`
  + `math.isnan(x)`：如果`x`不是数字，返回`True`，否则返回`False`
  + `math.log(x, base=math.e)`：返回`x`的自然对数，默认以`e`为基数
  + `math.log1p(x)`：返回 $\ln{(1+x)}$
  + `math.log2(x)`：返回`x`的以`2`为底的对数
  + `math.log10(x)`：返回`x`的以`10`为底的对数
  + `math.modf(x)`：返回由`x`的小数部分和整数部分组成的元组
  + `math.fmod(x, y)`：返回`x`对`y`取模后的数，`x、y`可以是浮点数
  + `math.fsum(iterable)`：返回无损精度的和
  + `math.sqrt(x)`：求`x`的平方根
  + `math.pow(x)`：返回`x`的`y`次方
  + `math.degrees(x)`：把`x`从弧度转换成角度
  + `math.radians(x)`：把角度`x`转换成弧度
  + `math.sin(x)`：求`x`(`x`为弧度)的正弦值
  + `math.cos(x)`：求`x`(`x`为弧度)的余弦值
  + `math.tan(x)`：求`x`(`x`为弧度)的正切值
  + `math.asin(x)`：求`x(-1<=x<=1)`的反正弦值
  + `math.acos(x)`：求`x(-1<=x<=1)`的反余弦值
  + `math.atan(x)`：求`x`的反正切值
  + `math.atan2(y, x)`：求`y/x`的反正切值
  + `math.sinh(x)`：返回`x`的双曲正弦函数
  + `math.cosh(x)`：返回`x`的双曲余弦函数
  + `math.tanh(x)`：返回`x`的双曲正切函数
  + `math.asinh(x)`：返回`x`的反双曲正弦函数
  + `math.acosh(x)`：返回`x`的反双曲余弦函数
  + `math.atanh(x)`：返回`x`的反双曲正切函数
  + `math.erf(x)`：返回`x`的误差函数
  + `math.erfc(x)`：返回`x`的余误差函数
  + `math.gamma(x)`：返回`x`的伽玛函数
  + ` math.lgamma(x)`：返回`x`的绝对值的自然对数的伽玛函数

## `random`

+ 相关函数

  + `random.random()`：用于生成一个`0.0<=n<1.0`的随机浮点数
+ `random.uniform(a,b) `：用于生成一个指定范围内的随机浮点数
  + `random.randint(a,b) `：用于生成一个指定范围内的随机整数
  + `random.randrange([start], stop[, step])`：从指定范围内，按指定基数递增的集合中获取一个随机数
  + `random.choice(sequence)`：从`list、dict、str`中获取一个随机元素
  + `random.shuffle(list)`：将序列中的元素顺序打乱
  + `random.sample(sequence, k)`：从从list序列中，随机获取k个元素，生成一个新序列，不改变原来序列
  + `random.getrandbits(k)`：返回带有k个随机位的`Python`整数
  + `random.betavariate(alpha, beta)`：`beta`分布。返回的结果在0~1之间
  + `random.expovariate(lambda)`：随机生成符合指数分布的随机数
  + `random.gammavariate(alpha, beta)`：`Gamma`分布
  + `random.gauss(mu, sigma)`：随机生成符合`Gauss`分布的随机数
  + `random.lognormvariate(mu, sigma)`：对数正态分布
  + `random.normalvariate(mu, sigma)`：正态分布
  + `random.paretovariate(alpha)`：帕累托分布
  + `random.triangular(low, high, mode)`：返回`low<=N<=high`的三角形分布的随机数。`mode`指明众数出现位置
  + `random.vonmisesvariate(mu, kappa)`：`Kappa`分布
  + `ramdom.weibullvariate(alpha, beta)`：`Weibull`分布, 比例参数为`alpha`, 形状参数为`beta`

## `datetime.datetime`

+ 构造函数

  + `datetime.datetime.today()`：使用今天的日期构造对象，返回今天的日期
  + `datetime.datetime.fromordinal(n)`：使用日期序数构造对象，传入参数为一个整数序数，代表从公元1年1月1日开始的序数，序数每增加1代表增加1天，返回最终计算出的日期
  + `datetime.datetime.fromtimestamp(t)`：使用时间戳构造对象，传入参数t为一个时间戳，返回对应的日期
  + `datetime.datetime.now()`：使用当前日期和时间构造对象，返回当前的日期和时间
  + `datetime.datetime.utcnow()`：使用当前日期和时间构造对象，返回当前的UTC（时间标准时间）日期和时间
  + `datetime.datetime.combine(date,time)`：使用`date`和`time`构造对象，传入参数为1个`date`对象和1个`time`对象，返回计算出的日期

+ 相关方法

  + `t.timetuple()`：返回日期对应的`time.struct_time`对象，格式为`time.struct_time(tm_year=1, tm_mon=1, tm_mday=2, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=2, tm_isdst=-1)`
  + `t.utctimetuple()`：与`timetuple()`相似，返回日期时间对应的UTC（时间标准时间）`time.struct_time`对象
  + `t.toordinal`：相当于`fromordinal(n)`的逆过程，返回值即为`fromordinal(n)`中的日期序数`n`
  + `t.weekday()`：返回该日期对应星期几，用`[0,6]`代表星期一到星期日
  + `t.isoweekend()`：作用同`weekday()`，用`[1,7]`代表星期一到星期日
  + `t.isocalendar()`：返回一个三元组，格式为`(year,week_number,weekday)`
  + `t.isoformat()`：返回标准日期格式`YYYY-MM-DD`
  + `t.ctime()`：返回格式为`WWW MMM D HH:MM:SS YYYY`
  + `t.strftime(format)`：把日期按照`format`指定的格式进行格式化
  + `t.strptime(date_string,format)`：将字符串格式转换为日期格式
  + `t.replace(year,month,day)`：传入参数为`year, month, day`，返回对应的新日期
  + `t.astimezone()`：返回的格式中加入时区信息，格式为`YYYY-MM-DD HH:MM:SS+08:00`
  + `t.date()`：返回`date`部分
  + `t.time()`：返回`time`部分，`tzinfo`设置为`None`。（另有`timetz()`方法返回有相同`tzinfo`的`time()`）

+ 格式控制

  | 格式控制符 | 说明                                      |
  | ---------- | ----------------------------------------- |
  | `%y`       | 两位数的年份表示`(00-99)`                 |
  | `%Y`       | 四位数的年份表示`(000-9999)`              |
  | `%m`       | 月份`(01-12)`                             |
  | `%d`       | 月内中的一天`(0-31)`                      |
  | `%H`       | 24小时制小时数`(0-23)`                    |
  | `%I`       | 12小时制小时数`(01-12)`                   |
  | `%M`       | 分钟数`(00-59)`                           |
  | `%S`       | 秒`(00-59)`                               |
  | `%a`       | 本地简化星期名称                          |
  | `%A`       | 本地完整星期名称                          |
  | `%b`       | 本地简化的月份名称                        |
  | `%B`       | 本地完整的月份名称                        |
  | `%c`       | 本地相应的日期表示和时间表示              |
  | `%j`       | 年内的一天`(001-366)`                     |
  | `%p`       | 本地`A.M.`或`P.M.`的等价符                |
  | `%U`       | 一年中的星期数`(00-53)`星期天为星期的开始 |
  | `%w`       | 星期`(0-6)`，星期天为星期的开始           |
  | `%W`       | 一年中的星期数`(00-53)`星期一为星期的开始 |
  | `%x`       | 本地相应的日期表示                        |
  | `%X`       | 本地相应的时间表示                        |
  | `%Z`       | 当前时区的名称                            |
  | `%%`       | `%`号本身                                 |

## `numpy`

+ 数组创建
  + `np.array(iter)`：传入多维列表元组等`iter`创建`numpy.ndarray`对象
  + `np.matrix(iter)`：传入多维列表元组等`iter`创建`numpy.matrix`对象，可以直接进行矩阵的加减乘除运算
  + `np.arange(start=0, stop, step=1)`：创建从`start`到`stop(not include stop)`的等间距元素数组
  + `np.random.random(shape)`：创建元素范围在`[0,1)`之间的数组
  + `np.linspace(start, stop, num, endpoint=False)`：创建`num`个从`start`到`stop`的等间距元素数组
  + `np.zeros(shape)`：创建元素全为`0`的数组
  + `np.ones(shape)`：创建元素全为`1`的数组
  + `np.eye(N)`：创建`N × N`的单位矩阵
  + `np.full(shape, fill_value)`：创建元素全为`fill_value`的数组
  + `np.transpose(a)`：转置矩阵(而非向量)
+ 相关操作
  + 索引
    + `a[i][j]`：普通索引
    + `a[[elements], [elements]]`：平行遍历参数获得索引下标进行索引，并返回索引值数组，允许混合切片
    + `a[[elements], [elements]] += num`：将所有索引值加上`num`，类似地有加减乘除指数等运算
  + 切片
    + `a[start=None:stop=None:step=1, start=None:stop=None:step=1]`：允许多维度切片
  + 运算
    + `a * b`：数组对应元素相乘，类似地有加减乘除指数等运算，若`shape`不同但列数相同会自动扩增
    + `a > num`：返回布尔数组
    + `a[a > num]`：利用布尔数组筛选元素，返回符合条件的元素组成的数组
    + `a.T`：数组`a`的转置
+ 相关方法
  + `a.dot(b)`：返回 $AB$
  + `a.inner(b)`：返回 $AB^T$
  + `a.outer(b)`：将`a`展开为一维列向量，`b`展开为一维行向量进行相乘
  + `a.sum(axis=None)`：默认全部求和，`axis=0`则列求和，`axis=1`则行求和，返回求和结果数组
  + `a.cumsum(axis=None)`：返回由前`n`项和组成的数组
  + `a.min(axis=None)`：求数组内的最小值
  + `a.max(axis=None)`：求数组内的最大值
  + `a.mean(axis=None)`：求平均值
  + `a.reshape(shape)`：将`a`的元素重新排列为`shape`
+ 相关函数
  + `np.vstack(arr1, arr2)`：垂直合并矩阵
  + `np.hstack(arr1, arr2)`：水平合并矩阵
  + `np.concatenate(arr_tuple,axis=0)`：当`axis = 0`时为垂直合并，`axis = 1`时为水平合并
  + `np.mean(a, axis=None)`：求平均值，等效于`a.mean(axis=None)`
  + `np.median(a, axis=None)`：求中位数
  + `np.multiply(a, b)`：矩阵对应元素相乘，等效于`a * b`
  + `np.min(a, axis=None)`：求数组内的最小值，等效于`a.min(axis=None)`
  + `np.max(a, axis=None)`：求数组内的最大值，等效于`a.max(axis=None)`
  + `np.cumsum(a, axis=None)`：返回由前`n`项和组成的数组，等效于`a.cumsum(axis=None)`
  + `np.dot(a, b)`：返回 $AB$，等效于`a.dot(b)`
  + `np.inner(a, b)`：返回 $AB^T$，等效于`a.inner(b)`
  + `np.outer(a, b)`：将`a`展开为一维列向量，`b`展开为一维行向量进行相乘，等效于`a.outer(b)`
  + `np.where(condition)`：传入布尔数组，返回符合条件元素数组和数据类型的元组，可用`0`索引得到数组
  + `np.tile(a, shape)`：以`a`整体为元素，创建`shape`形状的数组
  + `np.prod(a, axis=None)`：返回各元素乘积的数组
  + `np.reshape(a, shape)`：将`a`的元素重新排列为`shape`，等效于`a.reshape(shape)`
  + `np.transpose(a)`：返回数组`a`的转置
  + `np.linalg.solve(A,b)`：返回方程`Ax=b`的解
  + `np.random.randn(d0,d1,d2,...)`：返回一个或一组服从标准正态分布的随机样本值

## `matplotlib`

+ `matplotlib.pyplot`

  + `plt.plot(xdata=None, ydata, color=None, marker=None, linestyle=None, linewidth=None, label=None)`

    + `xdata`：可省略，默认为自然数列

    + `ydata`：不可省略，y轴数据

    + `color`：曲线颜色，可直接由`fmt`指定，也可以单独指定

      | character | color             |
      | --------- | ----------------- |
      | `b`       | `blue`（蓝）      |
      | `g`       | `green`（绿）     |
      | `r`       | `red`（红）       |
      | `c`       | `cyan`（蓝绿）    |
      | `m`       | `magenta`（洋红） |
      | `y`       | `yellow`（黄）    |
      | `k`       | `black`（黑）     |
      | `w`       | `white`（白）     |

    + `marker`：点型，可直接由`fmt`指定，也可以单独指定

      | character | description             |
      | --------- | ----------------------- |
      | `.`       | `point marker`          |
      | `,`       | `pixel marker`          |
      | `o`       | `circle marker`         |
      | `v`       | `triangle_down marker`  |
      | `^`       | `triangle_up marker`    |
      | `<`       | `triangle_left marker`  |
      | `>`       | `triangle_right marker` |
      | `1`       | `tri_down marker`       |
      | `2`       | `tri_up marker`         |
      | `3`       | `tri_left marker`       |
      | `4`       | `tri_right marker`      |
      | `s`       | `square marker`         |
      | `p`       | `pentagon marker`       |
      | `*`       | `star marker`           |
      | `h`       | `hexagon1 marker`       |
      | `H`       | `hexagon2 marker`       |
      | `+`       | `plus marker`           |
      | `x`       | `x marker`              |
      | `D`       | `Diamond marker`        |
      | `d`       | `thin_diamond marker`   |
      | `|`       | `vline marker`          |
      | `_`       | `hline marker`          |

    + `linestyle`：线型，可直接由`fmt`指定，也可以单独指定

      | character | description                     |
      | --------- | ------------------------------- |
      | `-`       | `olid line style`（实线）       |
      | `--`      | `dashed line style`（虚线）     |
      | `-.`      | `dash-dot line style`（点画线） |
      | `:`       | `dotted line style`（点线）     |

      

    + `linewidth`：线宽

    + `label`：图例，需要设定显示位置

  + `plt.legend(loc, fontsize, edgecolor, frameon, )`

    + `loc`：控制位置，可选参数`best, right, center [left|right], upper|lower left|right|center`
    + `fontsize`：控制文字大小，可选参数`xx-small, x-small, small, medium, large, x-large, xx-large`
    + `edgecolor`：控制边框颜色
    + `frameon`：控制是否显示边框，可选参数`True|False`

  + `plt.gca()`：获得`axes`对象

    + `ax.spines[pos]`：指向某条坐标轴的对象，可选参数`top, bottom, left, right`，有`set_color(color), set_posotion('data',0), set_visible(bool), set_linewidth(lw), set_linestyle(ls)`等方法
    + `ax.xaxis.set_ticks_position(pos)`：设定`x`轴依赖的坐标轴
    + `ax.yaxis.set_ticks_position(pos)`：设定`y`轴依赖的坐标轴
    + `ax.patch.set_facecolor(color) `：设置区域背景颜色
    + `ax.patch.set_alpha(pellucidity) `：设置区域背景颜色透明度

  + `plt.xticks(ticks, labels=None)`：在`x`轴设置标记点，可用公式给每个标志点做标志

  + `plt.yticks(ticks, labels=None)`：在`y`轴设置标记点，可用公式给每个标志点做标志

  + `plt.xlim(left, right)`：控制`x`轴范围

  + `plt.ylim(left, right)`：控制`y`轴范围

  + `plt.xlabel(name)`：为`x`轴命名

  + `plt.ylabel(name)`：为`y`轴命名

  + `plt.title(name)`：为图画的标题命名

  + `plt.figure(figsize、facecolor，edgecolor)`：返回一个`figure`对象

  + `plt.subplots(figsize)`：更改临时画板占用空间，返回一个`figure`图像和一个`axes`子图的列表

  + `plt.scatter(x, y, c=None, marker=None, cmap=None, label=None, norm=None,
  vmin=None, vmax=None, alpha=None, linewidths=None,edgecolors=None)`画散点图
  
+ `plt.savefig(path, dpi=None)`：保存图画
  
  + `plt.rcParams[property]`：默认属性，可选参数`figure.figsize=(6.0,4.0), image.interpolation, image.cmap, savefig.dpi,figure.dpi=100, font.sans-serif`
  + `plt.show()`：显示图画

## `pandas`

+ `Series`
  + `pandas.Series(data, index=None, dtype=None, copy=False)`：创建`Series`对象
  + `s[slice/iter/index]`：切片/多索引/索引
  + `s.property`：等价于`s[property]`
  + `s.axes`：返回`index`列表
  + `s.values`：返回`ndarray`数据
  + `s.size`：返回数据总数
  + `s.ndim`：返回数据维度
  + `s.empty`：返回数据是否为空
  + `s.dtype`：返回数据类型
  + `s.head(n=5)`：返回前`n`项
  + `s.tail(n=5)`：返回后`n`项
  + `s.append(s)`：添加新`index`
  + `s.map(func)`：遍历列数据代入函数
  + `s.apply(func)`：遍历列数据代入函数
+ `DataFrame`
  + `pandas.DataFrame(data, index=None, columns=None, dtype=None, copy=False)`：创建`DataFrame`对象
  + `df[slices/condition/property]`：切片/列索引
  + `df.apply(func)`：遍历指定列代入函数
  + `df.loc[slices/iters/item]`：返回指定`item(s)`
  + `df.iloc[slices/iters/item]`：以数字索引返回指定`item(s)`
  + `df.at[item, property]`：返回指定数据
  + `df.iat[item, property]`：以数字索引返回指定数据
  + `df.property`：等价于`df[property]`
  + `df.T`：数据的转置
  + `df.axes`：返回`index`和`columns`列表
  + `df.values`：返回`ndarray`数据
  + `df.size`：返回数据总数
  + `df.shape`：返回形状
  + `df.ndim`：返回数据维度
  + `df.empty`：返回数据是否为空
  + `df.dtype`：返回数据类型
  + `df.head(n=5)`：返回前`n`项
  + `df.tail(n=5)`：返回后`n`项
  + `df.describe()`：返回列统计信息
  + `df.copy()`：返回被复制的数据
  + `df.append(df)`：添加新`item(s)`，也可直接赋值添加
  + `df.drop(item)`：删除`item`
  + `df.sort_index(axis=0, ascending=True)`：当`axis=0`排序`index`，当`axis=1`排序`columns`，默认升序排列
  + `df.sort_values(by, ascending=True)`：指定列排序，默认升序
  + `df[property].isin(iter)`：返回符合条件的数据
  + `df[property].map(func)`：遍历指定列代入函数
  + `df[property].apply(func)`：遍历指定列代入函数
  + `df.sum(axis=None)`：求和
  + `df.cumsum()`：累计求和
  + `df.mean()`：求均值
  + `df.max(axis=None)`：求最大值
  + `df.cummax()`：累计求最大值
  + `df.min(axis=None)`：求最小值
  + `df.cummin()`：累计求最小值
  + `df.std(axis=None)`：求标准差
  + `df.median(axis=None)`：求中位数
  + `df.abs()`：求绝对值
  + `df.count(axis=None)`：求有效数据数量
  + `df.prod(axis=None)`：求累乘
  + `df.cumprod()`：累计求累乘
+ `Panel`
  + `pandas.Panel(data, items=None, major_axis=None, minor_axis=None, dtype=None, copy=False)`：创建`Panel`对象
  + `p[item]`：索引
  + `p.major_xs(property)`：返回主轴数据
  + `p.minor_xs(property)`：返回次轴数据

## `scipy`

+ `scipy.constants`

  | character                                   | desciption                       |
  | ------------------------------------------- | -------------------------------- |
  | `constants.pi`                              | `圆周率 3.1415926535897931`      |
  | `constants.golden | constants.golden_ratio` | `黄金比例 1.618033988749895`     |
  | `constants.speed_of_light | constants.c`    | `真空中的光速 299792458.0`       |
  | `constants.h`                               | `普朗克常数 6.62607004e-34`      |
  | `constants.Planck`                          | `普朗克常数 6.62607004e-34`      |
  | `constants.G`                               | `引力常数 6.67408e-11`           |
  | `constants.e`                               | `基本电荷 1.6021766208e-19`      |
  | `constants.R`                               | `摩尔气体常数 8.3144598`         |
  | `constants.Avogadro`                        | `阿伏加德罗常数 6.022140857e+23` |
  | `constants.k`                               | `波尔兹曼常数 1.38064852e-23`    |
  | `constants.atomic_mass | constants.m_u`     | `原子质量 1.66053904e-27`        |
  | `constants.electron_mass | constants.m_e`   | `电子质量 9.10938356e-31`        |
  | `constants.proton_mass | constants.m_p`     | `质子质量 1.672621898e-27`       |
  | `constants.neutron_mass | constants.m_n`    | `中子质量 1.674927471e-27 `      |

+ `scipy.integrate`

  + `integrate.quad(func, a, b)`：一元积分，返回`(integral_value, absolute_error)`
  + `integrate.dblquad(func, a, b, gfun, hfun)`：二元积分，返回`(integral_value, absolute_error)`
  + `integrate.tplquad(func, a, b, gfun, hfun, qfun, rfun)`：二元积分，返回`(integral_value, absolute_error)`

+ `scipy.interpolate`

  + `interpolate.interp1d(x, y, kind)`：返回插值函数，`kind`可选参数`[data|linear|cubic|nearest]`
  + `interpolate.UnivariateSpline(x, y)`：返回`UnivariateSpline`对象，可用`set_smoothing_factor`调节平滑度，范围在`[0,1]`

+ `scipy.io`

  + `io.loadmat(path)`：载入`.mat`文件
  + `io.savemat(path)`：保存`.mat`文件
  + `io.whosmat(path)`：列出`.mat`文内的变量

+ `scipy.linalg`

  + `linalg.det(A)`：计算方阵`A`的行列式
  + `linalg.inv(A)`：计算方阵`A`的逆矩阵
  + `lonalg.norm(A)`：计算`A`的范数
  + `linalg.solve(A,B)`：求`Ax = B`的解
  + `linalg.eig(A)`：计算求方阵`A`的特征值和特征向量`(evalues, evectors)`
  + `linalg.svd(A)`：计算`A`的奇异值分解`(U, sigma, V)`，其中的`U, V`属于`unitary matrix`，即$UU^H=VV^H=I$，这些返回值之间的关系满足 $A=U(diag(sigma),0)^TV$

+ `scipy.special`

  + `special.cbrt(x)`：立方根函数
  + `special.exp2(x)`：以`2`为底的指数函数
  + `special.exp10(x)`：以`10`为底的指数函数
  + `special.exprel(x)`：相对误差指数函数
  + `special.comb(N，k)`：组合函数
  + `special.perm(N，k)`：排列函数
  + `special.gamma(x)`：伽马函数
  + `special.lambertw(x)`：兰伯特W函数，定义为`f(z) = z*exp(z)`的反函数

## `sympy`

+ 相关常数

  | character  | description    |
  | ---------- | -------------- |
  | `sympy.I`  | 虚数单位       |
  | `sympy.E`  | 自然对数的底数 |
  | `sympy.oo` | 无穷大         |
  | `sympy.pi` | 圆周率         |

+ 相关函数

  + `sympy.log(x, base)`：以`base`为底对数函数
  + `sympy.root(x, n)`：`x`的`n`次方根
  + `sympy.factorial(n)`：`n`的阶乘
  + `sympy.Symbol(symbol)`：返回用于符号计算的对象
  + `sympy.solve(func, symbol)`：求解方程`func = 0`
  + `sympy.summation(func, (symbol,a,b))`：以`func`为表达式从`a`到`b`求和
  + `sympy.limit(e, z, z0, dir='+')`：求表达式`e`在`z=z0`处的极限，默认为求右极限`dir='+'`
  + `sympy.diff(func, symbol)`：对`func`求导
  + `sympy.integrate(func, (symbol,a,b))`：以`func`为表达式从`a`到`b`求定积分
  + `sympy.integrate(func, symbol)`：以`func`为表达式求不定积分

+ 相关方法

  + `func.evalf(subs)`：`func`是由`symbols`经过运算得到的`object`，传入`symbols_dict`可获得函数值

## `collections`

+ `collections.namedtuple(typename, property_list)`：返回一个非正式的类，可以用`isinstance(var, typename)`判断类型，允许用下标索引，允许用`var.property`索引，允许像元组一般使用，由于元组不可变，需要用`var._replace(property=value)`更改属性值
+ `collections.deque(iter=None, maxlen=None)`：返回一个双向列表对象，其实例有多个方法：`append, appendleft, clear, copy, count, extend, extendleft, index, insert, pop, popleft, remove, reverse, rotate`
+ `collections.defaultdict(default_factory)`：除了在`key`不存在时返回默认值，其行为与`dict`完全一样，其参数`default_factory`是无参数的工厂函数，其返回值作为不存在的`key`的默认值
+ `collections.OrderedDict(*args, **kwds)`：除了`key`的有序性，其行为与`dict`完全一样，可以传入能够构造`dict`的`iter`创建`OrderedDict`对象
+ `collections.Counter(*args, **kwds)`：可以传入`iter`创建`Counter`对象，继承了`dict`，允许通过`var[elements]`索引，有多个方法：`copy, elements, most_common, subtract, update`

## `functools`

+ `functools.cmp_to_key(func)`：将老式的比较函数（`comparison function`）转换为关键字函数（`key function`），与接受`key function`的工具一同使用（例如`sorted，min，max，heapq.nlargest，itertools.groupby`），该函数主要用于将程序转换成`Python 3`格式的，因为`Python 3`中不支持比较函数
+ `functools.partial(func, *args, **kw)`：传入不完整的`args`后，不完整的参数会保存至返回的函数，新函数只需要传入剩下的参数
+ `functools.reduce(func, iter)`：归并函数，通过不断传入`iter`的元素再将结果作为元素继续递归调用得到结果
+ `functools.total_ordering`：只需要`from functools import total_ordering `，这是一个类装饰器，给定一个类，这个类定义了一个或者多个比较排序方法，这个类装饰器将会补充其余的比较方法，减少了自己定义所有比较方法时的工作量；被修饰的类必须至少定义 `__lt__()， __le__()，__gt__()，__ge__()`中的一个，同时，被修饰的类还应该提供` __eq__()`方法

## `chardet`

+ 相关函数
  + `chardet.detect(byte_str)`：返回编码信息的字典`{'encoding': 'xxx', 'confidence':possibility , 'language': 'xxx'}`

## `copy`

- 相关函数
  - `copy.copy(var)`：返回`var`的浅拷贝
  - `copy.deepcopy(var)`：返回`var`的深拷贝

## `csv`

+ 相关方法
  + `csv.reader(<_io.TextIOWrapper>, dialect='excel')`：使用文件对象创建`_csv.reader`对象，默认为`excel`编码风格
  + `csv.writer(<_io.TextIOWrapper>, dialect='excel')`：使用文件对象创建`_csv.writer`对象，默认为`excel`编码风格
  + `csv.writerrow(iter)`：写入一行数据
  + `csv.writerrows(iter)`：写入多行数据

## `hashlib`

+ 相关函数
  + `hashlib.md5(string=None)`：创建`md5`对象，可选传入`bytes`型字符串
  + `hashlib.new(name, data=b'')`：可选传传入`bytes`型字符串创建`name`对象，`name`是可选加密算法，如`md5`
+ 相关方法
  + `m.update(string)`：传入`bytes`型字符串更新`md5`对象
  + `m.hexdigest()`：以十六进制数字的字符串形式返回加密值

## `pyinstaller`

+ 命令行参数：`pyinstaller [-F][-i icon_path][-w] pyfile.py`
+ 说明：`-F：编译出的exe文件不依赖其他文件  -i：图标文件的路径  -w：不显示命令窗口  `
+ 生成位置：`C:\user\用户名\dist`
+ 递归过深处理：修改`.sepc`文件，增加代码`import sys \ sys.setrecursionlimit(5000)`，再在`cmd`运行代码`pyinstaller filename.sepc`

## `turtle`

+ `turtle`对象
  + `turtle.Turtle()`：返回`<turtle.Turtle>`对象
  + `t.clone()`：创建和返回具有相同位置、方向和海龟箭头属性的`<turtle.Turtle>`对象
  + `t.getturtle()|t.getpen()`：返回当前屏幕上第一个`<turtle.Turtle>`对象本身
  + ` t.getscreen()`：返回绘制着海龟的`<turtle_Screen>`对象
  + `ts.bgcolor(color)`：设置背景颜色
  + `ts.clear()`：清空画布
  + `ts.delay(delay=None)`：设置或返回绘图延迟(单位:毫秒)
  + `ts.mode(mode=None)`：返回或设置海龟模式为`standard,logo,world`
+ 控制画笔状态
  + `t.pendoun()|t.pd()|t.down()`：放下画笔
  + `t.penup()|t.pu()|t.up()`：提起画笔
  + `t.pensize(width)|t.width()`：设置画笔宽度
  + `t.pencolor(color)|t.color(color)`：设置画笔颜色
  + `t.fillcolor(color)`：设置填充颜色
  + `t.begin_fill()`：开始填充
  + `t.end_fill()`：结束填充
  + `t.showturtle()|t.st()`：显示`turtle`
  + `t.hideturtle()|t.ht()`：隐藏`turtle`
  + `t.isvisible()`：判断是否可见
  + `t.isdown()`：判断是否放下画笔
  + `t.filling()`：返回填充状态
  + `t.write(arg, move=False, align='left', font=('Arial',8,'normal'))`：写入文本
  + `t.shape(name=None)`：返回或设置画笔形状为`classic,arrow,triangle,turtle,square,circle`
  + `t.shapesize(stretch_wid=None, stretch_len=None, outline=None)`：返回或设置画笔形状拉伸度和轮廓宽度
  + `t.resizemode(rmode=None)`：返回或设置画笔重置模式为`auto,user,noresize`
  + `t.shapetransform(t11=None, t12=None, t21=None, t22=None)`：返回或设置箭头形状的当前变换矩阵
  + `t.clear()`：清除已绘屏，龟当前状态和位置不变
  + `t.reset()`：返回原点并清除已绘屏
+ 控制画笔运动
  + `t.forward(distance)|t.fd(distance)`：前进`distance`距离
  + `t.backward(distance)|t.bk(distance)|t.back(distance)`：后退`distance`距离
  + `t.right(angle)|t.rt(angle)`：右转`angle`角度
  + `t.left(angle)|t.lt(angle)`：左转`angle`角度
  + `t.circle(radius, extent=None, steps=None)`：以`radius`为半径，分`steps`步在左方画`extent`角度弧线
  + `t.dot(size, color)`：画一个半径为`size`的实心圆点
  + `t.setheading(to_angle)|t.seth(to_angle)`：设置朝向为`to_abgle`角度
  + `t.goto(x, y)|t.sstposition(x, y)|t.setpos(x, y)`：跳转画笔坐标至`(x, y)`
  + `t.position()|t.pos()`：返回坐标`(x, y)`
  + `t.xcor()`：返回`x`坐标
  + `t.ycor()`：返回`y`坐标
  + `t.setx(x)`：设置`x`坐标
  + `t.sety(y)`：设置`y`坐标
  + `t.home()`：返回原点
  + `t.speed(speed=None)`：设置画笔运动在`0<=speed<=10`之间，`speed=0`为最快
  + `t.undo()`：撤消最后一个动作
  + `t.degrees(fullcircle=360.0)`：设置一周的度数
  + `t.radians()`：设置弧度制
