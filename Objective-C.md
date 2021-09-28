# Objective-C

## 数据类型

### 基本类型

- 整数类型：`(unsigned|signed) char|short|int|long`，常量前缀包括`0b|0|0x`，常量后缀包括 `u(unsigned) | l(long) | ul(unsigned long)`
- 浮点类型：`float|double|long double(10B)`，常量后缀包括 `f(float)`
- 宏定义：`NSInteger|NSUInteger|CGFloat`

### 枚举类型

### void 类型

### 派生类型

- 指针类型、数组类型、结构类型、联合类型、函数类型

### 块

- 声明

  ```objective-c
  返回类型 (^块)(参数类型, ...);
  ```

- 实现

  ```objective-c
  ^(参数类型 参数, ...) {
      // 代码
  }
  ```

- 原理

  ```objective-c
  #define func_data_t spec_type_1
  #define func_ret_t spec_type_2
  #define block_data_t spec_type_3
  #define block_ret_t spec_type_4
  typedef func_ret_t (*func_ptr_t) (func_data_t);
  typedef block_ret_t (*block_t) (block_data_t);
  
  struct __block_impl {
      void *isa;
      int flags;
      int reserved;
      void *func_ptr;
      __block_impl(void *isa, int flags, int reversed, void *func_ptr):
      	isa(isa), flags(flags), reserved(reserved), func_ptr(func_ptr) {}
  }
  
  static void __block_func_spec(struct __block_spec *__cself) {}
  
  static struct __block_desc_spec {
      size_t reserved;
      size_t block_size;
      __block_desc_spec(size_t reserved, size_t block_size):
      	reserved(reserved), block_size(block_size) {}
  }
  
  struct __block_spec {
      struct __block_impl impl;
      struct __block_desc_spec desc_spec;
      func_data_t arg;
      __block_spec(void *func_ptr, struct __block_desc_spec* desc_spec_instance, func_data_t arg, int flags = 0):
          impl(&_NSConcreteXBlock, flags, 0, func_ptr) {
              desc_spec = desc_spec_instance;
          }
  }
  
  block_t block = (block_t)&__block_spec(
      (void *)__block_func_spec,
      &__block_desc_spec(0, sizeof(__block_spec)),
      arg
  );
  ((((__block_spec *)block)->impl).func_ptr)((__block_spec *)block);
  ```

## 面向对象

### 接口

```objective-c
@interface 接口: 被继承类 <协议> {
@访问权限
    数据类型 属性;
    ...
}
// 属性
@property(操作类型?, 分配方式?, 读写权限?) 数据类型 属性;
// 方法，返回值使用 id 则返回 NSObject *，使用 instancetype 则返回对应实例类型，自定义构造方法推荐使用
+ (返回类型) 静态方法: (参数类型) 参数名 加入参数: (参数类型) 参数名 ...;
- (返回类型) 实例方法: (参数类型) 参数名 加入参数: (参数类型) 参数名 ...;
- (返回类型) 无参实例方法;
@end
```

### 属性

```objective-c
@property(操作类型?, 分配方式?, 读写权限?) 数据类型 属性;
```

- 原子性
  - `atomic`：保证一定程度上的线程安全，默认方式
  - `nonatomic`：不保证线程安全，但效率更高
- 分配方式
  - `assign`：直接分配，不使用引用计数，基本数据类型推荐使用，默认方式
  - `retain`：直接分配，保留引用计数，指针类型推荐使用
  - `copy`：调用 `copy` 进行深拷贝操作，`NSString`推荐使用
  - `weak`：与 `assign` 类似，但引用计数为 0 后会被自动置为 `nil` 防止出现野指针，开启 `ARC` 后使用
  - `strong`：与 `retain` 相似，使用引用计数，即使对象在别处被通知 `release` 也不会导致意外释放，开启 `ARC` 后使用
  - `unsafe_unretained `：与 `weak` 相似，若对象被置为 `nil` 时访问会造成 crash，但效率比 `weak` 的查询修改弱引用的  `CFMutableDictionary`  哈希表要高
- 读写权限
  - `readwrite`：可读可写，默认方式
  - `readonly`：仅可读

### 实现

```objective-c
@implementation 接口
// 实际方法签名为 "方法:加入参数:..."
(+|-) (返回类型) 方法: (参数类型) 参数名 加入参数: (参数类型) 参数名 ... {
    // 代码
}
(+|-) (返回类型) 无参实例方法 {
    // 代码
}
// 显式为属性绑定变量，若同时自定义了 getter 和 setter 则等于无效
// 在最新的 XCode 中，合成部分由 XCode 处理，不需要包含 synthesize 语句
@synthesize 属性 = 变量;
// 显式定义构造函数
- (instancetype) init {
    // 可以用 self = [super init]; 调用父类构造函数
    // 可以用 [[NSMutableArray allocWithZone: [self zone]] init]; 初始化 NSMutableArray
    // 其他代码
    return self;
}
// 显式定义析构函数
- (void) dealloc {
    // 代码
}
// 可以自定义 getter, setter
- (返回类型) 属性 {
    // 代码中不使用 self，否则造成死循环
}
- (返回类型) set属性: (参数类型) 参数 {
    // 代码中不使用 self，否则造成死循环
}
// 可以自定义 copy，需要使用协议 <NSCoping>
- (instancetype) copyWithZone: (NSZone *) zone {
    // 可以通过 [self class] 获得类本身，并发送 allocWithZon: zone 指定空间进行分配，最后处理并返回指针
}
@end
```

### 发送

```objective-c
[接口 静态方法: 参数 加入参数: 参数 ...];
[实例 实例方法: 参数 加入参数: 参数 ...];
```

### 释放

```objective-c
obj = nil; // 等价于 [obj release]; obj = [nil retain];
```

### 继承

- 使用 `self = [super init]` 递归调用初始化构造函数

### 多态

- 在不同子类实现相同方法签名的方法

### 访问权限

- 变量
  - `public`： 作用范围最大，在任何地方
  - `protected`： 作用范围在自身类和继承自己的子类，默认
  - `private`： 作用范围只能在自身类
- 方法
  - `.h` 文件中声明于 `interface` 内的方法为共有方法
  - `.h` 文件未声明，但 `.m` 文件声明并实现的方法为私有方法，前提是不 `import` `.m` 文件

### 类别

```objective-c
@interface 类(类别)
// 类别内容声明
@end

@implementation 类
// 类别内容实现
@end
```

- 类别只能添加方法，不能添加新的实例变量 
- 类别可以添加属性，而且属性必须是 `@dynamic` 类型的，即可以通过点表达式来访问 `setter|getter` 方法 
- 若类别中的新方法的名称与现有类中方法名称重名，类别具有更高的优先级

### 扩展

```objective-c
@interface 类()
// 扩展内容声明
@end
```

- 类扩展实际上是没有类别名称的类别，它通常被称为匿名类别
- 类扩展声明的方法是在原始类的实现块中实现的，因此不能在框架类上声明类扩展
- 类扩展仅特定于添加类的私有方法和私有变量，外部不可访问不可继承，即默认访问权限是 `@private`

### 区别

|      | 类别                                                     | 扩展                                                         | 继承                                           |
| ---- | -------------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------- |
| 功能 | 为类添加方法，不用知道类的源码，添加变量                 | 为类添加私有变量和私有方法，在类的源文件中书写，所以知道类的源代码 | 即能为类添加方法又能添加变量                   |
| 特点 | 添加的方法称为类的一部分，可以被子类继承                 | 添加的变量和方法只有这个类内部访问，正常情况下外边不能访问，不能被子类继承 | 新添加的变量和方法只能子类才能具有，父类不具有 |
| 使用 | 使用原始类的对象（调用-方法）或者类（调用+方法）调用方法 | 在类内部（源文件）使用                                       | 只能子类使用                                   |

### 协议

```objective-c
@protocol 协议
@required
// 必需实现方法
@optional
// 可选实现方法
@end

// 通过 <协议> 实现协议，不需要在 interface 重新声明
@interface 类: 父类 <协议>
...
@end
```

### self 指针

- 静态方法中，`self`指向类型对象
- 实例方法中，`self` 指向实例对象

## NSX

### NSNumber

- 基本类型转`NSNumber`：`+ (NSNumber *) numberWithX`，`X = BOOL|Char|Double|Float|Int|NSInteger`
- `NSNumber`转基本类型：`- (X) XValue`，`X = bool|char|int|double|float|string|integer`

### NSString

- 转基本类型：`- (X) XValue`，`X = int|float|double|integer`
- 单词大写：`- (NSString *) capitalizedString`
- 字符索引：`- (unichar) characterAtIndex: (NSUInteger) index`
- 包含前后缀：`- (BOOL) (hasPrefix|hasSuffix) :(NSString *) aString`
- 判断相等：`- (BOOL) isEqualToString: (NSString *) aString`
- 转换大小写：`- (NSString *) (lowercaseString|uppercaseString)`
- 获取长度：`- (NSUInteger) length`
- 子串搜索：`- (NSRange) rangeOfString: (NSString *) aString`，`typedef struct _NSRange { NSUInteger location; NSUInteger length; } NSRange`
- 获取子串：`- (NSString *) substringFromIndex: (NSUInteger) anIndex `、`- (NSString *) substringWithRange: (NSRange) range`
- 格式化初始化：`- (id) initWithFormat: (NSString *) format ...`
- 追加格式化：`- (NSString *) stringByAppendingFormat: (NSString *) format ...`
- 分割字符串：`[str componentsSeparatedByString: str]`、`[str componentsSeparatedByCharactersInSet: characterSet]`

### NSScanner

- 创建：
  - 从字符串：`[NSScanner scannerWithString: str]`
- 查询
  - 当前待扫描位置：`(readwrite) scanner.scanLocation`
  - 是否扫描到末尾：`[scanner isAtEnd]`
- 修改
  - 扫描至指定字符串（不包括）：`[scanner scanUpToString: str intoString: &str] -> BOOL`
  - 扫描至指定字符集（不包括）：`[scanner scanUpToCharactersInSet: characterSet intoString: &str] -> BOOL`
  - 扫描整数：`[scanner scanInteger: &i] -> BOOL`
  - 扫描浮点数：`[scanner scanFloat: &f] -> BOOL`
  - 扫描字符串：`[scanner scanString: str, intoString: &str]`

### Collections

#### NSArray

- 创建
  - 无元素：`[NSArray array]`
  - 单元素：`[NSArray arrayWithObject: obj]`
  - 多元素：`[NSArray arrayWithObjects: obj, ..., nil]`，`[[NSArray alloc] initWithObjects: obj, ..., nil]`
  - 从数组：`[NSArray arrayWithArray: arr]`
  - 简化形式：`@[obj, ...]`
- 查询
  - 获取长度：`ins.count`
  - 根据索引获取元素：`[arr objectAtIndex: index]`，`arr[index]`
  - 根据元素获取索引：`[arr indexOfObject: obj]`
  - 判断是否包含元素：`[arr containsObject: obj]`
  - 获取描述：`[dict description]`
- 转换
  - 提取子序列：`[arr objectsAtIndexes: indexSet]`
  - 数组转枚举器：`[arr objectEnumerator]`，`[arr reverseObjectEnumerator]`
  - 枚举器转数组：`[enumerator allObjects]`
  - 数组转字符串（拼接）：`[arr componentsJoinedByString: str]`
  - 字符串转数组（分割）：`[str componentsSeparatedByString: str]`、`[str componentsSeparatedByCharactersInSet: characterSet]`
- 读写
  - 写入：`[array writeToFile: filePath atomically: bool] -> BOOL`
  - 读出：`[NSArray arrayWithContentsOfFile: filePath]`

#### NSSet

- 创建
  - 无元素：`[NSSet set]`
  - 单元素：`[NSSet setWithObject: obj]`
  - 多元素：`[NSSet setWithObjects: obj, ..., nil]`，`[[NSSet alloc] initWithObjects: obj, ..., nil]`
  - 从数组：`[NSSet setWithArray: arr]`
- 查询
  - 获取长度：`ins.count`
  - 判断是否相等：`[set isEqualToSet: _set]`
  - 判断是否为子集：`[set isSubsetOfSet: _set]`
  - 判断是否有交集：`[set intersectsSet: _set]`
  - 判断是否包含元素：`[set containsObject: obj]`
  - 随机获取元素：`[set anyObject]`
  - 判断是否存在元素满足条件：`[set bk_any: ^BOOL(id)]`
  - 判断是否所有元素满足条件：`[set bk_all: ^BOOL(id)]`
  - 判断是否所有元素不满足条件：`[set bk_none: ^BOOL(id)]`
  - 获取描述：`[dict description]`
- 转换
  - 集合转数组：`[set allObjects]`
  - 集合转枚举器：`[set objectEnumerator]`
  - 过滤元素：`[set bk_select: ^BOOL(id)]`
  - 过滤元素（仅匹配一次）：`[set bk_match: ^BOOL(id)]`
  - 取反过滤：`[set bk_reject: ^BOOL(id)]`
  - 元素映射：`[set bk_map: ^id(id)]`

#### NSDictionary

- 创建
  - 无键值对：`[NSDictionary dictionary]`
  - 单键值对：`[NSDictionary dictionaryWithObject: value forKey: key]`
  - 多键值对：`[NSDictionary dictionaryWithObjects: values forKeys: keys]`，`[[NSDictionary  alloc] initWithObjects: values forKeys: keys count: count]`，`[[NSDictionary  alloc] initWithObjectsAndKeys: value, key, ..., nil]`
  - 从字典：`[NSDictionary dictionaryWithDictionary: dict]`
  - 从文件：`[[NSDictionary alloc] initWithContentsOfFile: filePath]`
- 查询
  - 获取长度：`[dict count]`
  - 判断是否相等：`[dict isEqualToDictionary: _dict]`
  - 根据键获取值：`[dict objectForKey: key]`
  - 获取描述：`[dict description]`
- 转换
  - 字典转键数组：`[dict allKeys]`
  - 字典转值数组：`[dict allValues]`
  - 字典转键枚举器：`[dict keyEnumerator]`
  - 字典转值枚举器：`[dict objectEnumerator]`
  - 字典转同值的键数组：`[dict allKeysForObject: obj]`
- 读写
  - 写入：`[dict writeToFile: filePath atomically: bool] -> BOOL`
  - 读出：`[NSDictionary dictionaryWithContentsOfFile: filePath]`

#### NSMutableArray

- 添加
  - 追加单元素：`[marr addObject: obj]`
  - 追加多元素：`[marr addObjectsFromArray: arr]`
  - 插入单元素：`[marr insertObject: obj]`
- 删除
  - 删除指定元素：`[marr removeObject: obj]`
  - 删除指定下标元素：`[marr removeObjectAtIndex: index]`
  - 删除末尾元素：`[marr removeLastObject]`
  - 删除所有元素：`[marr removeAllObjects]`
  - 删除数组中含有的元素：`[marr removeAllObjectsInArray: arr]`
- 修改
  - 替换元素：`[marr replaceObjectAtIndex: index withObject: obj]`、`[marr replaceObjectsInRange: range withObjectsFromArray: arr]`
  - 交换元素：`[marr exchangeObjectAtIndex: index withObjectAtIndex: index]`
  - 元素排序：`[marr sortedArrayUsingSelector: selector]`

#### NSMutableSet

- 创建
  - 指定容量：`[[NSMutableSet alloc] initWithCapacity: capacity]`
  - 指定集合：`[mset setSet: set]`
- 添加
  - 添加单元素：`[mset addObject: obj]`
  - 添加多元素：`[mset addObjectsFromArray: arr]`
  - 并集：`[mset unionSet: set]`
  - 差集：`[mset minusSet: set]`
- 删除
  - 删除单元素：`[mset removeObject: obj]`
- 转换
  - 过滤元素：`[set bk_performSelect: ^BOOL(id)]`
  - 过滤元素（仅匹配一次）：`[set bk_performMatch: ^BOOL(id)]`
  - 取反过滤：`[set bk_performReject: ^BOOL(id)]`
  - 元素映射：`[set bk_performMap: ^id(id)]`

#### NSMutableDictionary

#### NSIndexSet

- 创建
  - 从范围：`[[NSIndexSet alloc] initWithIndexesRange: range]`
- 添加
  - `[indexSet addIndex: index]`

#### NSCharacterSet

- 创建
  - 从字符串：`[NSCharacterSet characterSetWithCharactersInString: str]`
  - 从字符集：`[characterSet invertedSet]`
  - 数字字符集：`[NSCharacterSet decimalDigitCharacterSet]`
  - 字母字符集：`[NSCharacterSet letterCharacterSet]`
  - 小写字母字符集：`[NSCharacterSet lowercaseLetterCharacterSet]`
  - 大写字母字符集：`[NSCharacterSet uppercaseLetterCharacterSet]`
  - 数字字母字符集：`[NSCharacterSet alphanumericCharacterSet]`
  - 标点符号字符集：`[NSCharacterSet punctuationCharacterSet]`
  - 空格字符集：`[NSCharacterSet whitespaceCharacterSet]`
  - 换行字符集：`[NSCharacterSet newlineCharacterSet]`
  - 空格换行字符集：`[NSCharacterSet whitespaceAndNewlineCharacterSet]`
- 修改
  - 取反：`[characterSet  invert]`

#### NSEnumerator

- 创建
  - 从数组：`[arr objectEnumerator]`，`[arr reverseObjectEnumerator]`
  - 从集合：`[set objectEnumerator]`
  - 从字典：`[dict keyEnumerator]`，`[dict objectEnumerator]`
- 读取
  - 迭代读取：`[enumerator nextObject]`

### NSError

生成错误：`+ (NSError *) errorWithDomain: (NSStirng *) code: (NSInteger) userInfo: (NSDictionary *)`

### NSFunction

- `NSRange NSMakeRange(NSUInteger loc, NSUInteger len)`

## 错误处理

- 代码结构：`@try {...} @catch (NSException *exception) {...} @finally {...}`
- 生成异常：`[NSException exceptionWithName: str reason: str userInfo: dict]`
- 抛出异常：`@throw exception`
