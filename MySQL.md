# MySQL

## 列出数据库

```mysql
show databases;
```

## 新建数据库

```mysql
create database database_name;
```

## 删除数据库

```mysql
drop database database_name;
```

## 进入数据库

```mysql
use database -u root -p;
```

> 参数u: 用户名.
>
> 参数p: 密码.

## 列出所有表

```mysql
show tables;
```

## 列出表结构

```mysql
desc thisTable_name;
```

## 查看创建表的SQL语句

```mysql
show create table thisTable_name;
```

## 新建表

```mysql
create table thisTable_name(
    `id` bigint(1) unsigned auto_increment primary key not null,
    `username` varchar(13) not null default 'Nier' unique,
    `password` char(17) not null
) engine=innodb, auto_increment=1, default charset=utf8, collate=utf8_bin;
```

## 删除表

```mysql
drop table thisTable_name;
```

## 修改表

### 增加列

```mysql
alter table thisTable_name add column thisColumn_name bigint(100) not null;
```

### 删除列

```mysql
alter table thisTable_name drop column thisColumn_name;
```

### 修改列

```mysql
alter table thisTable_name change column thisColumn_name newColumn_name varchar(100) not null;
```

## 插入或替换列

```mysql
replace into thisTable_name (column1_name, column2_name) values (column1_value1, column2_value2);
```

## 插入或忽略列

```mysql
insert ignore into thisTable_name (column1_name, column2_name) values (column1_value1, column2_value2);
```

## 插入或更新列

```mysql
insert into thisTable_name (column1_name, column2_name) values (column1_value1, column2_value2) on duplicata key update column2_name=column2_value2;
```

## 插入行

```mysql
insert into table_name (column1_name, column2_name)
values
(value1, value2);
```

## 插入结果集

```mysql
insert into newTable_name (column1_name, column2_name) select (column1_name, func(column3_name)) from thisTable_name group by column1_name;
```

## 结果集的快照

```mysql
create table newTable_name select * from thisTable_name where thisCondition;
```

## 强制使用指定索引

```mysql
force index (index_name);
```

## 修改字符集

### 修改数据库字符集

```mysql
alter database database_name character set utf8 collate utf8_bin;
```

>collate后跟排序规则, 可选项.  
utf8_bin将字符串中的每一个字符用二进制数据存储, 区分大小写.  
utf8_general_ci不区分大小写, ci为case insensitive的缩写, 大小写不敏感.  
utf8_general_cs区分大小写, cs为case sensitive的缩写, 大小写敏感.

### 修改表字符集

```mysql
alter table table_name character set utf8 collate utf8_bin;
```

### 修改列字符集

```mysql
alter table table_name modify column column_name varchar(30) character set utf8 collate utf8_bin;
```

### 修改表中所有列字符集

```mysql
alter table table_name convert to character set utf8 collate utf8_bin;
```

## 唯一索引

### 单列添加

```mysql
alter table table_name add unique key(`column_name`);
```

## 外键

### 添加外键约束

```mysql
ALTER TABLE this_table_name ADD CONSTRAINT foreign_key_constraint FOREIGN KEY (this_table_column) REFERENCES that_table_name (that_table_column);
```

### 删除外键约束

```mysql
ALTER TABLE this_table_name DROP FOREIGN KEY foreign_key_constraint;
```
