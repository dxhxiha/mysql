## mysql

创建库名和表名、不允许和命令冲突，否则建立失败。

修改表的操作

1.修改表名

```mysql
alter  table 表名 rename  [to]   newname

alter table example2 rename to alter_test.example10;   加上to可以实现数据表在库与库之间的迁移，并且可以在迁移过程中修改表名。
```

2.修改字段的数据类型

```mysql
alter table 表名 modify 字段  新数据类型  [约束条件]

注意：如果在表中存留有数据，有可能导致因为修改数据类型而造成的数据丢失问题。

```

3.修改字段名

```mysql
alter table 表名 change  旧字段名 新字段名 新数据类型  [约束条件]

alter table example10 change address s_address varchar(10);
```

4.添加字段

```mysql
alter table 表名 add 新字段名 新数据类型 [约束条件] [first|after 已经存在的字段]

添加新字段，名为phone 数据类型为int ，添加到name的前面
alter table  example10 add phone int after id ;
```

当有密码时，清空密码

mysqladmin -u root  -p  密码  password  新密码

当没有密码时，设置密码



5删除字段

```mysql
alter table 表名 drop 字段名
注意L:删除字段时，该字段的数据 ，随着字段的删除而消失
```

6.修改存在字段位置

```mysql
alter table 表名 modify 字段名 数据类型 first|after 字段名

字段地排列顺序，决定了inert写入数据是所对应的数据顺序，因此字段的位置，决定字段的顺序决定在使用
```

7.修改表的存储引擎

```mysql
alter table  表名 engine=要修改的引擎名
```

8.删除数据表

```mysql 
drop table [if exists]  表名   加上if exists大部分情况下直接在脚本里面使用，如果不加入if exists 在表不存在的情况下 返回error，加上以warning形式返回


```

create table student(id int primary key , name varchar(30) not null,address char(50) not null ,sex char(2) not null, brithday date );

create table score(id int primary key,course_id int not null , grade decimal(8.2),constraint fk_sc foreign key(id ) references student(id));

9.删除外键约束关系

```mysql
alter table 表名  drop  foreign key 约束名称
```

10 删除约束条件

（1）删除主键

```mysql
alter table 表名 drop primary key
```

（2）删除唯一性约束条件

```mysql
alter table 表名 drop  key 约束名
```

（3）删除外键

```mysql
alter table  表名 drop 
```

（4）删除自增、非空、默认值

```mysql
alter table 表名 modify（change）  只要不带有（非空约束|自增|默认值）条件相当于删除。 
```



```mysql
添加：在mysql中主键和唯一性约束条件可以通过modify、change、和add进行添加（上述两种约束条件都有连中创建的语法，modify对应的跟在数据类型后面添加，add对应另起一行进行添加）。
非空、自增、默认值约束条件只能用modify或者change 进行添加，对应的语法中的跟在数据类型后面进行写入的方式

外键作为较为特殊的约束条件只能使用add来进行添加
```



如果当前库太多，忘记表在哪个库“

```mysql 
select TABLE_SCHEMA FROM TABLES WHERE TABLE_NAME='表名';

注意：information_schema作为mysql自带数据库存放了所有数据库以及标的信息，只需要在该库中找到tables有TABLE_NAME选项即可得知要查询的表在哪个库中，如果多个库都有同名表，只需要切换到指定的库查看即可得知。
```



如果当前表太多，忘记外键在那个表

```mysql
查询外键
select * from TABLE_CONSTRAINTS where CONSTRAINT_TYPE='FOREIGN KEYG'\G
查询唯一性约束
select * from TABLE_CONSTRAINTS where CONSTRAINT_TYPE='UNIQUE'\G
查询外键
select * from TABLE_CONSTRAINTS where CONSTRAINT_TYPE=PRIMARY KEYG


注意：information_schema库中有个名为TABLE_CONSTRAINTS的表里存放了所有表中的约束条件，包含主键，外键，唯一性约束条件，需要查看那个类型的约束条件将上述中foreign key进行替换即可

```







## 将数据库sjk1223改成sjk1226

```mysql
#!/bin/bash
mysql -uroot -p123.com -e 'create database if not exists sjk1226'
list_table=$(mysql  -uroot -p123.com -Nse "select table_name from information_schema.TABLES where TABLE_SCHEMA='sjk1223'")
for table in $list_table
do
        mysql -uroot -p123.com -e "alter table sjk1223.$table rename to sjk1226.$table"
done

```

