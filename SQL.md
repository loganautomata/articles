# SQL语言

分为层次, 网状, 关系模型数据库.

>eg: MySQL为关系型数据库.

## 数据类型

| 名称         | 类型           | 说明                                                         |
| :----------- | :------------- | :----------------------------------------------------------- |
| INT          | 整型           | 4字节整数类型，范围约+/-21亿                                 |
| BIGINT       | 长整型         | 8字节整数类型，范围约+/-922亿亿                              |
| REAL         | 浮点型         | 4字节浮点数，范围约+/-1038                                   |
| DOUBLE       | 浮点型         | 8字节浮点数，范围约+/-10308                                  |
| DECIMAL(M,N) | 高精度小数     | 由用户指定精度的小数，例如，DECIMAL(20,10)表示一共20位，其中小数10位，通常用于财务计算 |
| CHAR(N)      | 定长字符串     | 存储指定长度的字符串，例如，CHAR(100)总是存储100个字符的字符串 |
| VARCHAR(N)   | 变长字符串     | 存储可变长度的字符串，例如，VARCHAR(100)可以存储0~100个字符的字符串 |
| BOOLEAN      | 布尔类型       | 存储True或者False                                            |
| DATE         | 日期类型       | 存储日期，例如，2018-06-22                                   |
| TIME         | 时间类型       | 存储时间，例如，12:20:59                                     |
| DATETIME     | 日期和时间类型 | 存储日期+时间，例如，2018-06-22 12:20:59                     |

## 关系模型

### 主键

主键是用来定位记录, 不可再修改, 完全与业务无关.

#### 自增类型

自动为新纪录分配自增主键.

```sql
primary_key_name bigint not null auto_increment
```

#### 全局唯一GUID类型

使用GUID算法计算出主键.

#### 联合主键

使用多个字段组合来作为主键.

### 外键

外键将一个表通过一对一, 一对多或多对多的关系关联到另一个表. 使用外键约束会降低数据库性能, 一般不使用.

```sql
alter table table_name
add constraint constraint_name
foreign key (foreign_key_name)
references constraint_table_name (main_key_name);
```

删除外键约束.

```sql
alter table table_name
drop foreign key (constraint_name);
```

> 只是删除了外键约束, 并未删除外键这一列.

### 索引

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构.

> 使用索引使得查找时不必遍历整个表, 加速查找.

```sql
alter table table_name
add index index_name (column1_name, column2_name...);
```

有些列具有唯一性故需要增加唯一索引.

```sql
alter table table_name
add unique index index_name (column_name);
```

也可以只添加唯一约束, 不添加索引.

```sql
alter table table_naem
add constraint constraint_name unique (column_name);
```

## 查

查询整表:

```sql
select * from table_name;
```

> 可以给某表取别名`select * from table_name new_name`.

查询某几列:

```sql
select column1_name, column2_name from table_name;
```

> 可以给某列取别名`select column_name new_name from table_name`.

查询多表:

```sql
select * from table1_name, table2_name;
```

> 这会将table1和table2进行两两组合, 最后返回的记录条数为表1记录条数和表2记录条数的乘积.
>
> 也可以通过`table_name.column_name`的方法使得结果集返回某列或某几列.

### 条件查询

```sql
select * from table_name where condition1 operator condition2 operator...;
```

#### operator

##### and

与.

##### or

或.

##### not

非.

##### condition中的运算符

| 符号     | 说明                       |
| ------- | -------------------------- |
| =       | 等于                     |
| <>      | 不等于                     |
| >       | 大于                       |
| <       | 小于                       |
| >=      | 大于等于                   |
| <=      | 小于等于                   |
| between | 在某个范围内               |
| like    | 搜索某种模式               |
| in      | 指定针对某个列的多个可能值 |

>1. `!=`只在一部分数据库可以被识别, 如MySQL, 推荐使用`<>`.
>2. `between num1 and num2`表示num1~num2.
>3. `name like`'百合%' 表示查询姓百合的人, %为通配符.
>4. `name in`('nier', 'sagiri') 表示查询姓名为nier和sagiri的人.

### 排序

正序:

```sql
select * from table_name order by column_name asc;
```

> asc可以省略.

反序:

```sql
select * from table_name order by column_name desc;
```

多重排序:

```sql
select * from table_name order by column1_name column2_name;
```

>按colunmn1_name排完序后若有相同的项, 再按column2_name排序.

> `order by`语句必须放在`where`语句之后.

### 分组

```sql
select column1_name, column2_name group by column1_name, column2_name;
```

> 按某几列分组.

### 分页

```sql
limit page_size offset page_offset;
```

> page_index = page_size * (page_index - 1).
>
> offset超过最大页数即会返回空集.

### 聚合

```sql
select function_name(column_name) from table_name;
```

#### function

##### count

参数为*或列名都可以, 都返回表中行数.

> 可以配合分组来查询每组人数.

##### sum

参数为列名, 该列必须为数值类型, 返回该列合计值.

##### avg

参数为列名, 该列必须为数值类型, 返回该列平均值.

##### max

参数为列名, 返回该列最大值.

##### min

参数为列名, 返回该列最小值.

### 连接

#### 内连接

```sql
select t1.c1, t2.c1, t2.c2
from table1_name t1
inner join table2_name t2
on condition;
```

> `inner join`后跟需要连接的表名, `on`后跟返回结果集条件, eg:`t1.c1=t2.c1`, 结果集返回的是两表都存在的记录.

#### 外连接

```sql
select t1.c1, t2.c1, t2.c2
from table1_name t1
full outer join table2_name t2
on condition;
```

> 结果集中包含两张表中t1存在t2不存在和t1不存在t2存在的记录.

##### 左连接

```sql
select t1.c1, t2.c1, t2.c2
from table1_name t1
left outer join table2_name t2
on condition;
```

> 返回左表即t1存在的记录, 若t2不存在则将结果集中t2相关列置为null.

##### 右连接

```sql
select t1.c1, t2.c1, t2.c2
from table1_name t1
right outer join table2_name t2
on condition;
```

> 返回右表即t2存在的记录, 若t1不存在则将结果集中t1相关列置为null.

## 增

### insert to ... values ...

```sql
insert to table_name (column1_name, column2_name) values
(column1_value1, column2_value2)
(column1_value3, column2_value4)
...; 
```

> 可以只插入1行, 也可以插入多行记录.

## 改

### update ... set ... where ...

```sql
update table_name set column1_name = value1, column2_name = value2 where condition;
```

## 删

### delete from ... where ...

```sql
delete from table_name where condition;
```

