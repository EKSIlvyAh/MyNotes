# SQL 基本语法 2 -- 某些主流数据库数据库中部分 SQL 语法的差异

## 文本模糊查找

文本模糊主要使用 `LIKE` 关键字。

LIKE 运算符支持 2 个通配符:

- `_` 匹配任意一个字符

- `%` 匹配任意多个字符

> **查询文本中包含%_通配符**

如果查找的文本中包含这个两个通配符，则可对齐进行转义。

可使用 `LIKE '字符串' ESCAPE '字符'` 语句来进行模糊查找。

其中 ESCAPE 表示指定的转义字符，只能是单个字符或者空字符，如果未使用 ESCAPE 关键字指定，
则对于 MySQL 和 PostgreSQL 来说，默认使用 `\`（反斜杠表示）。

*示例*

```sql
SELECT some_varchar_col
FROM a_table
WHERE some_varchar_col LIKE '%25#%%' ESCAPE '#';

-- 模糊查找包含 25% 的值为字符串的列，转义字符为 '#'
```

> **查询文本中需要区分大小写**

对于 LIKE 运算符，MySQL，Microsoft SQL Server 和 SQLite 的字符串默认不区分大小写，
而 Oracle 和 PostgreSQL 默认区分大小写。

如果想要区分大小写：

- PostgreSQL 提供了不区分大小写的 `ILIKE` 运算符，用法和 LIKE 一样

- Microsoft SQL Server 支持方括号匹配`（[]）`或者不匹配`（[^]）`指定范围或者匹配括号
内的任何单个字符，类似正则表达式，例如 [a-z] 表示匹配 a 到 z 的字符

- 对于其他的暂时不知道方法

## 排除重复数据

默认的查询结果是不排除重复数据的（默认使用 ALL 关键字）。

要排除重复数据则需要使用的 `DISTINCT` 关键字，位于 SELECT 关键字后。

*例如*

```sql
SELECT DISTINCT col_1, col_2
FROM a_table;

-- 可以用于多个字段的去重
```

在不同的数据库中，可以使用其他关键字代替：

- 在 Oracle 中，可以使用 UNIQUE

- 在 MySQL 中，可以使用 DISTINCTROW

## 数据排序

### 单列排序

单列排序指基于单个字段值的排序，由 `ORDER BY` 关键字指定排序规则。
语法如下：

```sql
SELECT col_1, col_2, ...
FROM a_table
[WHERE ...]
ORDER BY certain_col [ASC | DESC];

-- ASC(ascending) 由小到大排序，升序
-- DESC(descending) 由大到小排序，降序
```

不同类型的字段排序方法不同：

- `数字` 根据数值大小排序

- `字符` 根据编码顺序排序

- `日期和时间` 根据时间早晚排序

### 多列排序

如果排序字段中出现相同数据，那他们的排序是随机的。

要进一步明确排序顺序的话，可以指定多个字段，字段间用逗号隔开。

这时如果第一个字段相同，则根据第二个字段排序，以此类推。

语法如下

```sql
SELECT col_1, col_2, ...
FROM a_table
[WHERE ...]
ORDER BY certain_col_1 [ASC | DESC], certain_col_2 [ASC | DESC], ...;
```

### 基于表达式进行排序

除了基于字段，还可以基于表达式进行排序，例如：

```sql
-- 下面语句以下语句查找行政管理部门（dept_id = 1）的员工，
-- 并根据全年总收入进行排序

SELECT emp_name, salary * 12 + bonus
FROM employee
WHERE dept_id = 1
ORDER BY salary * 12 + bonus;

-- bonus 奖金
```

同时，我们也可以用`字段`或者`表达式`在 SELECT 列表中出现的位置来指定数据的排序，
例如上面的 sql 语句可以改写成下面的形式：

```sql
SELECT emp_name, salary * 12 + bonus
FROM employee
WHERE dept_id = 1
ORDER BY 2;

-- 表达式所在位置为第二个
```

### 关于空值（NULL）的排序位置

`空值（NULL）` 在数据库中表示未知的值或者缺失的数据。

不同的数据库对空值的处理方法不同，如下，对于空值的排序：

- MySQL，Microsoft SQL Server 和 SQLite 认为排序时空值最小

- Oracle 和 PostgreSQL 认为排序时空值最大

空值小，则在 ASC 排序中靠前，DESC 排序中靠后，空值大则相反。

不过 Oracle，PostgreSQL 和 SQLite 支持使用 `NULL FIRST` 和 `NULL LAST` 指定空值的排序位置，例如：

```sql
SELECT emp_name, bonus
FROM employee
WHERE dept_id = 2
ORDER BY bonus NULL LAST;

-- NULL LAST 则表示空值排最后
-- 如果是 NULL FIRST 则表示空值排最前
-- 这时无论是 ASC 还是 DESC 排序，都不影响空值记录的排序顺序，
-- 因为不是按照大小排序了
```

### 关于中文字符的排序（指定按照什么编码进行排序）

在创建数据库或者表时，通常会要求指定一个字符集和排序规则。

`字符集（Charset）`决定了数据库可以存储哪些字符，例如：

- ANSII 字符集（American Standard Code for Information Interchange，"美国信息交换标准码"）只能存储简单的英文和一些控制字符

- GB2312 字符集可以存储中文字符

- Unicode 字符集支持世界上所有字符

`排序规则（Collation）`定义了字符集中字符的排序规则，包括是否区分大小写，是否区分重音等等。
对于中文排序，排序方式和英文不同，通常需要根据拼音或者偏旁部首或者笔画进行排序。

支持中文排序的最简单的办法是使用支持中文排序的字符集和排序规则。
不过也可使用其他兼容性更好的办法。

<details>
<summary><b>对于 MySQL</b></summary></br>
MySQL 8.0 默认使用 utf8mb4 字符编码，中文按照偏旁部首进行排序。

可以通过一个转化函数来实现其他编码的排序，例如下面使用拼音对员工名字进行排序：

```sql
SELECT emp_name
FROM employee
ORDER BY CONVERT(emp_name, USING GBK);

-- CONVERT 是 MySQL 提供的系统函数，用于转化数据的字符集编码
-- GBK 编码默认使用拼音进行排序
```
</details>

</br>

<details>
<summary><b>对于 PostgreSQL</b></summary></br>
PostgreSQL 默认使用 UTF-8 字符编码，中文按照偏旁部首进行排序。

可以使用 `COLLATE` 关键字来选择其他排序规则，例如下面使用拼音对员工名字进行排序：

```sql
SELECT emp_name
FROM employee
ORDER BY COLLATE 'zh_CN';

-- COLLATE 表示按照某种编码规则进行排序
-- zh-CN 表示中文拼音排序
```
</details>

</br>

<details>
<summary><b>对于 Oracle</b></summary></br>
Oracle 默认使用 AL32UTF8 字符编码，中文按照偏旁部首进行排序。

可以通过一个转化函数来实现其他编码的排序，例如下面使用拼音对员工名字进行排序：

```sql
SELECT emp_name
FROM employee
ORDER BY NLSSORT(emp_name, 'NLS_SORT = SCHINESE_PINYIN_M');

-- NLSSORT 是 Oracle 提供的系统函数，用于转化数据的字符集编码
-- SCHINESE_PINYIN_M 表示中文的拼音排序规则
```

除开拼音排序外，Oracle 还支持按照`偏旁部首（SCHINESE_REDICAL_M）`以及`笔画（SCHINESE_STROKE_M）`进行中文排序。
</details>

</br>

<details>
<summary><b>对于 Microsoft SQL Servers</b></summary></br>
Microsoft SQL Servers 中字符集和排序规则是一个概念，安装时默认根据操作系统所在地区
选择排序规则，中国地区默认使用 Chinese_PRC_CI_AS 排序规则。

该排序规则中，中文根据偏旁部首进行排序，可以使用 `COLLATE` 关键字来选择其他排序规则
，例如下面使用拼音对员工名字进行排序：

```sql
SELECT emp_name
FROM employee
ORDER BY COLLATE Chinese_PRC_CI_AI_KS_WS;

-- COLLATE 表示按照某种编码规则进行排序
-- Chinese_PRC_CI_AI_KS_WS 表示中文拼音排序
```
</details>

## 限定数量结果

### 数据分页显示

有时数据的查询结果包含了成千上万条记录，在前端显示时需要分页显示。实现方式有两种：

> **第一种，使用 SQL 标准的 FETCH 和 OFFSET 子句**

*示例* Oracle，Microsoft SQL Server 和 PostgreSQL 支持如下写法。

```sql
SELECT emp_name, salary
FROM employee
ORDER BY salary DESC
OFFSET 10 ROWS
FETCH 10

-- 偏移（跳过） 10 行，抓取随后的 10 条记录（即抓取返回结果 10 - 20 行的记录）
```

除开以上基本用法，`FETCH` 关键字还支持以下扩展选项，完整语法如下：

*注：`[]` 代表可选，`{}` 代表必选，`|` 代表二选一*

```sql
[OFFSET m {ROW | ROWS}]
FETCH {FIRST | NEXT} [num_rows | n PERSCENT] {ROW | ROWS} {ONLY | WITH TIES};
```

**每个选项作用如下**：

- `OFFSET` 代表偏移量，即从 m + 1 处开始返回数据，默认为 0，即从第一行开始返回

- `ROW` 和 `ROWS` 等价（语法要求）

- `FETCH` 表示返回多少条数据

- `num_rows` 代表以行单位限制返回的数据

- `n PERCENT` 代表以百分比为单位限制返回的数据（返回 n % 的数据）

- `ONLY` 和 `WITH TIES` 区别在于，当有多个排名相同的记录时，WITH TIES 返回更多的记录，ONLY 不返回更多的记录

目前仅有 Oracle 12c 以上版本同时支持 n PERCENT 和 WITH TIES 选项，PostgreSQL 13 版本以上支持 WITH TIES 选项。


> **第二种，使用 LIMIT 和 OFFSET 子句**

LIMIT 关键字相对于 FTECH 关键字不支持选项，只能限制获取数据的条数。

但是使用该关键字的兼容性或许会更好。

*示例*

```sql
SELECT emp_name, salary
FROM employee
ORDER BY salary DESC
LIMIT 10 
OFFSET 10;
```

在 MySQL 和 SQLite 种，下面两种写法相同：

```sql
LIMIT m OFFSET n;
LIMIT n, m;
```

### Top-N 排行榜

像十大热门游戏，热搜排行榜等 Top-N 排行榜的原理是先对数据进行排序，然后返回前 N 条记录。

利用前面所说的 FETCH 或者 LIMIT 和 OFFSET 子句就可实现这一功能。

*示例 1* Oracle，Microsoft SQL Server，PostgreSQL 支持下面标准语法：

```sql
SELECT emp_name, salary
FROM employee
ORDER BY salary DESC
OFFSET 10 ROWS
FETCH 10 ROWS ONLY;
```

其中 Oracle 和 PostgreSQL 可省略 `OFFSET 0`，默认偏移量为 0。

*示例 2* MySQL，PostgreSQL 和 SQLite 支持下面的写法：

```sql
SELECT emp_name, salary
FROM employee
ORDER BY salary DESC
LIMIT 10 OFFSET 0;
```

*示例 3* 对于 Microsoft SQL Server 还支持如下写法：

```sql
SELECT TOP(5) emp_name, salary
FROM employee
ORDER BY salary DESC
```

其中 `TOP(n)` 表示调用 TOP 函数返回前 n 条记录。

## SQL 注释

> **单行注释**

SQL 中单行注释使用 `--`，在 MySQL 中 `--` 后面要跟一个空格才能起到注释效果。

另外 MySQL 还可使用 `#` 作为注释符号。

> **多行注释**

SQL 也支持使用 C 风格的多行注释 `/* ... */`

> **特殊注释**

Oracle 中的特殊注释 `--+` 和 `/*+hint*/` 用于表示查询优化器提示。

MySQL 的 `/*+hint*/` 注释作用也同上。除此之外，MySQL 还支持一种可执行的注释，例如：

```sql
CREATE TABLE t1 (id_ INT KEY)
/*! KEY_BLOCK_SIZE=1024 */
```

如果是 MySQL 5.1.10 以上版本，创建表 t1 时则会使用 KEY_BLOCK_SIZE 作为参数，如果是更低版本的 MySQL 或者其他数据库，则会忽略该参数，从而实现跨版本和可移植性。 
