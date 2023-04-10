# SQL 基本语法 3 -- SQL 函数

## SQL 函数介绍

为了提高数据的处理效率，SQL 提供了一系列预定义的功能模块，即 `SQL 函数`。

它是一个具有某种数据处理的模块，可以接收一个或者多个值，并返回一个输出值。

**SQL 中的函数主要分为以下两种类型**：

- `标量函数（Scalar Function）` 针对每个输出参数返回一个输出值（类似接受固定参数的函数）

- `聚合函数（Aggregate Function）` 基于一组输入进行汇总并返回一个结果。（类似接受可变参数的函数）

## 标量函数

常见的标量函数分为以下几类

- 数值函数

- 字符函数

- 日期函数

- 类型转化函数

**下面依次介绍这些函数**

### 数值函数

下面是常见的 SQL 数值函数以及各数据库的实现：

|数值函数|函数功能|Oracle|MySQL|Microsoft SQL Server|PostgreSQL|SQLite|
|:-|:-|:-|:-|:-|:-|:-|
|ABS(x)|计算 x 的绝对值|支持|支持|支持|支持|支持|
|CEIL(x), CEILING(x)|返回大于或等于 x 的最小整数|CEIL(x)|支持|CEILING(x)|支持|支持|
|FLOOR(x)|返回小于或等于 x 的最大整数|支持|支持|支持|支持|支持|
|MOD(x, y)|计算 x 除以 y 的余数（求模）|支持|支持|x % y|支持|x % y|
|ROUND(x, n)|将 x 四舍五入到 n 位小数|支持|支持|支持|支持|支持|
|RANDOM()|返回一个伪随机数|DBMS_RANDOM|RAND()|RAND()|支持|支持|

*注：SQLite 3.35.0 开始支持内置的数值函数，不再需要单独编译 extension-functions.c 文件*

**下面通过示例分别说明这些函数的具体用法和注意事项**。

<details><summary><b>绝对值函数</b></summary><br>

`ABS(x)` 函数计算输入的参数的绝对值，例如：

```sql
SELECT ABS(-1), ABS(1), ABS(0);

-- 输出应为 1，1，0
```
</details><br>

<details><summary><b>取整函数</b></summary><br>

`CEIL(x)` 和 `CEILING(x)` 函数返回大于或者等于 x 的最小整数，即向上取整。

`FLOOR(x)` 函数返回小于或者等于 x 的最大整数，即向下取整，例如：

```sql
SELECT CEIL(-2)，CEILING(2.5)，FLOOR(4.5);

-- 输出应为 -2, 3, 4
```

**Oracle 不支持 CEILING(x) 函数，Microsoft SQL Server 不支持 CEIL(x) 函数**。

`ROUND(x, n)` 函数将 x 四舍五入到 n 为小数，省略 n 参数则表示保留 0 位小数例如：

```sql
SELECT ROUND(9.556, 1), ROUND(9.556)
FROM dual;

-- 输出应为 9.5, 9
```
</details><br>

<details><summary><b>求余函数</b></summary><br>

`MOD(x, y)` 计算 x % y 的值，例如：

```sql
SELECT MOD(5, 3);

-- 输出应为 2
```
</details><br>

<details><summary><b>生成伪随机数</b></summary><br>

计算机生成的随机数都为伪随机数。

**MySQL** 使用 `RAND()`函数返回一个`值域为 [0, 1)` 的随机浮点数，在同一条查询语句中多次调用 RAND 将返回`不同的值`。

**Microsoft SQL Serve**r 使用 `RAND()` 函数返回的一个`值域为 (0, 1)` 的随机浮点数，在同一条查询语句中多次调用 RAND 将返回`相同的值`（不会更新随机数种子）。

也可以给 RAND 指定一个随机数种子来重现相同随机数，例如：

```sql
SELECT RAND(x);

-- 多次调用会返回相同的值
```

**Oracle** 则是提供了一个系统程序包 `DBMS_RANDOM`，调用其中的 `VALUE` 可以获取 `值域为 [0, 1)` 的随机浮点数，每次调用都会返回不同的随机数，例如：

```sql
SELECT DBMS_RANDOM.VALUE
FROM dual;
```
*注：对于 Oracle，每次调用 DBMS_RANDOM.VALUE 都会返回不同的随机数，同时，DBMS_RANDOM 中还提供了其他生成随机数和随机字符串的函数，以及设置随机数种子的方法*

**PostgreSQL** 则是使用 RANDOM() 函数，返回一个 `值域为[0, 1)` 的随机浮点数，每次调用都会返回不同的随机数，例如：

```sql
SELECT RANDOM();
```

如果要重现随机数（设置随机数种子），则可使用 `SETSEED(x)` 函数，例如：

```sql
SELECT SETSEED(1);
SELECT RANDOM();
```

**SQLite** 同样提供 `RANDOM()` 函数，返回一个 `值域为[-2^63, 2^63 - 1)` 的随机`整数`，每次调用返回不同的随机数，例如：

```sql
SELECT RANDOM();
```

*注：SQLite 不支持设置随机数种子，无法从重现相同的随机数*
</details><br>

### 字符函数

下面是常见的 SQL 字符函数以及各数据库的实现：

|字符函数|函数功能|Oracle|MySQL|Microsoft SQL Server|PostgreSQL|SQLite|
|:-|:-|:-|:-|:-|:-|:-|
|CONCAT(s1, s2, ...)|连接字符串|支持|支持|支持|支持|\|\| 运算符|
|INSTR(s, s1)|返回字串 s 中首次子串 s1 出现的位置|支持|支持|PATINDEX(s1, s)|POSITION(s1 IN s)|支持|
|LOWER(s)|返回字符串 s 的小写形式|支持|支持|支持|支持|支持|
|OCTET_LENGTH(s)|返回字符串 s 包含的字节数量|LENGTHB(s)|支持（或 LENGTH(s)）|DATALENGHT(s)|支持|不支持|
|LENGTH(s)|返回字符串 s 包含的字符数量|LENGTH(s)|CHAR_LENGTH(s)|LEN(s)|支持（或 CHAR_LENGTH(s)）|支持|
|REPLACE(s, old, new)|将字符串 s 中的 old 子串替换成 new 子串|支持|支持|支持|支持|支持|
|SUBSTRING(s, n, m)|返回从位置 n 开始的 m 个字符|SUBSTR(s, n, m)|支持|支持|支持|支持|
|TRIM(s1 FROM s)|删除字符串 s 开头和结尾的字串 s1|支持|支持|支持|支持|TRIM(s, s1)|
|UPPER(s)|返回字符串 s 的大小形式|支持|支持|支持|支持|支持|

**下面通过示例分别说明这些函数的具体用法和注意事项**

<details><summary><b>字符串的长度</b></summary><br>

字符串的长度可以按照字符数量和字节数量两种方式进行计算。

多字节编码中一个字节可以占多个字节。

`CHAR_LENGTH(s)` 函数用于计算字符串中字节的数量，例如：

```sql
SELECT CHAR_LENGTH('数据库'), OCTET_LENGTH('数据库');

-- 输出结果为 3, 9
-- UTF-8 字符中，一个汉字占 3 个字节
```

MySQL 和 PostgreSQL 支持这两个函数。

Oracle 则使用 `LENGTH(s)`, `LENTHB(s)` 两个函数分别计算字符数量和字节数量，例如：

```sql
SELECT LENGTH('数据库'), LENGTHB('数据库')
FROM daul;

-- 输出结果为 3, 9
```

值得注意的是，PostgreSQL 和 MySQL 也提供了 LENGTH(s) 函数，不过返回值不同：

- PostgreSQL LENGTH(s) 返回字符串中字符的数量

- MySQL LENGHT(s) 返回字符串的字节数量

Microsoft SQL Server 使用 `LEN(s)` 和 `DATALENGTH(s)` 函数分别计算字符数量和字节数量，例如：。

```sql
SELECT LEN(s), DATALENGTH(s);

-- 输出结果为 3, 6
-- 每个中文汉字在 Chinese_PRC_CI_AS 字符集中占用 2 个字节
```

SQLite 只提供了 `LENGTH(s)` 函数，用于计算字符串中字符的个数，例如：

```sql
SELECT LENGTH('数据库');

-- 输出结果为 3
```
</details><br>

<details><summary><b>连接字符串</b></summary><br>

MySQL，Microsoft SQL Server 和 PostgreSQL 支持使用 `CONCAT(s1, s2, ...)` 函数将两个或者多个字符串连接在一起，组成一个新的字符串，例如：

```sql
SELECT CONCAT('S', 'Q', 'L');

--- 结果为 SQL
```

Oracle 中的 `CONCAT(s1, s2)` 函数只支持一次连接两个字符串，例如：

```sql
SELECt CONCAT(CONCAT('S', 'Q'), 'L')
FROM dual;

-- 结果为 SQL
-- 通过嵌套调用的方式也可以实现连接多个字符串
```

SQLite 则不提供来连接字符串的函数，但是可以直接通过运算符 `||` 来连接字符串，例如：

```sql
SELECT 'S' || 'Q' || 'L';

-- 结果为 SQL
```

Oracle 和 PostgreSQL 也提供了连接运算符 `||`，Microsoft SQL Server 则使用 `+` 作为连接运算符。

另外，MySQL，Microsoft SQL Server 和 PostgreSQL 实现另一个字符串连接函数 `CONCAT_WS(sep, s1, s2, ...)` 函数，可以用指定分隔符连接字符串，例如：

```sql
SELECT CONCAT_WS('-', 'S', 'Q', 'L');

-- 结果为 S-Q-L
```
</details><br>

<details><summary><b>大小写转换</b></summary><br>

`LOWER(s)` 函数将字符串转换为小写形式，`UPPER(s)` 函数将字符串转换为大写形式，上述 5 种数据库都支持，例如：

```sql
SELECT LOWER('SQL'), UPPER('sql')
FROM a_table;

-- 结果为 sql, SQL
```

同时，MySQL 中的 `LCASE(s)` 函数等价于 LOWER(s)，`UCASE(s)` 函数等价于 UPPER(s)。

Oracle 和 PostgreSQL（12.x 以上版本） 还提供了首字母大写的 `INITCAP(s)` 函数。
</details><br>

<details><summary><b>获取子串</b></summary><br>

`SUBSTRING(s, n, m)` 函数返回字符串 s 从位置 n 开始的 m 个字符，除 Oracle 外，其他数据库都支持，例如：

*注：SQL 中，字符串的起始索引是 1 不是 0*

```sql
SELECT SUBSTRING('Hello', 1, 2);

-- 结果为 He
```

Oracle 使用简写的 `SUBSTR(s, n, m)` 函数返回子串，例如：

```sql
SELECT SUBSTR('数据库', 1, 2)
FROM dual;

-- 结果为 数据
```

同时 MySQL，PostgreSQL，SQLite 也支持简写的 SUBSTR 函数。

另外，Oracle，MySQL 以及 SQLite 中的 SUBSTRING / SUBSTR 函数的`起始位置 n`可以指定负数，表示从字符串的尾部倒数查找起始位置，再返回子串，例如：

```sql
SELECT SUBSTR('数据库', -2, 2)
FROM a_table;

-- 结果为 据库
```

MySQL，Micrisoft SQL Server 以及 PostgreSQL 提供了 LEFT_(s, n) 和 RIGHT(s, n) 函数，分别用于返回字符串`从开头往后`和`从结尾往前`的 n 个字符，例如：

```sql
SELECT LEFT('123456', 2), RIGHT('123456', 2);

-- 结果为 12, 56
```
</details><br>

<details><summary><b>子串查找与替换</b></summary><br>

`INSTR(s, s1)` 函数查找并返回字符串 s 中子串 s1 第一次出现的位置，如果未找到字符，则返回 0，Oracle，MySQL 以及 SQLite 支持该函数，例如：

```sql
SELECT INSTR('123456', '4')
FROM a_table;

-- 结果为 4
```

Microsoft SQL Server 使用 `PATINDEX(s1, s)` 函数查找字符串 s 中子串 s1 的位置，例如：

```sql
SELECT PATINDEX('123456', '4');

-- 结果为 4
```


PostgreSQL 使用 `POSITION(s1 IN s)` 函数查找字符串 s 中子串 s1 的位置，例如：

```sql
SELECT POSITION('4' IN '123456');

-- 结果为 4
```

`REPLACE(s, pold, new` 函数将字符串 s 中的子串 old 替换为 new 并返回替换的结果，例如：

```sql
SELECT REPLACE('123456', '4', '0')
FROM a_table;

-- 结果为 123056
```

以上五种数据库都支持 REPLACE 函数。
</details><br>

<details><summary><b>截断字符串</b></summary><br>

`TRIM (s1 FROM s)` 函数用于删除字符串 s 开头和结尾的子串 s1，除 SQLite 数据库外，其他四个数据库都支持，例如：

```sql
SELECT TRIM('-' FROM '----S-Q--L---'), TRIM('  S Q-L  ')
FROM a_table;

-- 结果为 S-Q--L, S Q-L
```

如果省略子串 s 则，默认剔除字符串两边的空格。

*注意：Oracle 中 TRIM 函数的参数 s1 只能是单个字符，其他数据库可以是多个字符*

SQLite 也支持 TRIM 函数但是调用格式为 `TRIM(s, s1)`，例如：

```sql
SELECT TIRM('---S-QL--', '-');

-- 结果为 S-QL
```

另外还有 `LTRIM(s)` 和 `RTRIM` 两个函数，分别用于删除左边或者右边的空格。
</details><br>

### 日期函数

下面是常见的 SQL 日期函数以及各数据库的实现：

|日期函数|函数功能|Oracle|MySQL|Microsoft SQL Server|PostgreSQL|SQLite|
|:-|:-|:-|:-|:-|:-|:-|
|CURRENT_DATE|返回当前日期|支持|支持|GETDATE()|支持|支持|
|CURRENT_TIME|返回当前时间|不支持|支持|GETDATE()|支持|支持|
|CURRENT_TIMESTAMP|返回当前日期|支持|支持|支持|支持|支持|
|EXTRACT(p FROM dt)|提取日期中部分信息|支持|支持|DATEPART_(p, dt)|支持|STRFTIME|
|dt - dt2|计算两个日期之间的间隔天数|支持|DATEDIFF(dt2, dt1)|DATEDIFF(p, dt1, dt2)|支持|STRFTIME
|dt + INTERVAL|日期加上一个日期间隔|支持|支持|DATEADD(p, n, dt)|支持|STRFTIME|

*注：dt = datetime 即日期时间 p = part 即日期中的一部分*

**下面通过示例分别说明这些函数的具体用法和注意事项**

<details><summary><b>返回当前日期和时间</b></summary><br>

`CURRENT_DATE`， `CURRENT_TIME`，`CURRENT_TIMESTAMP` 分别返回数据库的当前`日期`，`时间`以及`时间戳`（日期和时间，包含时区），MySQL，PostgreSQL，SQLite 支持以上函数，例如：

```sql
SELECT CURRENT_DATE, CURRENT_TIME, CURRENT_TIMESTAMP;

-- 结果为 "2023-04-10",  "12:27:06.435654+08:00", "2023-04-10 12:27:06.435654+08"
```

Oracle 中的日期类型包含了日期和时间信息，Oracle 不支持上述函数中的 `CURRENT_TIME` 函数，并且其余两个函数返回值相同，例如：

```sql
SELECT CURRENT_DATE, CURRENT_TIMESTAMP
FROM dual;

-- 结果为 "2023-04-10 12:27:06" 2023-04-10 12:27:06
```

在 Microsoft SQL Server 中，需要使用 `GETDATE` 函数来返回当前时间戳，然后通过类型转换函数 `CAST` 将其转换为日期或者时间类型，例如：

```sql
SELECT CAST(GETDATE() AS DATE), CAST(GETDATE() AS TIME), CURRENT_TIMESTAMP;

-- 结果为 "2023-04-10",  "12:27:06.435654+08:00", "2023-04-10 12:27:06.435654+08"
```
</details><br>

<details><summary><b>提取日期中的部分信息</b></summary><br>

`EXTRACT(p FROM dt)` 函数用于提取日期时间中的部分信息，比如年月日时分秒等，Oracle，MySQL 以及 PostgreSQL 支持该函数，例如：

```sql
SELECT EXTRACT(YEAR FROM CURRENT_DATE)
FROM a_table;

-- 结果为 2023
```

除此之外，还可利用其他时间关键字来提取日期中的其他信息，列举如下：

- YEAR 年

- MONTH 月

- DAY 日

- MINUTE 分

- SECOND 秒

Microsoft SQL Server 使用 `DATEPART(p, dt)` 函数来提取日期中的部分信息，例如：

```sql
SELECT DATEPART(YEAR, GETDATE());

-- 结果为 2023
```

SQLite 则提供了日期格式化函数 `STRFTIME`，可以提取日期中的信息，例如：

```sql
SELECT STRFTIME('%Y', CURRENT_DATE);

-- 结果为 2023
```
其中 `%Y` 是 SQLite 中的日期格式化字符串，代表 4 位数的年份，其余常用日期格式化字符串列举如下：

- %m 月

- %d 日

- %H 小时

- %M 分钟

- %S 秒
</details><br>

<details><summary><b>日期的加减运算</b></summary><br>

日期的加减运算主要包括，两个日期之间的相减以及一个日期加减一个时间间隔，Oracle，PostgreSQL 支持下面的写法：

**注意：日期之间不能相加**

```sql
SELECT 
DATE '2023-03-01' - DATE '2021-02-01',
DATE '2023-02-01' + INTERVAL '-1' MONTH  -- 表示减去一个月

-- 结果为 28, 2023-01-01
```

在 Oracle 和 PostgreSQL 中，两个日期相减可以得到他们之间相差的天数，日期加时间 `INTERVAL` 可以得到一个新的时间。

MySQL 中使用 `DATEDIFF(dt2, dt1)` 函数计算日期 dt2 - dt1 得到的天数，日期加间隔时间的写法和上面一样，例如：

```sql
SELECT 
DATEDIFF(DATE '2023-03-01', DATE '2021-02-01'),
DATE '2023-02-01' + INTERVAL '-1' MONTH;

-- 结果为 28, 2023-01-01
```

Microsoft SQL Server 中使用 `DATEDIFF(p, dt1, dt2)` 函数计算日期 dt2 - dt1 得到的时间间隔，使用 `DATEADD(p, n, dt)` 函数计算日期与时间间隔相加，例如：

```sql
SELECT 
DATEDIFF(DAY, DATE '2023-03-01', DATE '2021-02-01'),
DATEADD(MONTH, -1, '2023-02-01');

-- 结果为 28, 2023-01-01
```

其中 DATEDIFF 的第一个参数 `p` 可填写日期关键字，来修改返回的时间间隔单位，DATEADD 中的参数 `p` 也填写日期关键字，`n` 表示加上的日期数量。

SQLite 则依然使用 `STRFTIME` 函数实现两个日期的相减和日期与时间间隔的相加（减），例如：

```sql
SELECT 
STRFTIME('%J', '2023-03-01') - STRFTIME('%J', '2023-02-01'),
STRFTIME('%Y-%m-%d', '2023-02-01', '-1 months');

-- 结果为 28, 2023-01-01
```

其中 `%J` 表示将日期转换为`儒略日`（Julian Day，点击此处查看[**百度词条**](https://baike.baidu.com/item/%E5%84%92%E7%95%A5%E6%97%A5/693474?fr=aladdin)），STRFTIME 的第三个参数可以指定时间间隔。

</details><br>

### 类型转化函数

当不同类型数据在一起进行计算是，就会设置数据之间的转换，可以使用转换函数进行显示的类型转换，数据库也可能执行隐式的类型转换。

`CAST(expr AS type)` 函数用于将数据转换为其他的类型，以上五种数据库都支持，例如：

*注：expr = exprssion 即表达式*

```sql
SELECT CAST('123' AS INTEGER)
FROM a_table;
```

**值得注意的是，类型转换可能导致精度的丢失，并且 CAST 函数在各个数据库中支持的类型取决于数据库的实现。**

除开显示类型转换，数据库在执行某些操作时也会尝试进行隐式的类型转换，例如下面字符串与数字和日期进行相加：

```sql
SELECT '666' + 123, CONCAT('DATE: ', CURRENT_DATE)
FROM a_table;

-- 五种数据库在字符串与数字和日期进行相加时，都支持隐式的类型转换
-- 结果为 789，DATE：2023-04-09
```

可以看到，数字与字符串相加会将字符串转换为数字然后相加，而不是把数字转换成字符串然后相加。


## 聚合函数



