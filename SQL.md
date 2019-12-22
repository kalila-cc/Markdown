[TOC]

# 数据类型

## `Access`

| dtype           | description                                                  | Memory  |
| --------------- | ------------------------------------------------------------ | ------- |
| `Text`          | 用于文本或文本与数字的组合。最多`255`个字符                  |         |
| `Memo`          | 用于更大数量的文本，最多存储`65536`个字符，可搜索不可排序    |         |
| `Byte`          | 允许`0~255`的数字                                            | `1B`    |
| `Integer`       | 允许介于`-32768+32767`之间的数字                             | `2B`    |
| `Long`          | 允许介于`-2147483648~+2147483647`之间的全部数字              | `4B`    |
| `Single`        | 单精度浮点，处理大多数小数                                   | `4B`    |
| `Double`        | 双精度浮点，处理大多数小数                                   | `8B`    |
| `Currency`      | 用于货币。支持`15`位的元，外加`4`位小数                      | `8B`    |
| `Autonumber`    | 自动为每条记录分配数字，通常从`1`开始                        | `4B`    |
| `Date/Time`     | 用于日期和时间                                               | `8B`    |
| `Yes/No`        | 逻辑字段，可以显示为`Yes/No, True/False, On/Off`，在代码中，使用常量`True, False`（等价于`0, 1`，`Yes/No`字段中不允许`NULL`值 | `1b`    |
| `Ole Object`    | 可以存储图片、音频、视频或其他`BLOBs` (`Binary Large OBjects`) | `<=1GB` |
| `Hyperlink`     | 包含指向其他文件的链接，包括网页                             |         |
| `Lookup Wizard` | 允许你创建一个可从下列列表中进行选择的选项列表               | `4B`    |

## `MySQL`

+ `Text`

  | dtype              | description                                                  |
  | ------------------ | ------------------------------------------------------------ |
  | `CHAR(zise)`       | 保存固定长度的字符串（可包含字母、数字以及特殊字符），在括号中指定字符串的长度，最多`255`个字符 |
  | `VARCHAR(size)`    | 保存可变长度的字符串（可包含字母、数字以及特殊字符），在括号中指定字符串的最大长度，最多`255`个字符，如果值的长度大于`255`，则被转换为`TEXT`类型 |
  | `TINYTEXT`         | 存放最大长度为`255`个字符的字符串                            |
  | `TEXT`             | 存放最大长度为`65535`个字符的字符串                          |
  | `BLOB`             | 用于`BLOBs`(`Binary Large OBjects`)，存放最多`65535B`的数据  |
  | `MEDIUMTEXT`       | 存放最大长度为`16777215`个字符的字符串                       |
  | `MEDIUMBLOB`       | 用于`BLOBs`(`Binary Large OBjects`)，存放最多`16777215`字节的数据 |
  | `LONGTEXT`         | 存放最大长度为`4294967295`个字符的字符串                     |
  | `LONGBLOB`         | 用于`BLOBs`(`Binary Large OBjects`)，存放最多`4294967295`字节的数据 |
  | `ENUM(x,y,z,etc.)` | 允许你输入可能值的列表。可以在列表中列出最大`65535`个值，如果列表中不存在插入的值，则插入空值 |
  | `SET`              | 与`ENUM`类似，`SET`最多只能包含`64`个列表项，不过`SET`可存储一个以上的值 |

+ `Number`

  +  这些整数类型拥有额外的选项`UNSIGNED`，如果添加`UNSIGNED`属性，那么范围将从 0 开始，而不是某个负数

  | dtype             | description                                                  |
  | ----------------- | ------------------------------------------------------------ |
  | `TINYINT(size)`   | `-128~+127`常规，`0-255`无符号*。在括号中规定最大位数        |
  | `SMALLINT(size)`  | `-32768~+32767`常规，`0-65535`无符号*。在括号中规定最大位数  |
  | `MEDIUMINT(size)` | `-8388608~+8388607`常规，`0-16777215`无符号*。在括号中规定最大位数 |
  | `INT(size)`       | `-2147483648~+2147483647`常规，`0-4294967295`无符号*。在括号中规定最大位数 |
  | `BIGINT(size)`    | `-9223372036854775808~+9223372036854775807`常规，`0-18446744073709551615`无符号*。在括号中规定最大位数 |
  | `FLOAT(size,d)`   | 带有浮动小数点的小数字，在括号中规定最大位数，在`d`参数中规定小数点右侧的最大位数 |
  | `DOUBLE(size,d)`  | 带有浮动小数点的大数字，在括号中规定最大位数，在`d`参数中规定小数点右侧的最大位数 |
  | `DECIMAL(size,d)` | 作为字符串存储的`DOUBLE`类型，允许固定的小数点               |

+ `Date`

  + 即便`DATETIME`和`TIMESTAMP`返回相同的格式，它们的工作方式很不同。在`INSERT`或`UPDATE`查询中，`TIMESTAMP`自动把自身设置为当前的日期和时间。`TIMESTAMP`也接受不同的格式，比如`YYYYMMDDHHMMSS, YYMMDDHHMMSS, YYYYMMDD, YYMMDD`

  | dtype         | description                                                  |
  | ------------- | ------------------------------------------------------------ |
  | `DATE()`      | 日期。格式：`YYYY-MM-DD`，支持的范围是从`1000-01-01 to 9999-12-31` |
  | `DATETIME()`  | 日期和时间的组合。格式：`YYYY-MM-DD HH:MM:SS`，支持的范围是从`1000-01-01 00:00:00 to 9999-12-31 23:59:59` |
  | `TIMESTAMP()` | 时间戳。使用 Unix 纪元(`1970-01-01 00:00:00 UTC`)至今的描述来存储，格式：`YYYY-MM-DD HH:MM:SS`，支持的范围是从`1970-01-01 00:00:01 UTC to 2038-01-09 03:14:07 UTC` |
  | `TIME()`      | 时间。格式：`HH-MM-SS`，支持的范围是从`-838:59:59 to 838:59:59'` |
  | `YEAR()`      | `2/4`位格式的年，`4`位格式所允许的值：`1901-2155`。`2`位格式所允许的值：`70-69`，表示从 `1970-2069` |

## `SQL Server`

+ `Character`

| dtype          | description                              |
| -------------- | ---------------------------------------- |
| `char(n)`      | 固定长度的字符串，最多`8000`个字符       |
| `varchar(n)`   | 可变长度的字符串，最多`8000`个字符       |
| `varchar(max)` | 可变长度的字符串，最多`1073741824`个字符 |
| `text`         | 可变长度的字符串，最多`2GB`字符数据      |

+ `Unicode`

| dtype           | description                                    |
| --------------- | ---------------------------------------------- |
| `nchar(n)`      | 固定长度的`Unicode`数据，最多`4000`个字符      |
| `nvarchar(n)`   | 可变长度的`Unicode`数据，最多`4000`个字符      |
| `nvarchar(max)` | 可变长度的`Unicode`数据，最多`536870912`个字符 |
| `ntext`         | 可变长度的`Unicode`数据，最多`2GB`个字符       |

+ `Binary`

| dtype            | description                            |
| ---------------- | -------------------------------------- |
| `bit`            | 允许`0, 1, NULL`                       |
| `binary(n)`      | 固定长度的二进制数据，最多`8000`个字符 |
| `varbinary(n)`   | 可变长度的二进制数据，最多`8000`个字符 |
| `varbinary(max)` | 可变长度的二进制数据，最多`2GB`个字符  |
| `image`          | 可变长度的二进制数据，最多`2GB`个字符  |

+ `Number`

| dtype          | description                                                  | Memory   |
| -------------- | ------------------------------------------------------------ | -------- |
| `tinyint`      | 允许从`0-255`的所有数字                                      | `1B`     |
| `smallint`     | 允许从`-32768 to 32767`的所有数字                            | `2B`     |
| `int`          | 允许从`-2147483648 to 2147483647`的所有数字                  | `4B`     |
| `bigint`       | 允许从`-9223372036854775808 to 9223372036854775807`的所有数字 | `8B`     |
| `decimal(p,s)` | 固定精度和比例的数字，允许从`-10^38+1 to 10^38-1`之间的数字，`p`：可以存储的最大位数（小数点左侧和右侧），必须是`1-38`之间的值。默认是`18`，`s`：小数点右侧存储的最大位数，必须是`0-p`之间的值。默认是`0` | `5B-17B` |
| `numeric(p,s)` | 固定精度和比例的数字，允许从`-10^38+1 to 10^38-1`之间的数字，`p`：可以存储的最大位数（小数点左侧和右侧），必须是`1-38`之间的值。默认是`18`，`s`：小数点右侧存储的最大位数，必须是`0-p`之间的值。默认是`0` | `5B-17B` |
| `smallmoney`   | 介于`-214748.3648 to 214748.3647`之间的货币数据              | `4B`     |
| `money`        | 介于`-922337203685477.5808 to 922337203685477.5807`之间的货币数据 | `8B`     |
| `float(n)`     | 从`-1.79E+308 to 1.79E+308`的浮动精度数字数据，参数`n`指示该字段保存`4`字节还是`8`字节，`float(24)`保存 4 字节，而`float(53)`保存`8`字节，`n`的默认值是`53` | `4/8B`   |
| `real`         | 从`-3.40E+38 to 3.40E+38`的浮动精度数字数据                  | `4B`     |

+ `Date`

| dtype            | description                                                  | Memory   |
| ---------------- | ------------------------------------------------------------ | -------- |
| `datetime`       | 从`1753-01-01 to 9999-12-31`，精度为`3.33`毫秒               | `8B`     |
| `datetime2`      | 从`1753-01-01 to 9999-12-31`，精度为`100`纳秒                | `6B-8B`  |
| `smalldatetime`  | 从`1900-01-01 to 2079-06-06`，精度为`1`分钟                  | `4B`     |
| `date`           | 仅存储日期，从`0001-01-01 to 9999-12-31`                     | `3B`     |
| `time`           | 仅存储时间，精度为 100 纳秒                                  | `3-5B`   |
| `datetimeoffset` | 与`datetime2`相同，外加时区偏移                              | `8B-10B` |
| `timestamp`      | 存储唯一的数字，每当创建或修改某行时，该数字会更新，`timestamp`基于内部时钟，不对应真实时间，每个表只能有一个`timestamp`变量 |          |

+ `others`

| dtype              | description                                                  |
| ------------------ | ------------------------------------------------------------ |
| sql_variant``      | 存储最多`8000`字节不同数据类型的数据，除了`text, ntext, timestamp` |
| `uniqueidentifier` | 存储全局标识符(`GUID`)                                       |
| `xml`              | 存储`XML`格式化数据，最多 2GB                                |
| `cursor`           | 存储对用于数据库操作的指针的引用                             |
| `table`            | 存储结果集，供稍后处理                                       |

# 语句

## `SELECT ?`

+ `col,...`：从表中选取指定列数据，结果被存储在一个结果表中（称为结果集）
+ `DISTINCT col,...`：从表中选取指定列数据数据，结果被存储在一个结果表中（称为结果集），仅选取唯一不同的值
+ `*`：从表中选取所有列数据，结果被存储在一个结果表中（称为结果集）
+ `col AS c,...`：选取一个列并为它取一个别名
+ `TOP number col,...|*`：选取指定列的前`number`个
+ `TOP number PERCENT col,...|*`：选取指定列的前`number%`个

## `INTO ?`

+ `table [IN database]`：从一个表中选取数据，然后把数据插入另一个表或另一个数据库中，常用于创建表的备份复件或者用于对记录进行存档

## `FROM ?`

+ `table,...`：选择工作表
+ `table AS t,...`：选择一个工作表并为它取一个别名
+ `table [INNER|LEFT[ OUTER]|RIGHT[ OUTER]|FULL] JOIN table[ AS t] ON col = col`：联系两个工作表，`[INNER ]JOIN`：如果表中有至少一个匹配，则返回行；`LEFT JOIN`：即使右表中没有匹配，也从左表返回所有的行；`RIGHT JOIN`：即使左表中没有匹配，也从右表返回所有的行；`FULL JOIN`: 只要其中一个表中存在匹配，就返回行

## `WHERE ?`

+ `col op value [AND|OR ...]`：有条件地从表中选取数据，`value`为字符串时用单引号，`op`可选基本参数为`=, <>|!=, >, <, >=, <=`
+ `col = col`：绑定两个工作表的主键的对应关系
+ `col [NOT] LIKE pattern`：类似正则，大小写不敏感，对于参数`pattern`，`%`匹配任意个字符，`_`仅匹配一个字符，`[charlist]`匹配字符列中的任何单一字符，`[^charlist]|[!charlist]]`匹配不在字符列中的任何单一字符
+ `col IN (value,...)`：规定指定列的多个值以过滤列
+ `col BETWEEN value AND value`：选取介于两个值之间的数据。这些值可以是数值、文本或者日期
+ `ROWNUM op number`：选取指定列数
+ `col IS [NOT] NULL`：测试是否为`NULL`值

## `ORDER BY ?`

+ `col [ASC|DESC],...`：根据指定的列对结果集进行排序，升序为`ASC`，降序为`DESC`

## `INSERT INTO ?`

+ `table VALUES (value,...)`：向表格中插入新的行
+ `table (col,...) VALUES ([autoincrement.nextval,]value,...)`：向表格的指定列中插入新的数据

## `UPDATE ?`

+ `table SET col = value,... WHERE ?`：更新符合条件的指定列

## `DELETE ?`

+ `FROM table WHERE ?`：删除符合条件行
+ `* FROM table`：删除所有行
+ `FROM table`：删除所有行

## `CREATE ?`

+ `DATABASE database`：创建数据库

+ `TABLE table (col dtype[ constraint ...[AUTO_INCREMENT|AUTOINCREMENT|IDENTITY[(start, step)]]], [[CONSTRAINT constraint_name ]constraint (col,...) ...])`：添加数据库表，数据类型`dtype`如下，约束类型如下

  | dtype                                                     | description                                                  |
  | --------------------------------------------------------- | ------------------------------------------------------------ |
  | `integer(size), int(size), smallint(size), tinyint(size)` | 仅容纳整数。在括号内规定数字的最大位数                       |
  | `decimal(size, d), numeric(size, d)`                      | 容纳带有小数的数字，`size`规定数字的最大位数，`d`规定小数点右侧的最大位数 |
  | `char(size)`                                              | 容纳固定长度的字符串（可容纳字母、数字以及特殊字符），在括号中规定字符串的长度 |
  | `varchar(size)`                                           | 容纳可变长度的字符串（可容纳字母、数字以及特殊的字符），在括号中规定字符串的最大长度 |
  | `date(yyyymmdd)`                                          | 容纳日期，包含：`DATE(YYYY-MM-DD), DATETIME|SMALLDATETIME(YYYY-MM-DD HH:MM:SS), TIMESTAMP(YYYY-MM-DD HH:MM:SS|number), ` |

  | constraint    | description                                                  |
  | ------------- | ------------------------------------------------------------ |
  | `NOT NULL`    | 约束强制列不接受`NULL`值                                     |
  | `UNIQUE`      | 约束唯一标识数据库表中的每条记录，每个表可以有多个`UNIQUE`约束 |
  | `PRIMARY KEY` | 约束唯一标识数据库表中的每条记录，主键必须包含唯一的值，主键列不能包含`NULL`值，每个表都应该有一个主键，并且每个表只能有一个主键 |
  | `FOREIGN KEY` | 约束用于预防破坏表之间连接的动作，也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一，一个表中的`FOREIGN KEY`指向另一个表中的`PRIMARY KEY`，与`REFERENCES table (col,...)`连用 |
  | `CHECK`       | 约束用于限制列中的值的范围，如果对单个列定义`CHECK`约束，那么该列只允许特定的值；如果对一个表定义`CHECK`约束，那么此约束会在特定的列中对值进行限制，与`(col op value[AND|OR ...])`连用 |
  | `DEFAULT`     | 约束用于向列中插入默认值，如果没有规定其他的值，那么会将默认值添加到所有的新记录，与`value`连用 |

+ `INDEX index on table (col[ ASC|DESC],...)`：在表中创建索引；用户无法看到索引，它们只能被用来加速搜索/查询

+ `SEQUENCE autoincrement MINVALUE value START WITH value INCREMENT BY value CACHE value`：创建名为`autoincrement`的序列对象，它以`1`起始且以`1`递增，`CACHE`选项规定了为了提高访问速度要存储多少个缓存序列值

+ `VIEW view AS SELECT col AS c,... FROM table [WHERE ?]`：查询视图

+ `OR REPLACE VIEW view AS SELECT ? FROM ? [WHERE ?]`：更新视图

## `? UNION ?`

+ `SELECT ? UNION [ALL] SELECT ?`：合并两个或多个`SELECT`语句的结果集，结果集中的列名总是等于`UNION`中第一个`SELECT`语句中的列名，`UNION`内部的`SELECT`语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条`SELECT`语句中的列的顺序必须相同，`UNION`不会列出重复的内容，`UNION ALL`则列出所有内容

## `ALTER TABLE ?`

+ `table [AUTO_INCREMENT=value]`：开启更改表格属性模式，同时修改自动递增字段的初始值，`AUTO_INCREMENT`默认`start=1, step=1`
+ `[COLUMN] col SET DEFAULT value`：在表已存在的情况下为已存在列创建 `DEFAULT`约束
+ `[COLUMN] col DROP DEFAULT`：撤销`DEFAULT`约束
+ `[COLUMN] col dtype`：改变表中列的数据类型

## `ADD ?`

+ `[CONSTRAINT constraint_name ]constraint (col,...) ...`：命名约束，并定义多个列的约束
+ `col dtype`：在表中添加列

## `DROP ?`

+ `INDEX|CONSTRAINT|constraint [constraint_name]`：撤销约束
+ `INDEX [table.]index[ ON table]`：删除表格中的索引
+ `TABLE table`：删除表（表的结构、属性以及索引也会被删除）
+ `DATABASE database`：删除数据库
+ `COLUMN col`：删除表中的列
+ `VIEW view`：删除视图

## `TRUNCATE ?`

+ `TABLE table`：仅仅除去表内的数据，不删除表本身

## `SQL ?`

+ `CREATE OR REPLACE VIEW view`：开启更新视图
+ `DROP VIEW view`：开启删除视图

# 格式

## `SELECT`

+ `SELECT [DISTINCT] [TOP number[ PERCENT]] [tabel.]col[ AS c],...|* [INTO table [IN database]] FROM table[ AS t],... [[INNER|RIGHT[ OUTER]|LEFT[ OUTER]|FULL JOIN] table[ AS t] on col = col] [ORDDER BY col [ASC|DESC] | WHERE col [NOT][IN|BETWEEN|LIKE] ?[AND|OR ...]] [GROUP BY col,...[HAVING func(col) op value]]`

## `INSERT`

+ `INSERT INTO table [(col,...)] VALUES (value,...)`

## `UPDATE`

+ `UPDATE table SET col = value,... WHERE col [NOT][IN|BETWEEN|LIKE] ?[AND|OR ...]]`

## `DELETE`

+ `DELETE [*] FROM table [WHERE col [NOT][IN|BETWEEN|LIKE] ?[AND|OR ...]]`

## `UNION`

+ ``SELECT ? UNION [ALL] SELECT ?``

## `ALTER TABLE`

+ `ALTER TABLE table ADD|DROP|TRUNCATE ...`
+ `ALTER TABLE table [AUTO_INCREMENT=value] | [COLUMN] col dtype|DROP DEFAULT|SET DEFAULT value`

# 函数

## `DATE`

+ `CONVERT()`：用不同的格式显示日期/时间
+ `CURDATE()`：返回当前的日期
+ `CURTIME()`：返回当前的时间
+ `DATE()`：提取日期或日期/时间表达式的日期部分
+ `DATE_ADD() | DATEADD()`：给日期添加指定的时间间隔
+ `DATE_SUB()`：从日期减去指定的时间间隔
+ `DATEDIFF()`：返回两个日期之间的天数
+ `DATE_FORMAT()`：用不同的格式显示日期/时间
+ `EXTRACT() | DATEPART()`：返回日期/时间的单独部分
+ `NOW() | GETDATE()`：返回当前日期和时间

## `NULL`

+ `COALESCE(col, default)`：如果值是`NULL`则返回`default`
+ `IFNULL(col, default)`：如果值是`NULL`则返回`default`
+ `ISNULL(col, default)`：如果值是`NULL`则返回`default`
+ `NVL(col, default)`：如果值是`NULL`则返回`default`

## `Aggregate`

+ `AVG(col)`：返回某列的平均值
+ `BINARY_CHECKSUM`：返回按照表的某一行或表达式列表计算的二进制校验和值，忽略具有不可比数据类型的列
+ `CHECKSUM`：返回按照表的某一行或一组表达式计算出来的校验和值
+ `CHECKSUM_AGG`：返回组中各值的校验和，将忽略`NULL`值
+ `COUNT(col)`：返回某列的行数（不包括`NULL`值）
+ `COUNT(DISTINCT col)`：返回相异结果的行数
+ `COUNT(*)`：返回被选行数
+ `FIRST(col)`：返回在指定的域中第一个记录的值
+ `LAST(col)`：返回在指定的域中最后一个记录的值
+ `MAX(col)`：返回某列的最高值
+ `MIN(col)`：返回某列的最低值
+ `STDEV(col)`：返回某列的标准偏差
+ `STDEVP(col)`：返回某列总体的标准偏差
+ `SUM(col)`：返回某列的总和
+ `VAR(col)`：返回某列的方差
+ `VARP(col)`：返回某列总体的方差

## `Scalar`

+ `UCASE(c)`：将某个域转换为大写
+ `LCASE(c)`：将某个域转换为小写
+ `MID(c,start[,end])`：从某个文本域提取字符
+ `LEN(c)`：返回某个文本域的长度
+ `INSTR(c,char)`：返回在某个文本域中指定字符的数值位置
+ `LEFT(c,number_of_char)`：返回某个被请求的文本域的左侧部分
+ `RIGHT(c,number_of_char)`：返回某个被请求的文本域的右侧部分
+ `ROUND(c,decimals)`：对某个数值域进行指定小数位数的四舍五入
+ `MOD(x,y)`：返回除法操作的余数
+ `FORMAT(c,format)`：改变某个域的显示方式