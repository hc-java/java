F6打开控制台

~~~mysql
//显示所有数据库
show databases; 
//进入该 数据库
use 数据库名；
//显示该数据库 所有表
show tables;
//显示 student 表 中所有字段
describe student;
~~~





# 创建表

~~~mysql
#创建表student
create table student(
id int auto_increment primary key,  //id 为主键且不允许为空
name varchar(50) unique,   //name 里面的数据不可重复
sex varchar(20) not null,  //不为空
age int unsigned, #不能为负值(如为负值 则默认为0)
date varchar(50),
content varchar(100)
)default charset=utf8;
 
~~~



## 插入数据

~~~mysql
insert into student values(null,'aa','男','1988-10-2','......');
~~~



## 查询数据

~~~mysql
select * from student;
select id,name from student;
~~~



## 修改数据

~~~mysql
update student set sex='女' where id=1;
~~~



## 删除数据

~~~mysql
delete from student where id=5;
~~~



# 修改表



~~~mysql
#修改表的名字
#格式:
alter table tbl_name rename to new_name
~~~

## 添加一个字段

~~~mysql
//向 student 表中添加一个叫 columnname 的字段
alter table student add  columnname varchar(20);
~~~

## 修改一个字段

~~~mysql
//在 student 表中 将一个 字段名 name，改为 uname，字符串大小也可以修改
alter table student change name uname varchar(50);
~~~

## 删除一个字段

~~~mysql
//删除 student 表中，名为 name 的字段
alter table student 
drop column name;
~~~





# 删除表

~~~mysql
drop table student;
~~~



# 查询表

查看student表结构

```mysql
describe student;  #可以简写为desc student;
```



## 升/降序

```mysql
#排序 asc 升序  desc 降序
select * from student order by id asc;
```





## 分组查询、聚合函数

```mysql
#分组查询 #聚合函数
select max(id),name,sex from student group by sex;
 
select min(date) from student;
 
select avg(id) as '求平均' from student;
 
select count(*) from student;   #统计表中总数
 
select count(sex) from student;   #统计表中性别总数  若有一条数据中sex为空的话,就不予以统计~
 
select sum(id) from student;
```

## 查询定制集合内数据

```mysql
#in 查询制定集合内的数据，查询出id是1，3，5的数据
select * from student where id in (1,3,5);

#查询第i条以后到第j条的数据(不包括第i条)
select * from student limit 2,5;  #显示3-5条数据
```



## **LIKE 的字符匹配查询**

~~~mysql
//  % 任意长度的字符串
SELECT * FROM student WHERE name LIKE ‘A%’;  
//  -  单个字符
SELECT * FROM student WHERE name LIKE ’A_y’; 
~~~



## 单表查询

~~~mysql
//查询出 user 表中，所有大于平均年龄的数据
select * from user where age > (select avg(age) from user);
~~~



## 多表查询

### 内连接

查询出 两张表公共的部分

~~~mysql
//格式： select * from 表1 inner join 表2 on 条件
select * from t_user inner join t_user_role on t_user.id=t_user_role.user_id;
~~~

### 外连接

#### 左外连接

优先显示左边的数据，右边如果没有对应的数据，则显示null;

~~~mysql
//格式： select * from 表1 left join 表2 on 条件
select * from t_user left join t_user_role on t_user.id=t_user_role.user_id;
~~~



#### 右外连接

优先显示右边的数据，左边如果没有对应的数据，则显示null；

~~~mysql
//格式：select * from 表1 right join 表2 on 条件
select * from t_user right join t_user_role on t_user.id=t_user_role.user_id;
~~~



#### 全外连接

mysql没有 full join

全外连接=左外连接+右外连接，可能会出现左边null 或者 右边 null

~~~mysql
select * from 表1  left join 表2 on 条件((表1.字段=表2.字段))
union
select * from 表1  right join 表2 on 条件((表1.字段=表2.字段));
~~~



# 外键

外键和主键也是索引的一种

作用：使得两张表关联，保证数据的一致性和实现一些级联操作

## 添加外键

 添加外键约束：alter table 从表 

​			  add constraint 外键（形如：FK_从表_主表） foreign key (从表外键字段) references 主表(主键字段);



```
语句中的(`)全部是Esc下面那个键而非单引号
'fk' 是外键名，命名规则一般是两个表的名字各取一部分（这里直接就叫 fk 了）
```

```mysql
//使 t_role_permission(从表) 中的 permission_id，作为 t_permission（主表） 中 id 的外键
alter table t_role_permission
add constraint  `fk` foreign key (`permission_id`) references t_permission(`id`);
```

~~~mysql


~~~

### 外键联动操作

on delete on update的联动操作有四种

no action

cascade

set null

restrict

ON DELETE

**restrict(约束)**:当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除。

**no action:**意思同restrict.即如果存在从数据，不允许删除主数据。

**cascade(级联):**当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则也删除外键在子表（即包含外键的表）中的记录。

**set null:**当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（不过这就要求该外键允许取null）

ON UPDATE

**restrict(约束)**:当在父表（即外键的来源表）中更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许更新。

**no action:**意思同restrict.

**cascade(级联):**当在父表（即外键的来源表）中更新对应记录时，首先检查该记录是否有对应外键，如果有则也更新外键在子表（即包含外键的表）中的记录。

**set null:**当在父表（即外键的来源表）中更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（不过这就要求该外键允许取null）。

注：NO ACTION和RESTRICT的区别：只有在及个别的情况下会导致区别，前者是在其他约束的动作之后执行，后者具有最高的优先权执行。

~~~mysql

//关联 update 级联操作，delete 约束操作
alter table t_role_permission 
add constraint fk foreign key (`permission_id`) references t_permission(`id`) on update cascade on delete no action;
~~~

小结：如果更新主表中的数据，从表对应的数据也会更新；不允许删除主表中的数据。如果改为 on delete cascade，则删除主表数据，对应的从表也会被删除

 

## 删除外键

~~~mysql
//删除 t_role_permission 表中叫 fk的外键
alter table t_role_permission
drop foreign key fk;

~~~

