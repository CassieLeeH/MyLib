# 基础篇



## 通用语法及分类



- DDL: 数据定义语言，用来定义数据库对象（数据库、表、字段）

- DML: 数据操作语言，用来对数据库表中的数据进行增删改

- DQL: 数据查询语言，用来查询数据库中表的记录

- DCL: 数据控制语言，用来创建数据库用户、控制数据库的控制权限



### DDL（数据定义语言）



数据定义语言



#### 数据库操作



查询所有数据库：
`SHOW DATABASES;`
查询当前数据库：
`SELECT DATABASE();`
创建数据库：
`CREATE DATABASE [ IF NOT EXISTS ] 数据库名 [ DEFAULT CHARSET 字符集] [COLLATE 排序规则 ];`
删除数据库：
`DROP DATABASE [ IF EXISTS ] 数据库名;`
使用数据库：
`USE 数据库名;`



注意事项

- UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集



#### 表操作



查询当前数据库所有表：
`SHOW TABLES;`
查询表结构：
`DESC 表名;`
查询指定表的建表语句：
`SHOW CREATE TABLE 表名;`



创建表：



```plain
CREATE TABLE 表名(
	字段1 字段1类型 [COMMENT 字段1注释],
	字段2 字段2类型 [COMMENT 字段2注释],
	字段3 字段3类型 [COMMENT 字段3注释],
	...
	字段n 字段n类型 [COMMENT 字段n注释]
)[ COMMENT 表注释 ];
```



**最后一个字段后面没有逗号**



添加字段：
`ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];`
例：`ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';`



修改数据类型：
`ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);`
修改字段名和字段类型：
`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];`
例：将emp表的nickname字段修改为username，类型为varchar(30)
`ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';`



删除字段：
`ALTER TABLE 表名 DROP 字段名;`



修改表名：
`ALTER TABLE 表名 RENAME TO 新表名`



删除表：
`DROP TABLE [IF EXISTS] 表名;`
删除表，并重新创建该表：
`TRUNCATE TABLE 表名;`



### DML（数据操作语言）

#### 添加数据

指定字段：
`INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);`
全部字段：
`INSERT INTO 表名 VALUES (值1, 值2, ...);`



批量添加数据：
`INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`
`INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`



注意事项

- 字符串和日期类型数据应该包含在引号中

- 插入的数据大小应该在字段的规定范围内



#### 更新和删除数据



修改数据：
`UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];`
例：
`UPDATE emp SET name = 'Jack' WHERE id = 1;`



删除数据：
`DELETE FROM 表名 [ WHERE 条件 ];`



### DQL（数据查询语言）



语法：



```plain
SELECT
	字段列表
FROM
	表名字段
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后的条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```



#### 基础查询



查询多个字段：
`SELECT 字段1, 字段2, 字段3, ... FROM 表名;`
`SELECT * FROM 表名;`



设置别名：
`SELECT 字段1 [ AS 别名1 ], 字段2 [ AS 别名2 ], 字段3 [ AS 别名3 ], ... FROM 表名;`
`SELECT 字段1 [ 别名1 ], 字段2 [ 别名2 ], 字段3 [ 别名3 ], ... FROM 表名;`



去除重复记录：
`SELECT DISTINCT 字段列表 FROM 表名;`



转义：
`SELECT * FROM 表名 WHERE name LIKE '/_张三' ESCAPE '/'`
/ 之后的_不作为通配符



#### 条件查询



语法：
`SELECT 字段列表 FROM 表名 WHERE 条件列表;`



条件：

| 比较运算符          | 功能                                       |
| ------------------- | ------------------------------------------ |
| >                   | 大于                                       |
| >=                  | 大于等于                                   |
| <                   | 小于                                       |
| <=                  | 小于等于                                   |
| =                   | 等于                                       |
| <> 或 !=            | 不等于                                     |
| BETWEEN ... AND ... | 在某个范围内（含最小、最大值）             |
| IN(...)             | 在in之后的列表中的值，多选一               |
| LIKE 占位符         | 模糊匹配（_匹配单个字符，%匹配任意个字符） |
| IS NULL             | 是NULL                                     |

| 逻辑运算符 | 功能                         |
| ---------- | ---------------------------- |
| AND 或 &&  | 并且（多个条件同时成立）     |
| OR 或 \|\| | 或者（多个条件任意一个成立） |
| NOT 或 !   | 非，不是                     |



例子：



```plain
-- 年龄等于30
select * from employee where age = 30;
-- 年龄小于30
select * from employee where age < 30;
-- 小于等于
select * from employee where age <= 30;
-- 没有身份证
select * from employee where idcard is null or idcard = '';
-- 有身份证
select * from employee where idcard;
select * from employee where idcard is not null;
-- 不等于
select * from employee where age != 30;
-- 年龄在20到30之间
select * from employee where age between 20 and 30;
select * from employee where age >= 20 and age <= 30;
-- 下面语句不报错，但查不到任何信息
select * from employee where age between 30 and 20;
-- 性别为女且年龄小于30
select * from employee where age < 30 and gender = '女';
-- 年龄等于25或30或35
select * from employee where age = 25 or age = 30 or age = 35;
select * from employee where age in (25, 30, 35);
-- 姓名为两个字
select * from employee where name like '__';
-- 身份证最后为X
select * from employee where idcard like '%X';
```



#### 聚合查询（聚合函数）



常见聚合函数：

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 最大值   |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |



语法：
`SELECT 聚合函数(字段列表) FROM 表名;`
例：
`SELECT count(id) from employee where workaddress = "广东省";`



#### 分组查询



语法：
`SELECT 字段列表 FROM 表名 [ WHERE 条件 ] GROUP BY 分组字段名 [ HAVING 分组后的过滤条件 ];`



where 和 having 的区别：



- 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；having是分组后对结果进行过滤。

- 判断条件不同：where不能对聚合函数进行判断，而having可以。



例子：



```plain
-- 根据性别分组，统计男性和女性数量（只显示分组数量，不显示哪个是男哪个是女）
select count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性数量
select gender, count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性的平均年龄
select gender, avg(age) from employee group by gender;
-- 年龄小于45，并根据工作地址分组
select workaddress, count(*) from employee where age < 45 group by workaddress;
-- 年龄小于45，并根据工作地址分组，获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count from employee where age < 45 group by workaddress having address_count >= 3;
```



注意事项

- 执行顺序：where > 聚合函数 > having

- 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义



#### 排序查询



语法：
`SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;`



排序方式：



- ASC: 升序（默认）

- DESC: 降序



例子：



```plain
-- 根据年龄升序排序
SELECT * FROM employee ORDER BY age ASC;
SELECT * FROM employee ORDER BY age;
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employee ORDER BY age ASC, entrydate DESC;
```



注意事项

如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序



#### 分页查询



语法：
`SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;`



例子：



```plain
-- 查询第一页数据，展示10条
SELECT * FROM employee LIMIT 0, 10;
-- 查询第二页
SELECT * FROM employee LIMIT 10, 10;
```



注意事项



- 起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数

- 分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT

- 如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10



#### DQL执行顺序



FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT



### DCL



#### 管理用户



查询用户：



```plain
USER mysql;
SELECT * FROM user;
```



创建用户:
`CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`



修改用户密码：
`ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';`



删除用户：
`DROP USER '用户名'@'主机名';`



例子：



```plain
-- 创建用户test，只能在当前主机localhost访问
create user 'test'@'localhost' identified by '123456';
-- 创建用户test，能在任意主机访问
create user 'test'@'%' identified by '123456';
create user 'test' identified by '123456';
-- 修改密码
alter user 'test'@'localhost' identified with mysql_native_password by '1234';
-- 删除用户
drop user 'test'@'localhost';
```



注意事项

- 主机名可以使用 % 通配



#### 权限控制



常用权限：

| 权限                | 说明               |
| ------------------- | ------------------ |
| ALL, ALL PRIVILEGES | 所有权限           |
| SELECT              | 查询数据           |
| INSERT              | 插入数据           |
| UPDATE              | 修改数据           |
| DELETE              | 删除数据           |
| ALTER               | 修改表             |
| DROP                | 删除数据库/表/视图 |
| CREATE              | 创建数据库/表      |



更多权限请看[权限一览表](#权限一览表)



查询权限：
`SHOW GRANTS FOR '用户名'@'主机名';`



授予权限：
`GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';`



撤销权限：
`REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';`



注意事项

- 多个权限用逗号分隔

- 授权时，数据库名和表名可以用 * 进行通配，代表所有



## 函数



- 字符串函数

- 数值函数

- 日期函数

- 流程函数



### 字符串函数



常用函数：

| 函数                       | 功能                                                      |
| -------------------------- | --------------------------------------------------------- |
| CONCAT(s1, s2, ..., sn)    | 字符串拼接，将s1, s2, ..., sn拼接成一个字符串             |
| LOWER(str)                 | 将字符串全部转为小写                                      |
| UPPER(str)                 | 将字符串全部转为大写                                      |
| LPAD(str, n, pad)          | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
| RPAD(str, n, pad)          | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
| TRIM(str)                  | 去掉字符串头部和尾部的空格                                |
| SUBSTRING(str, start, len) | 返回从字符串str从start位置起的len个长度的字符串           |



使用示例：



```plain
-- 拼接
SELECT CONCAT('Hello', 'World');
-- 小写
SELECT LOWER('Hello');
-- 大写
SELECT UPPER('Hello');
-- 左填充
SELECT LPAD('01', 5, '-');
-- 右填充
SELECT RPAD('01', 5, '-');
-- 去除空格
SELECT TRIM(' Hello World ');
-- 切片（起始索引为1）
SELECT SUBSTRING('Hello World', 1, 5);
```



### 数值函数



常见函数：

| 函数        | 功能                             |
| ----------- | -------------------------------- |
| CEIL(x)     | 向上取整                         |
| FLOOR(x)    | 向下取整                         |
| MOD(x, y)   | 返回x/y的模                      |
| RAND()      | 返回0~1内的随机数                |
| ROUND(x, y) | 求参数x的四舍五入值，保留y位小数 |



### 日期函数



常用函数：

| 函数                               | 功能                                              |
| ---------------------------------- | ------------------------------------------------- |
| CURDATE()                          | 返回当前日期                                      |
| CURTIME()                          | 返回当前时间                                      |
| NOW()                              | 返回当前日期和时间                                |
| YEAR(date)                         | 获取指定date的年份                                |
| MONTH(date)                        | 获取指定date的月份                                |
| DAY(date)                          | 获取指定date的日期                                |
| DATE_ADD(date, INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1, date2)             | 返回起始时间date1和结束时间date2之间的天数        |



例子：



```plain
-- DATE_ADD
SELECT DATE_ADD(NOW(), INTERVAL 70 YEAR);
```



### 流程函数



常用函数：

| 函数                                                         | 功能                                                      |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| IF(value, t, f)                                              | 如果value为true，则返回t，否则返回f                       |
| IFNULL(value1, value2)                                       | 如果value1不为空，返回value1，否则返回value2              |
| CASE WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END    | 如果val1为true，返回res1，... 否则返回default默认值       |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END | 如果expr的值等于val1，返回res1，... 否则返回default默认值 |



例子：



```plain
select
	name,
	(case when age > 30 then '中年' else '青年' end)
from employee;
select
	name,
	(case workaddress when '北京市' then '一线城市' when '上海市' then '一线城市' else '二线城市' end) as '工作地址'
from employee;
```



## 约束



分类：

| 约束                    | 描述                                                     | 关键字      |
| ----------------------- | -------------------------------------------------------- | ----------- |
| 非空约束                | 限制该字段的数据不能为null                               | NOT NULL    |
| 唯一约束                | 保证该字段的所有数据都是唯一、不重复的                   | UNIQUE      |
| 主键约束                | 主键是一行数据的唯一标识，要求非空且唯一                 | PRIMARY KEY |
| 默认约束                | 保存数据时，如果未指定该字段的值，则采用默认值           | DEFAULT     |
| 检查约束（8.0.1版本后） | 保证字段值满足某一个条件                                 | CHECK       |
| 外键约束                | 用来让两张图的数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |



约束是作用于表中字段上的，可以再创建表/修改表的时候添加约束。



### 常用约束

| 约束条件 | 关键字         |
| -------- | -------------- |
| 主键     | PRIMARY KEY    |
| 自动增长 | AUTO_INCREMENT |
| 不为空   | NOT NULL       |
| 唯一     | UNIQUE         |
| 逻辑条件 | CHECK          |
| 默认值   | DEFAULT        |



例子：



```plain
create table user(
	id int primary key auto_increment,
	name varchar(10) not null unique,
	age int check(age > 0 and age < 120),
	status char(1) default '1',
	gender char(1)
);
```



### 外键约束



添加外键：



```plain
CREATE TABLE 表名(
	字段名 字段类型,
	...
	[CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
);
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);

-- 例子
alter table emp add constraint fk_emp_dept_id foreign key(dept_id) references dept(id);
```



删除外键：
`ALTER TABLE 表名 DROP FOREIGN KEY 外键名;`



#### 删除/更新行为

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致） |
| RESTRICT    | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与NO ACTION一致） |
| CASCADE     | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录 |
| SET NULL    | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（要求该外键允许为null） |
| SET DEFAULT | 父表有变更时，子表将外键设为一个默认值（Innodb不支持）       |



更改删除/更新行为：
`ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 行为 ON DELETE 行为;`



## 多表查询



### 多表关系



- 一对多（多对一）

- 多对多

- 一对一



#### 一对多



案例：部门与员工
关系：一个部门对应多个员工，一个员工对应一个部门
实现：在多的一方建立外键，指向一的一方的主键



#### 多对多



案例：学生与课程
关系：一个学生可以选多门课程，一门课程也可以供多个学生选修
实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键



#### 一对一



案例：用户与用户详情
关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率
实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的（UNIQUE）



### 查询



合并查询（笛卡尔积，会展示所有组合结果）：
`select * from employee, dept;`



笛卡尔积：两个集合A集合和B集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）



消除无效笛卡尔积：
`select * from employee, dept where employee.dept = dept.id;`



### 内连接查询



内连接查询的是两张表交集的部分



隐式内连接：
`SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;`



显式内连接：
`SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ...;`



显式性能比隐式高



例子：



```plain
-- 查询员工姓名，及关联的部门的名称
-- 隐式
select e.name, d.name from employee as e, dept as d where e.dept = d.id;
-- 显式
select e.name, d.name from employee as e inner join dept as d on e.dept = d.id;
```



### 外连接查询



左外连接：
查询左表所有数据，以及两张表交集部分数据
`SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ...;`
相当于查询表1的所有数据，包含表1和表2交集部分数据



右外连接：
查询右表所有数据，以及两张表交集部分数据
`SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ...;`



例子：



```plain
-- 左
select e.*, d.name from employee as e left outer join dept as d on e.dept = d.id;
select d.name, e.* from dept d left outer join emp e on e.dept = d.id;  -- 这条语句与下面的语句效果一样
-- 右
select d.name, e.* from employee as e right outer join dept as d on e.dept = d.id;
```



左连接可以查询到没有dept的employee，右连接可以查询到没有employee的dept



### 自连接查询



当前表与自身的连接查询，自连接必须使用表别名



语法：
`SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ...;`



自连接查询，可以是内连接查询，也可以是外连接查询



例子：



```plain
-- 查询员工及其所属领导的名字
select a.name, b.name from employee a, employee b where a.manager = b.id;
-- 没有领导的也查询出来
select a.name, b.name from employee a left join employee b on a.manager = b.id;
```



### 联合查询 union, union all



把多次查询的结果合并，形成一个新的查询集



语法：



```plain
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...
```



注意事项

- UNION ALL 会有重复结果，UNION 不会

- 联合查询比使用or效率高，不会使索引失效



### 子查询



SQL语句中嵌套SELECT语句，称谓嵌套查询，又称子查询。
`SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2);`
**子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个**



根据子查询结果可以分为：



- 标量子查询（子查询结果为单个值）

- 列子查询（子查询结果为一列）

- 行子查询（子查询结果为一行）

- 表子查询（子查询结果为多行多列）



根据子查询位置可分为：



- WHERE 之后

- FROM 之后

- SELECT 之后



#### 标量子查询



子查询返回的结果是单个值（数字、字符串、日期等）。
常用操作符：- < > > >= < <=



例子：



```plain
-- 查询销售部所有员工
select id from dept where name = '销售部';
-- 根据销售部部门ID，查询员工信息
select * from employee where dept = 4;
-- 合并（子查询）
select * from employee where dept = (select id from dept where name = '销售部');

-- 查询xxx入职之后的员工信息
select * from employee where entrydate > (select entrydate from employee where name = 'xxx');
```



#### 列子查询



返回的结果是一列（可以是多行）。



常用操作符：

| 操作符 | 描述                                   |
| ------ | -------------------------------------- |
| IN     | 在指定的集合范围内，多选一             |
| NOT IN | 不在指定的集合范围内                   |
| ANY    | 子查询返回列表中，有任意一个满足即可   |
| SOME   | 与ANY等同，使用SOME的地方都可以使用ANY |
| ALL    | 子查询返回列表的所有值都必须满足       |



例子：



```plain
-- 查询销售部和市场部的所有员工信息
select * from employee where dept in (select id from dept where name = '销售部' or name = '市场部');
-- 查询比财务部所有人工资都高的员工信息
select * from employee where salary > all(select salary from employee where dept = (select id from dept where name = '财务部'));
-- 查询比研发部任意一人工资高的员工信息
select * from employee where salary > any (select salary from employee where dept = (select id from dept where name = '研发部'));
```



#### 行子查询



返回的结果是一行（可以是多列）。
常用操作符：=, <, >, IN, NOT IN



例子：



```plain
-- 查询与xxx的薪资及直属领导相同的员工信息
select * from employee where (salary, manager) = (12500, 1);
select * from employee where (salary, manager) = (select salary, manager from employee where name = 'xxx');
```



#### 表子查询



返回的结果是多行多列
常用操作符：IN



例子：



```plain
-- 查询与xxx1，xxx2的职位和薪资相同的员工
select * from employee where (job, salary) in (select job, salary from employee where name = 'xxx1' or name = 'xxx2');
-- 查询入职日期是2006-01-01之后的员工，及其部门信息
select e.*, d.* from (select * from employee where entrydate > '2006-01-01') as e left join dept as d on e.dept = d.id;
```



## 事务



事务是一组操作的集合，事务会把所有操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。



基本操作：



```plain
-- 1. 查询张三账户余额
select * from account where name = '张三';
-- 2. 将张三账户余额-1000
update account set money = money - 1000 where name = '张三';
-- 此语句出错后张三钱减少但是李四钱没有增加
模拟sql语句错误
-- 3. 将李四账户余额+1000
update account set money = money + 1000 where name = '李四';

-- 查看事务提交方式
SELECT @@AUTOCOMMIT;
-- 设置事务提交方式，1为自动提交，0为手动提交，该设置只对当前会话有效
SET @@AUTOCOMMIT = 0;
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;

-- 设置手动提交后上面代码改为：
select * from account where name = '张三';
update account set money = money - 1000 where name = '张三';
update account set money = money + 1000 where name = '李四';
commit;
```



操作方式二：



开启事务：
`START TRANSACTION 或 BEGIN TRANSACTION;`
提交事务：
`COMMIT;`
回滚事务：
`ROLLBACK;`



操作实例：



```plain
start transaction;
select * from account where name = '张三';
update account set money = money - 1000 where name = '张三';
update account set money = money + 1000 where name = '李四';
commit;
```



### 四大特性ACID



- 原子性(Atomicity)：事务是不可分割的最小操作但愿，要么全部成功，要么全部失败

- 一致性(Consistency)：事务完成时，必须使所有数据都保持一致状态

- 隔离性(Isolation)：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行

- 持久性(Durability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的



### 并发事务

| 问题       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 一个事务读到另一个事务还没提交的数据                         |
| 不可重复读 | 一个事务先后读取同一条记录，但两次读取的数据不同             |
| 幻读       | 一个事务按照条件查询数据时，没有对应的数据行，但是再插入数据时，又发现这行数据已经存在 |



这三个问题的详细演示：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=55cd



并发事务隔离级别：

| 隔离级别              | 脏读 | 不可重复读 | 幻读 |
| --------------------- | ---- | ---------- | ---- |
| Read uncommitted      | √    | √          | √    |
| Read committed        | ×    | √          | √    |
| Repeatable Read(默认) | ×    | ×          | √    |
| Serializable          | ×    | ×          | ×    |



- √表示在当前隔离级别下该问题会出现

- Serializable 性能最低；Read uncommitted 性能最高，数据安全性最差



查看事务隔离级别：
`SELECT @@TRANSACTION_ISOLATION;`
设置事务隔离级别：
`SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE };`
SESSION 是会话级别，表示只针对当前会话有效，GLOBAL 表示对所有会话有效



# 进阶篇



## 存储引擎



MySQL体系结构：



![img](../assets/MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84_20220315034329549927.png)
![img](../assets/MySQL%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E5%B1%82%E7%BA%A7%E5%90%AB%E4%B9%89_20220315034359342837.png)



存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表而不是基于库的，所以存储引擎也可以被称为表引擎。
默认存储引擎是InnoDB。



相关操作：



```plain
-- 查询建表语句
show create table account;
-- 建表时指定存储引擎
CREATE TABLE 表名(
	...
) ENGINE=INNODB;
-- 查看当前数据库支持的存储引擎
show engines;
```



### InnoDB



InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB 是默认的 MySQL 引擎。



特点：



- DML 操作遵循 ACID 模型，支持**事务**

- **行级锁**，提高并发访问性能

- 支持**外键**约束，保证数据的完整性和正确性



文件：



- xxx.ibd: xxx代表表名，InnoDB 引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm、sdi）、数据和索引。



参数：innodb_file_per_table，决定多张表共享一个表空间还是每张表对应一个表空间



知识点：



查看 Mysql 变量：
`show variables like 'innodb_file_per_table';`



从idb文件提取表结构数据：
（在cmd运行）
`ibd2sdi xxx.ibd`



InnoDB 逻辑存储结构：
![img](../assets/%E9%80%BB%E8%BE%91%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84_20220316030616590001.png)



### MyISAM



MyISAM 是 MySQL 早期的默认存储引擎。



特点：



- 不支持事务，不支持外键

- 支持表锁，不支持行锁

- 访问速度快



文件：



- xxx.sdi: 存储表结构信息

- xxx.MYD: 存储数据

- xxx.MYI: 存储索引



### Memory



Memory 引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用。



特点：



- 存放在内存中，速度快

- hash索引（默认）



文件：



- xxx.sdi: 存储表结构信息



### 存储引擎特点

| 特点         | InnoDB              | MyISAM | Memory |
| ------------ | ------------------- | ------ | ------ |
| 存储限制     | 64TB                | 有     | 有     |
| 事务安全     | 支持                | -      | -      |
| 锁机制       | 行锁                | 表锁   | 表锁   |
| B+tree索引   | 支持                | 支持   | 支持   |
| Hash索引     | -                   | -      | 支持   |
| 全文索引     | 支持（5.6版本之后） | 支持   | -      |
| 空间使用     | 高                  | 低     | N/A    |
| 内存使用     | 高                  | 低     | 中等   |
| 批量插入速度 | 低                  | 高     | 高     |
| 支持外键     | 支持                | -      | -      |



### 存储引擎的选择



在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。



- InnoDB: 如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，则 InnoDB 是比较合适的选择

- MyISAM: 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那这个存储引擎是非常合适的。

- Memory: 将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory 的缺陷是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性



电商中的足迹和评论适合使用 MyISAM 引擎，缓存适合使用 Memory 引擎。



## 索引



索引是帮助 MySQL **高效获取数据**的**数据结构（有序）**。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查询算法，这种数据结构就是索引。



优缺点：



优点：



- 提高数据检索效率，降低数据库的IO成本

- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗



缺点：



- 索引列也是要占用空间的

- 索引大大提高了查询效率，但降低了更新的速度，比如 INSERT、UPDATE、DELETE



### 索引结构

| 索引结构            | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| B+Tree              | 最常见的索引类型，大部分引擎都支持B+树索引                   |
| Hash                | 底层数据结构是用哈希表实现，只有精确匹配索引列的查询才有效，不支持范围查询 |
| R-Tree(空间索引)    | 空间索引是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| Full-Text(全文索引) | 是一种通过建立倒排索引，快速匹配文档的方式，类似于 Lucene, Solr, ES |

| 索引       | InnoDB        | MyISAM | Memory |
| ---------- | ------------- | ------ | ------ |
| B+Tree索引 | 支持          | 支持   | 支持   |
| Hash索引   | 不支持        | 不支持 | 支持   |
| R-Tree索引 | 不支持        | 支持   | 不支持 |
| Full-text  | 5.6版本后支持 | 支持   | 不支持 |



#### B-Tree



![img](../assets/%E4%BA%8C%E5%8F%89%E6%A0%91_20220316153214227108.png)



二叉树的缺点可以用红黑树来解决：
![img](../assets/%E7%BA%A2%E9%BB%91%E6%A0%91_20220316163142686602.png)
红黑树也存在大数据量情况下，层级较深，检索速度慢的问题。



为了解决上述问题，可以使用 B-Tree 结构。
B-Tree (多路平衡查找树) 以一棵最大度数（max-degree，指一个节点的子节点个数）为5（5阶）的 b-tree 为例（每个节点最多存储4个key，5个指针）



![img](../assets/B-Tree%E7%BB%93%E6%9E%84_20220316163813441163.png)



B-Tree 的数据插入过程动画参照：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=68
演示地址：https://www.cs.usfca.edu/~galles/visualization/BTree.html



#### B+Tree



结构图：



![img](../assets/B+Tree%E7%BB%93%E6%9E%84%E5%9B%BE_20220316170700591277.png)



演示地址：https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html



与 B-Tree 的区别：



- 所有的数据都会出现在叶子节点

- 叶子节点形成一个单向链表



MySQL 索引数据结构对经典的 B+Tree 进行了优化。在原 B+Tree 的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的 B+Tree，提高区间访问的性能。



![img](../assets/%E7%BB%93%E6%9E%84%E5%9B%BE_20220316171730865611.png)



#### Hash



哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。
如果两个（或多个）键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。



![img](../assets/Hash%E7%B4%A2%E5%BC%95%E5%8E%9F%E7%90%86%E5%9B%BE_20220317143226150679.png)



特点：



- Hash索引只能用于对等比较（=、in），不支持范围查询（betwwn、>、<、...）

- 无法利用索引完成排序操作

- 查询效率高，通常只需要一次检索就可以了，效率通常要高于 B+Tree 索引



存储引擎支持：



- Memory

- InnoDB: 具有自适应hash功能，hash索引是存储引擎根据 B+Tree 索引在指定条件下自动构建的



#### 面试题



1. 为什么 InnoDB 存储引擎选择使用 B+Tree 索引结构？



- 相对于二叉树，层级更少，搜索效率高

- 对于 B-Tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针也跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低

- 相对于 Hash 索引，B+Tree 支持范围匹配及排序操作



### 索引分类

| 分类     | 含义                                                 | 特点                     | 关键字   |
| -------- | ---------------------------------------------------- | ------------------------ | -------- |
| 主键索引 | 针对于表中主键创建的索引                             | 默认自动创建，只能有一个 | PRIMARY  |
| 唯一索引 | 避免同一个表中某数据列中的值重复                     | 可以有多个               | UNIQUE   |
| 常规索引 | 快速定位特定数据                                     | 可以有多个               |          |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 | 可以有多个               | FULLTEXT |



在 InnoDB 存储引擎中，根据索引的存储形式，又可以分为以下两种：

| 分类                      | 含义                                                       | 特点                 |
| ------------------------- | ---------------------------------------------------------- | -------------------- |
| 聚集索引(Clustered Index) | 将数据存储与索引放一块，索引结构的叶子节点保存了行数据     | 必须有，而且只有一个 |
| 二级索引(Secondary Index) | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个         |



演示图：



![img](../assets/%E5%8E%9F%E7%90%86%E5%9B%BE_20220318194454880073.png)
![img](../assets/%E6%BC%94%E7%A4%BA%E5%9B%BE_20220319215403721066.png)



聚集索引选取规则：



- 如果存在主键，主键索引就是聚集索引

- 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引

- 如果表没有主键或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引



#### 思考题



\1. 以下 SQL 语句，哪个执行效率高？为什么？



```plain
select * from user where id = 10;
select * from user where name = 'Arm';
-- 备注：id为主键，name字段创建的有索引
```



答：第一条语句，因为第二条需要回表查询，相当于两个步骤。



\2. InnoDB 主键索引的 B+Tree 高度为多少？



答：假设一行数据大小为1k，一页中可以存储16行这样的数据。InnoDB 的指针占用6个字节的空间，主键假设为bigint，占用字节数为8.
可得公式：`n * 8 + (n + 1) * 6 = 16 * 1024`，其中 8 表示 bigint 占用的字节数，n 表示当前节点存储的key的数量，(n + 1) 表示指针数量（比key多一个）。算出n约为1170。



如果树的高度为2，那么他能存储的数据量大概为：`1171 * 16 = 18736`；
如果树的高度为3，那么他能存储的数据量大概为：`1171 * 1171 * 16 = 21939856`。



另外，如果有成千上万的数据，那么就要考虑分表，涉及运维篇知识。



### 语法



创建索引：
`CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (index_col_name, ...);`
如果不加 CREATE 后面不加索引类型参数，则创建的是常规索引



查看索引：
`SHOW INDEX FROM table_name;`



删除索引：
`DROP INDEX index_name ON table_name;`



案例：



```plain
-- name字段为姓名字段，该字段的值可能会重复，为该字段创建索引
create index idx_user_name on tb_user(name);
-- phone手机号字段的值非空，且唯一，为该字段创建唯一索引
create unique index idx_user_phone on tb_user (phone);
-- 为profession, age, status创建联合索引
create index idx_user_pro_age_stat on tb_user(profession, age, status);
-- 为email建立合适的索引来提升查询效率
create index idx_user_email on tb_user(email);

-- 删除索引
drop index idx_user_email on tb_user;
```



### 性能分析



#### 查看执行频次



查看当前数据库的 INSERT, UPDATE, DELETE, SELECT 访问频次：
`SHOW GLOBAL STATUS LIKE 'Com_______';` 或者 `SHOW SESSION STATUS LIKE 'Com_______';`
例：`show global status like 'Com_______'`



#### 慢查询日志



慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。
MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：
\# 开启慢查询日志开关
slow_query_log=1
\# 设置慢查询日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
更改后记得重启MySQL服务，日志文件位置：/var/lib/mysql/localhost-slow.log



查看慢查询日志开关状态：
`show variables like 'slow_query_log';`



#### profile



show profile 能在做SQL优化时帮我们了解时间都耗费在哪里。通过 have_profiling 参数，能看到当前 MySQL 是否支持 profile 操作：
`SELECT @@have_profiling;`
profiling 默认关闭，可以通过set语句在session/global级别开启 profiling：
`SET profiling = 1;`
查看所有语句的耗时：
`show profiles;`
查看指定query_id的SQL语句各个阶段的耗时：
`show profile for query query_id;`
查看指定query_id的SQL语句CPU的使用情况
`show profile cpu for query query_id;`



#### explain



EXPLAIN 或者 DESC 命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。
语法：
\# 直接在select语句之前加上关键字 explain / desc
EXPLAIN SELECT 字段列表 FROM 表名 HWERE 条件;



EXPLAIN 各字段含义：



- id：select 查询的序列号，表示查询中执行 select 子句或者操作表的顺序（id相同，执行顺序从上到下；id不同，值越大越先执行）

- select_type：表示 SELECT 的类型，常见取值有 SIMPLE（简单表，即不适用表连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION中的第二个或者后面的查询语句）、SUBQUERY（SELECT/WHERE之后包含了子查询）等

- type：表示连接类型，性能由好到差的连接类型为 NULL、system、const、eq_ref、ref、range、index、all

- possible_key：可能应用在这张表上的索引，一个或多个

- Key：实际使用的索引，如果为 NULL，则没有使用索引

- Key_len：表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好

- rows：MySQL认为必须要执行的行数，在InnoDB引擎的表中，是一个估计值，可能并不总是准确的

- filtered：表示返回结果的行数占需读取行数的百分比，filtered的值越大越好

## SQL优化

索引对于良好的性能非常关键，尤其是当表中的数据量越来越大时，索引对性能的影响愈发重要，在数据量较小且负载较低时，不恰当的索引对性能的影响可能还不明显，但当数据量逐渐增大时，性能则会急剧下降。

索引优化应该是查询性能优化最有效的手段了（可以起到立竿见影的效果），索引能够轻易将查询性能提高几个数量级，’最优’的索引有时比一个‘好的’索引性能要好两个数量级，创建一个真正’最优’的索引经常需要重写查询。

![img](../assets/1648457484199-1671ed06-b2a6-46a2-8ee7-fe484eafe69f-20220705211658747.png)

## 视图/存储过程/存储函数/触发器

# ![img](../assets/1648470665940-6595e836-a5d2-4b86-aa44-5ea474701f86.png)

### 视图

​    视图是一个虚拟表，其内容由查询定义。

​    同真实的表一样，视图包含一系列带有名称的列和行数据。

​    但是，视图并不在数据库中以存储的数据值集形式存在。

​    行和列数据来自由定义视图的查询所引用的表，并且在引用视图时动态生成。

​    视图具有表结构文件，但不存在数据文件。

​    对其中所引用的基础表来说，视图的作用类似于筛选。

​    定义视图的筛选可以来自当前或其它数据库的一个或多个表，或者其它视图。

​    通过视图进行查询没有任何限制，通过它们进行数据修改时的限制也很少。

​    视图是存储在数据库中的查询的sql语句，

​    它主要出于两种原因：安全原因，视图可以隐藏一些数据，

​    如：社会保险基金表，可以用视图只显示姓名，地址，而不显示社会保险号和工资数等，

​    另一原因是可使复杂的查询易于理解和使用。

-- 创建视图

CREATE [OR REPLACE] VIEW view_name [(column_list)] AS select_statement

​    \- 视图名必须唯一，同时不能与表重名。

​    \- 视图可以使用select语句查询到的列名，也可以自己指定相应的列名。

​    \- 可以指定视图执行的算法，通过ALGORITHM指定。

​    \- column_list如果存在，则数目必须等于SELECT语句检索的列数

-- 查看结构

​    SHOW CREATE VIEW view_name

-- 删除视图

​    \- 删除视图后，数据依然存在。

​    \- 可同时删除多个视图。

​    DROP VIEW [IF EXISTS] view_name ...

-- 修改视图结构

​    \- 一般不修改视图，因为不是所有的更新视图都会映射到表上。

​    ALTER VIEW view_name [(column_list)] AS select_statement

-- 视图作用

​    \1. 简化业务逻辑

​    \2. 对客户端隐藏真实的表结构

-- 视图算法(ALGORITHM)

​    MERGE       合并

​        将视图的查询语句，与外部查询需要先合并再执行！

​    TEMPTABLE   临时表

​        将视图执行完毕后，形成临时表，再做外层查询！

​    UNDEFINED   未定义(默认)，指的是MySQL自主去选择相应的算法。

### 存储过程

存储过程是一段可执行性代码的集合。相比函数，更偏向于业务逻辑。数据库SQL语言层面的代码封装和重用；

特点： 封装、复用； 可以接收参数，可以返回数据；减少网络交互，效率提升。

**创建：**

CREATE PROCEDURE 过程名 (参数列表)

BEGIN

​    过程体

END

**调用：**

CALL 过程名()

**删除：**

DROP PROCEDURE [IF EXISTS] 存储过程名



#### 变量

##### 系统变量

![img](../assets/1648468467746-acf2b2a9-f90a-4ef1-ae36-e36e123800bd.png)

##### 用户自定义变量

![img](../assets/1648468950288-3df58f6a-f269-4cd3-855c-4a9e422516cf.png)

##### 局部变量

![img](../assets/1648469132523-7cd7ba21-5d91-4f60-9afa-85af0c36be6b.png)

![img](../assets/1648469221444-c149b08a-adb3-4513-a0a4-78c069256fbf.png)



### 触发器

顾名思义，就是执行一些数据库语句之后，触发设定好的语句，一般是更新日志记录，数据校验之类的功能。

只支持行级触发，不支持语句触发。

![img](../assets/1648470011184-1a33ad95-0c61-4ebf-aa8c-e5baecf9b8e1.png)

![img](../assets/1648470072908-ce9cc8a5-46da-44cc-bb88-3fd0f29a6b4f.png)

## 锁

锁：协调并发访问的一种机制。保证数据并发访问的一致性。

MyISAM 采用**表级锁**

InnoDB 支持**行级锁和表级锁** , 默认为行级锁

### 全局锁：

锁定数据库中所有的表；加锁后整个数据库就处于只读状态，增删改之类的操作都会被阻塞。

主要的应用场景就是在对数据库做备份的时候，为了防止比如说备份表1时，表2被修改，导致数据不一致的情况，此时就会给全局加个锁，保证数据完整。

全局锁是一个比较重的操作，可能会导致一些问题：

1、如果在主库上备份，那在备份期间不能执行更新，除了读的所有操作都要被阻塞；

2、如果在从库上备份，那备份期间从库不能执行主库同步过来的binlog，会出现主从延迟问题。

### 表级锁： 

对当前操作的整张表加锁，资源消耗也比较少，加锁快，不会出现死锁。

其锁定粒度最大，触发锁冲突的概率最高，并发度最低，MyISAM 和 InnoDB 引擎都支持表级锁。

表级锁：

\1. **表锁** 分两类 表共享读锁/ 表独占写锁

读锁：其他客户端 只能读，不能写

写锁：其他客户端 不能读，不能写

语法：

加锁      lock tables 表名... read/write

释放锁  unlock tables /客户端断开连接

\2. **元数据锁**

系统自动控制的，不需要手动使用。在访问同一张表的时候会自动加上，来维护表元数据的一致性。

**3. 意向锁** 

 为了防止增删改操作的时候，加的行锁和表锁的冲突。比如说我们为一个表先加了一个行锁，另一个客户端为这个表试图加一个表锁，如果没有意向锁，表锁就需要每行去检查是否加了行锁，效率比较低。意向锁顾名思义，就是提前声明事务有意向对表中的某些行加锁 ，来减少表锁再去逐行检查。

意向锁分为：

意向共享锁 IS：事务有意向对表中的某些行加共享S锁 select...lock in share mode

意向排它锁 IX：事务有意向对表中的某些行加排它X锁 insert、update、delete、select...for update

### 行级锁： 

MySQL 中锁定 粒度最小 的一种锁，只针对当前操作的行进行加锁。 行级锁能大大减少数据库操作的冲突。其加锁粒度最小，并发度高，但加锁的开销也最大，加锁慢，会出现死锁。

InnoDB的数据是基于索引组织得到，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加的锁。

1. **行锁** 

锁定单个行，防止其他事务更删操作。（RR/RC隔离级别都支持）

两种类型的行锁：

1、共享锁（S）：

2、排他锁（X）：

1. **间隙锁**

锁定索引记录中的间隔，确保索引记录间隙不变，防止其他事务在这个间隙中插入，产生幻读。（在RR级别下都支持）。如果把事务的隔离级别降级为读提交(Read Committed, RC)，间隙锁则会自动失效。

1. **临键锁**

行锁与间隙锁的组合，锁住数据也锁住数据的间隙，RR隔离级别下支持。

如果一个会话占有了索引记录R的共享/排他锁，其他会话不能立刻在R之前的区间插入新的索引记录。

临键锁的主要目的，也是 为了避免幻读 。如果把事务的隔离级别降级为RC，临键锁则也会失效。

## InnoDB引擎

![img](../assets/1648539943289-b23273de-ef9b-45dd-b86d-f78623686362.png)

![img](../assets/1648523661405-02a3d081-6c34-4414-95b8-12dc923fe207-20220705211702573.png)

### 逻辑存储结构

![img](../assets/1648523243009-a94b0455-8ad6-40dd-8ccf-1caedbeac146-20220705211704206.png)



### InnoDB引擎的4大特性

- 插入缓冲（insert buffer)
- 二次写(double write)
- 自适应哈希索引(ahi)
- 预读(read ahead)

### InnoDB内存结构包含四大核心组件

- 缓冲池(Buffer Pool)
- 写缓冲(Change Buffer)
- 自适应哈希索引(Adaptive Hash Index)
- 日志缓冲(Log Buffer)

![img](../assets/1648523396369-2f56b558-f9f3-4e85-9eb6-84244619022b-20220705211703238.png)

![img](../assets/1648523472273-518c5d7d-c74c-4928-a2c3-87172d3467f5-20220705211704372.png)

![img](../assets/1648523551600-4f913f9b-59f2-4cc8-a032-62f7db56eef9-20220705211703402.png)

![img](../assets/1648523560644-7654239c-38ba-44d1-93d2-d613381ede2a-20220705211703576.png)

### MVCC

## MySQL管理

![img](../assets/1648541813496-05c902da-7aea-4a08-8416-1e86ba8d5101.png)

# 运维篇

## 日志

## 主从复制

## 分库分表

## 读写分离