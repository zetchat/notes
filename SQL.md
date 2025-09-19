# SQl





## CONCAT() / CONCAT_WS()

​    功能:

​        CONCAT(): 将两个或多个字符串连接成一个字符串。

​        CONCAT_WS(): 使用指定的分隔符将多个字符串连接起来 (CONCAT With Separator)。

​    语法:

CONCAT(string1, string2, ...)

CONCAT_WS(separator, string1, string2, ...)

示例:

​    

  -- 使用 CONCAT

```
 SELECT CONCAT('SQL', ' ', 'is', ' ', 'fun!');
```

​    -- 返回: 'SQL is fun!'



​    -- 使用 CONCAT_WS 

```
SELECT CONCAT_WS('-', '2023', '10', '26');
```

​    -- 返回: '2023-10-26'

​    说明:

​        如果 CONCAT() 的任何参数为 NULL，则整个结果返回 NULL。

​        CONCAT_WS() 会忽略 NULL 参数（分隔符除外）。



## UPPER() / LOWER()

​    功能:

​        UPPER(): 将字符串转换为大写。

​        LOWER(): 将字符串转换为小写。

​    语法:

UPPER(string)

LOWER(string)

示例:  

```
  SELECT UPPER('Hello World'), LOWER('Hello World');
```

​    -- 返回: 'HELLO WORLD', 'hello world'



## COUNT()



​    功能: 计算指定列的行数。

​    语法:

COUNT(column_name)

COUNT(*)

COUNT(DISTINCT column_name)

示例:

假设有一个 Orders 表：

​    -- 计算总订单数

   

```
 SELECT COUNT(*) FROM Orders;
```

​    -- 计算有多少个客户下了单（去重）

​    

```
SELECT COUNT(DISTINCT CustomerID) FROM Orders;
```

​    说明:

​        COUNT(*) 会计算表中的所有行，包括 NULL 值。

​        COUNT(column_name) 会计算指定列中非 NULL 值的行数。

​        COUNT(DISTINCT column_name) 计算指定列中唯一且非 NULL 值的行数。



## SUM() / AVG()

​    功能:

​        SUM(): 计算数值列的总和。

​        AVG(): 计算数值列的平均值。

​    语法:

SUM(column_name)

AVG(column_name)

示例:

假设有一个 Products 表：

-- 计算所有商品的总价

```
SELECT SUM(Price) FROM Products;
```

-- 计算所有商品的平均价格

```
SELECT AVG(Price) FROM Products;
```

说明:

​    这两个函数都会忽略 NULL 值。



## =, IN, LIKE 的区别

这三个操作符都用于 WHERE 子句中进行数据筛选，但它们的匹配逻辑和使用场景完全不同。

### = (等于号)

​    功能:

​    用于进行 精确匹配。它判断列的值是否与给定的值 完全相等。

​    语法:

WHERE column_name = '具体的值'

示例:

查询 students 表中，名字 正好是 "李华" 的学生。

```
SELECT * FROM students WHERE name = '李华';
```

####     核心特点:

- 一对一的精确比较。
- 适用于任何数据类型（数字、字符串、日期等）。



### IN

​    功能:

​    用于判断列的值是否存在于一个给定的 值列表（集合） 中。

​    语法:

WHERE column_name IN ('值1', '值2', '值3', ...)

示例:

查询 students 表中，城市是 "北京"、"上海" 或 "深圳" 的学生。   

```
SELECT * FROM students WHERE city IN ('北京', '上海', '深圳');


```

####     核心特点:

- ​        一对多的离散值匹配。

- ​        是多个 OR 条件的简写形式，通常更简洁且查询效率更高。例如，上面的查询等价于 

- ```
  WHERE city = '北京' OR city = '上海' OR city = '深圳'。
  ```

- ​        列表中的值必须是相同或兼容的数据类型。



### LIKE



​    功能:

​    用于进行字符串的 模糊匹配（模式匹配）。它通常与通配符结合使用。

​    语法:

WHERE column_name LIKE '模式字符串'

常用通配符:

   % (百分号): 代表零个、一个或多个任意字符。

​    _ (下划线): 代表一个任意字符。



示例:

​    查询 students 表中，所有姓 "王" 的学生：

```
SELECT * FROM students WHERE name LIKE '王%';
```

查询 students 表中，名字里包含 "明" 字的学生：

```
SELECT * FROM students WHERE name LIKE '%明%';


```

查询 students 表中，姓 "李" 且名字是两个字的学生（如 "李四"）：

```
   SELECT * FROM students WHERE name LIKE '李_';
```



#### 核心特点:

- 专门用于字符串类型的模糊查询。
- 匹配的灵活性取决于通配符的使用。



# 函数



### REPLACE()



​    功能:

​    在一个字符串中，将所有出现的指定子字符串替换为另一个子字符串。

​    语法:

REPLACE(original_string, from_substring, to_substring)

示例:

​    将网址中的 http 替换为 https。

```
SELECT REPLACE('http://www.example.com', 'http://', 'https://');
```

-- 返回: 'https://www.example.com'



清除电话号码中的 - 分隔符。

```mysql
      SELECT REPLACE('010-8888-6666', '-', '');
```

​        -- 返回: '01088886666'



​    说明:

​        此函数是大小写敏感的（在大多数数据库中）。例如，REPLACE('Hello', 'h', 'H') 不会产生任何替换。



### LENGTH() / CHAR_LENGTH()



​    功能:

​    返回字符串的长度。

​    语法:

LENGTH(string)

CHAR_LENGTH(string) -- 或 CHARACTER_LENGTH(string)

示例:

```mysql
SELECT LENGTH('sql note');
```

-- 返回: 8

```mysql
SELECT CHAR_LENGTH('sql note');
```

-- 返回: 8

重要说明 (跨数据库差异):

​    LENGTH() 的行为在不同数据库中存在差异。在 MySQL 中，LENGTH() 返回字符串所占的字节数，而 CHAR_LENGTH() 返回字符数。对于多字节字符（如中文），结果会不同。



​        -- 在 MySQL (UTF-8) 中执行      

```mysql
SELECT LENGTH('你好');       -- 返回 6 (每个汉字占3个字节)

SELECT CHAR_LENGTH('你好'); -- 返回 2 (有两个字符)
```

​        在 PostgreSQL 和 SQL Server (LEN 函数) 中，LENGTH() 或 LEN() 通常直接返回字符数。



​        最佳实践: 为了确保一致性并准确获取字符数量，推荐使用 CHAR_LENGTH()。



### LEFT()



​    功能:

​    从字符串的左侧开始，提取指定数量的字符。

​    语法:

LEFT(string, number_of_chars)

示例:

​    获取邮政编码的前两位。

```mysql
SELECT LEFT('518057', 2);
```

-- 返回: '51'

从员工表中获取姓氏（假设姓名为 "张三" 格式）。        

```
SELECT LEFT(employee_name, 1) AS family_name FROM employees;
```

​    相关函数:    

```mysql
 RIGHT(string, number_of_chars): 功能与 LEFT() 相对，从右侧提取字符。
```



### ROUND()



​    功能:

​    将一个数值四舍五入到指定的小数位数。

​    语法:

ROUND(number, [decimals])

​    number: 要进行四舍五入的数值。

​    decimals (可选): 要保留的小数位数。如果省略，则四舍五入到最接近的整数。

示例:

​    保留两位小数。

```
SELECT ROUND(123.456, 2);
```

-- 返回: 123.46



四舍五入为整数。

```
SELECT ROUND(123.456, 0);
```

-- 返回: 123



```
SELECT ROUND(123.567); -- 省略 decimals 参数效果相同
```

-- 返回: 124



decimals 可以是负数，表示对小数点左边的部分进行四舍五入。

```
SELECT ROUND(123.456, -1); -- 四舍五入到最近的十位
```

-- 返回: 120



```
SELECT ROUND(158.1, -2); -- 四舍五入到最近的百位
```

-- 返回: 200



# SQL 连接查询 (`JOIN`) 知识体系目录

连接：https://blog.csdn.net/m0_50513629/article/details/138345063

### 一、 内连接 (`INNER JOIN`)

- **功能**: 返回两个表中连接字段相匹配的行，即两个表的交集。
- **1. 等值连接 (Equi Join)**
  - 描述：使用 `=` 运算符在 `ON` 子句中进行连接，是 `INNER JOIN` 最常见的形式。
  - 语法：`... FROM tableA INNER JOIN tableB ON tableA.column = tableB.column`
- **2. 不等值连接 (Non-Equi Join)**
  - 描述：使用除 `=` 之外的比较运算符（如 `>`、`<`、`BETWEEN`）进行连接。
  - 学习链接：`[在此处插入链接]`
- **3. 自然连接 (`NATURAL JOIN`)**      //非常罕见，太智能，不受控制
  - 描述：一种特殊的 `INNER JOIN`，数据库自动在两个表中所有同名的列上进行等值连接。
  - **注意**：因其隐式连接行为可能导致不可预期的结果，生产环境中不推荐使用。

------

### 二、 外连接 (`OUTER JOIN`)

- **功能**: 除了返回两个表中匹配的行外，还会返回一个表中不匹配的行。
- **1. 左外连接 (`LEFT JOIN` / `LEFT OUTER JOIN`)**
  - 描述：返回左表的所有行，以及右表中与左表匹配的行。如果右表中没有匹配项，则右表的列显示为 `NULL`。
  - 应用场景：查询用户及其订单信息（即使某些用户没有下过单）。
- **2. 右外连接 (`RIGHT JOIN` / `RIGHT OUTER JOIN`)**
  - 描述：返回右表的所有行，以及左表中与右表匹配的行。如果左表中没有匹配项，则左表的列显示为 `NULL`。
  - **说明**：功能与 `LEFT JOIN` 相对，大多数情况下可以通过调换表的位置用 `LEFT JOIN` 代替。
- **3. 全外连接 (`FULL OUTER JOIN`)**
  - 描述：返回左表和右表中的所有行。当某一行在另一个表中没有匹配时，另一个表的列显示为 `NULL`。本质上是 `LEFT JOIN` 和 `RIGHT JOIN` 结果的并集。
  - **注意**：MySQL 不直接支持 `FULL OUTER JOIN` 关键字，但可以通过 `LEFT JOIN UNION RIGHT JOIN` 来模拟实现。

------

### 三、 交叉连接 (`CROSS JOIN`)

- **功能**: 返回左表中的每一行与右表中的每一行组合后的结果，即 **笛卡尔积 (Cartesian Product)**。
- **语法**:
  - 显式：`... FROM tableA CROSS JOIN tableB`
  - 隐式：`... FROM tableA, tableB`
- **注意**：若缺少 `WHERE` 条件，结果集会非常庞大，需谨慎使用。

------

### 四、 自连接 (`SELF JOIN`)

- **功能**: 一种特殊的连接，指表与其自身进行连接。
- **描述**: 这不是一个独立的 `JOIN` 类型，而是一种用法。通过为同一张表起不同的别名，将其当作两个独立的表来处理。
- **应用场景**: 处理表内具有层级关系的数据，如查询员工及其对应的经理（经理本身也是员工）。



#  代码书写顺序 (Writing Order)

### 顺序是固定的，否则会报语法错误。

这是我们作为开发者，在编辑器里编写SQL时必须遵守的顺序。



```mysql
SELECT   -- 1. 要查询的列

DISTINCT -- (可选) 去重

FROM     -- 2. 从哪个表

JOIN     -- (可选) 连接哪个表

ON       -- (可选) 连接条件

WHERE    -- 3. 行级别的过滤条件

GROUP BY -- 4. 分组依据

HAVING   -- 5. 组级别的过滤条件

ORDER BY -- 6. 排序方式

LIMIT    -- 7. 限制返回的行数 (或 TOP, OFFSET)
```



# HAVING和where的区别

### 对比

where不能直接接聚合函数是**重点**

```
特性			WHERE 子句							HAVING 子句

作用对象	单个数据行 (Individual Rows)			  由 GROUP BY 创建的分组 (Groups of Rows)

执行顺序	在 GROUP BY 之前					   在 GROUP BY 之后

是否依赖	否，可以独立使用						是，必须与 GROUP BY 配合使用（或在整个结果集被视为一个组时使用）
GROUP BY	

聚合函数	不能 使用聚合函数（如COUNT(), SUM(), AVG()）	可以，并且这正是它的主要用途
```

### 聚合函数

| 函数        | 作用         | 支持性                           |
| :---------- | :----------- | :------------------------------- |
| sum(列名)   | 求和         | 通用支持                         |
| max(列名)   | 最大值       | 通用支持                         |
| min(列名)   | 最小值       | 通用支持                         |
| avg(列名)   | 平均值       | 通用支持                         |
| first(列名) | 第一条记录   | 仅Access支持                     |
| last(列名)  | 最后一条记录 | 仅Access支持                     |
| count(列名) | 统计记录数   | 通用支持（注意与count(*)的区别） |



### 不使用GROUP BY，但要使用聚合函数



#### 方法一：使用子查询 (Subquery) - 最通用、最标准

```mysql
SELECT *
FROM Sales
WHERE Amount = (SELECT MAX(Amount) FROM Sales);
```



#### 方法二：使用 `ORDER BY` 和 `LIMIT` - 最简洁（有局限性）

```mysql
SELECT *
FROM Sales
ORDER BY Amount DESC -- DESC 表示降序 (Descending)
LIMIT 1;
```



#### 方法三：使用窗口函数 (Window Functions) - 更强大、更灵活

```mysql
-- 使用通用表表达式 (CTE) 让语法更清晰
WITH RankedSales AS (
    SELECT
        *,
        -- 为每一行计算其在所有记录中按 Amount 降序的排名
        RANK() OVER (ORDER BY Amount DESC) AS rnk
    FROM
        Sales
)
SELECT OrderID, Product, City, Amount
FROM RankedSales
WHERE rnk = 1;
```



# MyBatis-Plus 分页插件：优雅的解决方案

MyBatis-Plus (MP) 的分页插件可以让我们完全从手动拼接 `LIMIT` 和计算总数的繁琐工作中解脱出来。

### 工作原理：SQL 拦截

分页插件的核心是**SQL 拦截器**。它的工作流程如下：

当你的程序运行一个含有`Page`的MyBatis-Plus语句，系统不会直接按照这个MyBatis-Plus语句对应的SQL的LIMIT语句执行，而是先执行LIMIT前面的SQL语句，然后得到初步结果，然后再加上LIMIT句子。

1. **拦截**：在你调用 MP 的查询方法（如 `selectPage`）时，插件会拦截即将发送给数据库的 SQL 语句。
2. **判断**：插件检查到你的方法调用中传入了一个 `Page` 对象，就明白你想要进行分页。
3. **改写 SQL**：插件根据 `Page` 对象中的页码和页面大小，自动在你的 SQL 语句末尾拼接上 `LIMIT` 子句。
4. **执行 `COUNT`**：插件会自动生成一条 `COUNT(*)` 语句并执行，以获取总记录数。
5. **封装结果**：最后，插件将查询到的**当前页数据列表**、**总记录数**、**总页数**等所有信息，全部封装回你传入的那个 `Page` 对象中。

新版本配置：

```java
package zht.zhxy.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
// 使用 @MapperScan 批量扫描 Mapper 接口，一劳永逸
@MapperScan("zht.zhxy.mapper")
public class MybatisPlusConfig {

@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
    // 1. 创建总的拦截器
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    // 2. 添加分页插件，并指定数据库类型为 MySQL
    interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
    return interceptor;
}

}
```

老版本配置：

```java
package zht.zhxy.config;

import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("zht.zhxy.mapper")
public class MyConfig { // 你的类名

    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }

}
```





# 锁





### 使用记录锁：

```mysql
SELECT * FROM t WHERE id = 10 FOR UPDATE;
```

**分析**: 查询条件是唯一索引 (`PRIMARY KEY`) 上的等值匹配，且记录存在。InnoDB 会进行优化，此时只会对 `id=10` 这条记录施加 **Record Lock**。它不会加任何间隙锁，因为唯一性约束保证了不会有其他事务插入 `id=10` 的记录，也就没有幻读的风险。



### 使用临键锁：

假设表格：

| id   | name    |
| ---- | ------- |
| 5    | Alice   |
| 10   | Bob     |
| 20   | Charlie |



```mysql
SELECT * FROM t WHERE id > 9 FOR UPDATE;
```

 	**分析**: 这是一个范围查询，是间隙锁的主要应用场景。InnoDB 的锁定过程如下：

1. 存储引擎从索引的起始位置开始扫描，找到第一个满足 `id > 9` 的记录，即 `id=10`。
2. 在 `id=10` 这条索引记录上，施加一个 **Next-Key Lock**。这个锁覆盖的范围是 `(5, 10]`。它包含了对 `id=10` 的记录锁和对 `(5, 10)` 的间隙锁。
3. 扫描继续，找到下一条记录 `id=20`。
4. 在 `id=20` 这条索引记录上，同样施加一个 **Next-Key Lock**。这个锁覆盖的范围是 `(10, 20]`。它包含了对 `id=20` 的记录锁和对 `(10, 20)` 的间隙锁。
5. 扫描继续，到达了索引的末尾。为了防止在 `id=20` 之后插入新的记录（例如 `id=30`），InnoDB 会在索引末尾的 `supremum` 伪记录上加一个 Next-Key Lock，这个锁实际效果是锁定 `(20, +∞)` 这个间隙。

**最终效果**: Session A 锁定了 `(5, 10]`, `(10, 20]`, `(20, +∞)`。综合起来，任何试图在 `id > 5` 的范围内插入新记录的事务都会被阻塞。虽然查询条件是 `id > 9`，但由于 Next-Key Lock 的工作方式，锁定的范围实际上从 `id=5` 之后就开始了。

此时，如果 Session B 执行 `INSERT INTO t VALUES (15);`，它会尝试获取一个插入意向锁，该锁与 Session A 持有的 `(10, 20]` 这个 Next-Key Lock（其中的 Gap Lock 部分）冲突，因此 Session B 会被阻塞，从而防止了幻读。



### 间隙锁：

```mysql
UPDATE t SET id = 16 WHERE id = 15;
```

由于上面的表中没有id=15的数据，所以也会用间隙锁

**分析**: 尝试更新一条不存在的记录。

1. InnoDB 会在索引中扫描定位到 `id=15` 应该插入的位置，即间隙 `(10, 20)`。
2. 由于记录不存在，无法施加 Record Lock。为了确保这条记录在事务 A 提交前“继续不存在”，InnoDB 会在这个间隙 `(10, 20)` 上施加一个 **Gap Lock**。

**最终效果**: `(10, 20)` 这个范围被锁定。任何试图插入 `id` 在 11 到 19 之间的事务都会被阻塞。这保证了谓词 `WHERE id = 15` 的结果在事务期间不会改变。



间隙锁和临键锁的区别：临键锁是间隙锁+记录锁。
