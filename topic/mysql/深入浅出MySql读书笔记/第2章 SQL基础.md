##### DDL 数据定义语言

定义了数据库、表、列、索引等数据库对象。

修改表字段：
```
alter table emp modify name varchar(20);
```

修改字段名称：
```
alter table emp change age age1 int(4);
```

##### DML 数据操纵语句

用于添加、修改、删除和更新数据库记录，并检查数据完整性

LIMIT(startIndex,count) 
表示将在查询结果集中取从startIndex开始，取count个结果。

UNION 表示将两个查询结果集直接合并在一起

UNION ALL 表示将两个查询结果集合并后再 DISTINCT 去重

##### DCL 数据控制语句

用于控制不同数据段直接的许可和访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的语句关键词包括 grant、revoke 等。

创建一个用户z1，具有对test1数据库中所有表的SELECT/INSERT权限。

```
grant select,insert on test1.* from 'z1'@'localhost';
```

取掉用户 z1 对数据 test1  的 insert权限
```
revoke insert on test1.* from 'z1'@'localhost';
```

##### 帮助的使用

?content 可以显示可供查询的所有分类

例如 data types 数据类型

?data types 可以查看当前版本mysql都支持哪些数据类型

例如 BIGINT

?bigint 可以查看该数据类型的描述信息、取值范围、使用示例

?show; 查看show命令都能看哪些东西

?show create table emp; 查看emp的建表语句

?show table status 显示所有表名、存储引擎、数据行数等信息



