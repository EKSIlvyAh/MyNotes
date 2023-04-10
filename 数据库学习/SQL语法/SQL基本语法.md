# SQL 基本语法

该文件中出现的 SQL 代码在 MySQL 下可正常运行，但不确定是否在其它数据库中正常运行。

在 Windows 中，SQL 的关键字不区分大小写，所以在代码中大小写都可。如果是 Linux 系统则需要区分大小写。

如果使用 MySQL WorkBench 来练习 SQL 代码，使用快捷键 ctrl + B 可以快速格式化代码。

## 创建/删除数据库

`CREATE DATABASE 数据库名称`;

注意不能创建同名建数据库

```sql
CREATE DATABASE spiders;
-- 创建名为spiders的数据库（schemas）注意分号结尾

DROP DATABASE spiders;
-- 删除名为spiders的数据库
```

## 创建/删除/重命名/修改表格

`USE 数据库名称`  

表示使用某个数据库

```sql
CREATE TABLE 数据库名称.表格名称 (
    列名 数据类型,
    ...
);
```

创建表格并声明数据名称及其类型（注意不能在同一个数据库下创建/删除同名表格）

`DROP TABLE 数据库名称.表格名称;`

删除该数据库下的某表格


`ALTER TABLE 表名 ADD 列名 数据类型 [约束条件];`

给该数据的某表新增某个列，例如

```sql
ALTER TABLE temp_table
ADD COLUMN col1 VARCHAR(100),
ADD col2 BIGINT;

-- 上面演示了添加一列或者多列的情况，同时 COLUMN 关键词可以省略
```


`ALTER TABLE 原表名 RENAME TO 新表名;`

重命名表名

`ALTER TABLE 表名 CHANGE COLUMN 列名 新列名（和原来保持一致即不修改）类型 限定条件;`

修改表格某列类型

建议表格都跟上数据库名称，防止弄混
### 数据类型
- INT **整形**
- VARCHAR(数值) **长度为“数值”的字符串**
- DATE **格式为 年-月-日 的表示日期的字符串**
- DOUBLE **双精度浮点型**

### 限定条件
- AUTO_INCREMENT **自动递增**
- PRIMARY KEY (列名) **设置主键（可以在查询数据时用作定位）**
- NULL **数据可以为空**
- NOT NULL **数据不可为空（如果为空会报错）**
- CHARACTER SET 编码格式 **设置字符串的编码格式**
- IF (NOT) exists 数据库/表格 **如果数据库/表格(不)存在，则执行某语句**

```sql
USE spiders;
-- 使用spiders数据库


CREATE TABLE spiders.image_record (
 	id INT AUTO_INCREMENT PRIMARY KEY,
    image_name VARCHAR(20) NOT NULL,
    downloadtime DATA NULL
 );
-- 在spiders数据库下创建名为img_recORd的表格
-- 并依次设置三个列名
-- id 整形，自动递增，设置为主键，用于作为图片的唯一标识, 
-- image_name 最大长度为20的字符串，设置为不可为空，名誉作为图片的名称,
-- downloadtime 日期类型，可以为空，表示图片的下载时间 


DROP spiders.image_record;
-- 删除spiders数据库下的image_recORd表格


CREATE TABLE IF NOT EXISTS Covid_total (
    `Country_id` INT NOT NULL AUTO_INCREMENT,
    `Country` VARCHAR(22) CHARACTER SET utf8,
    `CONtinent` VARCHAR(17) CHARACTER SET utf8,
    `PopulatiON` INT,
    `Total_cases` INT,
    `Total_deaths` INT,
    `Total_recovered` INT,
    `Total_active` INT,
    PRIMARY KEY (Country_id)
);
-- 大体同上，不过演示了更多语法规则
-- 自行体会
```

## 插入/修改/删除/更新数据

`INSERT INTO 数据库名称.表格名称 (列名，可略表示插入全部列) VALUES (与列名相对应的数据);`

在指定表格中插入数据

`ALTER TABLE 数据库名称.表格名称 ADD 列名 数据类型 （限定条件）; `

声明要改变表格，然后往某表中添加某一类型数据

`UPDATE 数据库名称.表格名称 SET 列名 = 数值 WHERE 定位条件;`

根据定位条件修改指定表格某处数据

`DELETE FROM 数据库名称.表格名称 WHERE 定位条件;`

根据定位条件修改指定表格**某行**数据

### <span style='colOR:green'>一些便捷的关键字</span>

* **ON DUPLICATE KEY UPDATE**

如果插入行出现唯一索引或者主键重复时，则执行旧的update；如果不会导致唯一索引或者主键重复时，就直接添加新行。

* **REPLACE**

如果插入行出现唯一索引或者主键重复时，则delete老记录，而录入新的记录；如果不会导致唯一索引或者主键重复时，就直接添加新行。

* **上两条关键字区别**
1. 在没有主键或者唯一索引重复时，replace与insert ON duplicate udpate相同。

2. 在主键或者唯一索引重复时，replace是delete老记录，而录入新的记录，所以原有的所有记录会被清除，这个时候，如果replace语句的字段不全的话，有些原有的比如c字段的值会被自动填充为默认值。
----
例子

`INSERT INTO 表 (列, ...) VALUES (数据, ...) ON DEPLICATE KEY UPDATE 列 = 新数据, ...;`

`INSERT INTO 表 (列, ...) VALUES (数据, ...) REPLACE;`

```sql
INSERT INTO spiders.image_record2 (id, image_name, downloadtime) VALUES (1, 'image01', '2022-12-26');
INSERT INTO spiders.image_record2  VALUES (2, 'image02', '2022-12-26');
INSERT INTO spiders.image_record2  VALUES (DEFAULT, 'image01', NULL);
-- 插入表格


ALTER TABLE spiders.image_record2 ADD image_size double NULL;
-- 改变表格然后增加列


UPDATE spiders.image_record2 SET image_size = 1.24 WHERE id = 1;
-- 更新表格某一列数据，注意这里的定位条件就是用 = 就表示相等了


DELETE FROM spiders.image_record2 WHERE id = 3;
-- 删除id为3那行

INSERT INTO spiders.students (id, NAME, age) VALUES (123456, 'Mary', 23) ON DUPLICATE KEY UPDATE name = 'Mary', age = 23;
-- 插入该数据，如果主键出现重复，则更新名字和年龄
```

## 数据的查询
`SELECT * FROM 数据库名称.表格名称;`

查看表格内所有内容

`SELECT 列名1, 列名2, ... FROM 数据库名称.表格名称;`

查看表格内某些列的数据

## 附加条件（去重，排序，过滤）

- DISTINCT **去重，选择不重复的内容（记得加在 SELECT 的后面）**
- ORDER BY 列名 **根据列名进行排排序（加在表名的后面）**

- ASC/desc **加在列名后面，表示排列顺序顺序（默认 ASC）**
    - ASC **即 ascending, 指从低到高，从小到大排列** 
    - DESC **即 descending, 指从高到低，从大到小排列** 

排序条件可以指定多个列。

```
SELECT * FROM egg_database.covid_total ORDER BY Country ASC;
-- 从egg_database.covid_total表格中查看所有内容，并且按照Country列从小到大排序


SELECT DISTINCT Country FROM egg_database.covid_total;
-- 从egg_database.covid_total表格中选取不重复的Country列的内容
```
### 添加条件进行过滤
`SELECT 列名1, 列名2, ... FROM 数据库名称.表格名称 WHERE 判断条件;`

根据判断条件选择特定内容

**条件运算符**

|比较运算符|含义|
|:-:|:-:|
|= | 等于|
|!= 或者 <> |不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
|BETWEEN|两值之间|
|in|一组值里|
|LIKE|相似匹配|

逻辑运算符就三个 AND OR NOT 或者 !。

其中`关系运算符`优先级大于`逻辑运算符`，
NOT 大于 AND 和 OR，AND 优先级 大于 OR。

在使用时要尽量避免依赖使用逻辑运算的优先级来进行条件筛选，
以便在不同数据库中的造成歧义。

```sql
SELECT 
    *
FROM
    egg_database.covid_month
WHERE
    Recovered >= 1000000
ORDER BY Confirmed DESC;
-- 从月度统计表筛选康复数 >= 100w 并且从小到大排序


SELECT 
    *
FROM
    egg_database.covid_month
WHERE
    NOT Recovered >= 1000000
        AND Country != 'Brazil'
ORDER BY Confirmed DESC;
-- 在上面语句的基础上，排除巴西国家的数据，并且加上NOT取反，选择康复数小于100w的（国家名是字符串，要用引号扩起来）


SELECT 
    *
FROM
    egg_database.covid_month
WHERE
    Recovered BETWEEN 1000000 AND 1500000
ORDER BY Confirmed DESC;
-- 这次选取康复数在100w到150w范围之间的


SELECT 
    *
FROM
    egg_database.covid_month
WHERE
    Country IN ('Brazil', 'India')
ORDER BY Confirmed DESC;
-- IN关键字可以根据字符串匹配查看范围在('Brazil', 'India')之内的国家

SELECT
	*
FROM
	egg_database.covid_month
WHERE
	Country LIKE 'A%'
ORDER BY Confirmed DESC;
-- 使用LIKE关键字模糊匹配国家开头为字母A的数据


SELECT
	*
FROM
	egg_database.covid_month
WHERE
	Country LIKE '%a'
ORDER BY Confirmed DESC;
-- 使用LIKE关键字模糊匹配国家结尾为字母a的数据


SELECT
	*
FROM
	egg_database.covid_month
WHERE
	Country LIKE '__b%'
ORDER BY Confirmed DESC;
-- 使用LIKE关键字模糊匹配国家第三个字符为b的数据，这里 _ （下划线）表示任意字符
-- 这里如果不加 % 就表示匹配的字符是3个，最后一个是b
```
## 合并表格
`SELECT 列名, ...（可以填 * 选择全部） FROM 表1 INNERr JOIN 表2 ON 合并条件`

把表1选中的列和表2的所有列，按照合并条件，合并在一起，类似交集

`SELECT 列名, ...（可以填 * 选择全部）FROM 表1 UNION (ALL) SELECT 列名, ...（可以填 * 选择全部）FROM 表2`

以并集形式合并两张表所选内容，类似并集

`SELECT 列名, ...（可以填 * 选择全部） FROM 表1 left / right JOIN 表2 ON 合并条件`

把表1选中的列和表2的所有列，按照合并条件，左/右合并在一起


```sql
SELECT 
    *
FROM
    egg_database.covid_month
        INNERR JOIN
    egg_database.covid_total ON egg_database.covid_month.Country = egg_database.covid_total.Country;
-- 根据两个表有相同的Country列，合并两个表，FROM后面的表连接在左，JOIN后面的表连接在右，类似交集，合并后的表一般会多出一些列


SELECT 
    Country, Confirmed
FROM
    egg_database.covid_month 
UNION SELECT 
    Country, Continent
FROM
    egg_database.covid_total;
-- 合并两个表中指定的列，默认去重，最后得到的表，将只包含合并的列


SELECT 
    Country, Confirmed
FROM
    egg_database.covid_month 
UNION ALL SELECT 
    Country, Continent
FROM
    egg_database.covid_total;
-- 在上面代码的基础上，给UNION添加ALL关键字，即可设置不去重，这样会有重复值的可能

SELECT 
    *
FROM
    egg_database.covid_month
LEFT JOIN
    egg_database.covid_total ON egg_database.covid_month.Country = egg_database.covid_total.Country;
-- 左合并，保留左边全部数据，把右边符合条件的连接过来


SELECT 
    *
FROM
    egg_database.covid_month
RIGHT JOIN
    egg_database.covid_total ON egg_database.covid_month.Country = egg_database.covid_total.Country;
-- 右合并，保留右边全部数据，把左边符合条件的连接过来
```

