`MySql`数据库是关系型数据库
`MySQL`数据库不区分大小写

在本文中出现的部分语法如 `goudan,dachui,cuihua`都**不是**固定语法可以起任意名称

### 增加

#### 添加数据库

```js
create database goudan; //goudan 是数据库的名字
```

```js
create database if not exists goudan;//创建数据库 不显示任何错误
```

#### 查看所有的数据库

````js
show database;
````

#### 创建使用`gbk`字符集

```js
create database dachui character set gbk;//使用gbk字符集的数据库
```

#### 带校验规则的

```js
create database cuihua character set gbk
collate gbk_chinese_ci;
```

#### 添加数据库对象

##### 选择数据库

```js
use goudan;//进入goudan数据库
```

##### 查看当前的数据库

```js
select database();
```

##### 创建表

必须选择一个数据库`use dachui;`

```js
create table goudan(//创建goudan表
    sno char(6) not null,//创建sno列 类型是char 长度是6 值不为空(也就是必须存在)
    sgender char(2) default '男',//sgender列 类型是char 长度是2 默认值是男
    sday datetime);//sday列 类型是datetime时间
```

##### 创建唯一值

```js
create table test(
    id int primary key auto_increment,//id列 自动增长 只能拥有一列 primary key 主键
    name char(10));
```

##### 设置主键

```js
create table goudan(
    sno char(6) primary key,//将sno这一列设置为主键
	sdata datetime);
```

主键**不能重复**
且不能为空

##### 创建组合主键

```js
create table dachui(
	sno char(6),
    cno char(6),
    primary key(sno,cno)//设置组合主键的方法
);
```

主键约束定义不在同一列时一列中的值**可以重复** 

##### 创建外键约束表

有些表需要有父子关系
用外键约束

```js
create table goudan(
    sno char(6),
    cno char(6),
    score decimal(4,1),
    foreign key(sno) references dachui(sno)//让自己的sno列参照dachui的sno列
);
```

#### 表数据的添加

##### 向所有的列表添加数据`insert`

```js
insert into goudan(sno,sname,sgender,sbirthday,sdept)//必须列出所有的字段顺序可以不一致
values('18110','大锤','女','2002-02-14 00:00:00','d12001');//值一一对应
```

##### 查看是否添加成功

```js
select * from goudan;
```

##### 省略字段名的方式添加

```js
insert into goduan
values('18111','潘安','男','2001-03-05 00:00:00','d12003');//因为没有字段名所以必须按照定义的顺序来添加
```

##### 表的指定字段添加值

```js
insert into goudan(sno,sname,sgender)
values('18112','狗蛋','男');//值一一对应
//值得注意的是当某个字段 为非空 并没有设置默认值那么添加值时 必须 为此字段添加数值否则 报错
```

##### 表的添加多条记录

```js
insert into goudan(sno,sname,sgender)
values('18113','高火','男'),('18114','张三','男'),('18115','李四','男');
```



### 查找

#### 查看某一个数据库

```js
show create database goudan;
```

#### 查看数据表

首先选中数据库`use goudan;`

```js
show tables;
```

#### 查看数据表结构

```js
show create table 数据表名;
```

#### 以二维表结构展示数据表

```js
describe 数据表名;//简写desc 数据表名
```

#### 数据查寻`select`

查询所有数值

```js
select * from goudan;//不建议 数据过多时消耗性能大
```

#### 查询指定字段的所有值

```js
selsct sname,sage from goudan;
```

#### 条件查询

```js
selsct sno,sname from goudan where sno='19102';//sno为学号sname为名称
```

#### in查询多条

```js
select sno,sname from goudan where sno in('18102','18103','18105');
```

查询学号不为特定值

```js
select sno,sname from goudan where not in('18102','18103');//只要加上not就可以
```

#### 查询区间范围内的值

```js
select sno,sname from goudan where sno between '值一' and '值二';
//查询的解构包含值一和值二
```

查询学号不为特定区间的值

```js
select sno,sname from goudan where sno not between '值一' and '值二';
```

#### 空值查询

```js
select sno,sname,sgender from goudan where sgender is null;
//查询sgender是否是null
```

非空值的查询

```js
select sno,sname,sgender from goudan where sgender is not null;
//sgender不为空的值
```

#### 查询记录不重复

```js
select distinct sgender from goudan;//distinct关键字为不重复
//查询sgender查询记录不重复
```

#### 字符串模糊查询

```js
select sno,sname from goudan where sname like '狗%';
//% 通配符 可以匹配任意长度的字符串 包括空字符串 可以代表任意字符
```

`_ 通配符 可以匹配任意长度的字符串 包括空字符串` 只能代表**一个**字符

```js
select * from goudan where sname like '王_锤';
```

#### `and`多条件查询

```js
select * from goudan where sno='18101' and sname='狗蛋';
//查询学号为18101并且名称是狗蛋的值
```

#### `or`多条件查询

```js
select * from goudan where sno<'18101' or sdept='d12001';
//or是 或者 的意思
```

| 关系运算符 | 说明     |
| ---------- | -------- |
| =          | 等于     |
| <>         | 不等于   |
| !=         | 不等于   |
| <          | 小于     |
| <=         | 小于等于 |
| >          | 大于     |
| >=         | 大于等于 |

#### 分类汇总查询

聚集函数

| 函数名  | 作用             |
| ------- | :--------------- |
| count() | 返回某列的行数   |
| sum()   | 返回某劣值的和   |
| avg()   | 返回某列的平均值 |
| max()   | 返回某列的最大值 |
| min()   | 返回某列的最小值 |

##### 查询一共有多少条记录

```js
select count(*) from goudan;//goudan数据表中有多少记录
```

##### 查询平均值

```js
select sname,avg(scorge) from goudan GROUP BY sname;
//查询goudan表中scorge的平均值
```

`goudan`表中`scorge`平均值大于80的所有学生`sno`的值

```js
select sno,avg(scorge) from goudan group by sno HAVING avg(scorge)>80;
```

#### 查询结果排序

`asc`升序, 默认升序
`desc`降序

```js
select sno,sname,sdept from goudan ORDER BY sno;
//查询goudan表按照sno进行排序
```

用`asc`进行排序

```js
select sno,sname,sdept from goudan order by sno asc;
```

用`desc`进行排序

```js
select sno,sname,sdept from goudan order by sno desc;
```

性别用降序学号用升序

```js
select sno,sname,sgender from goudan order by sgender desc, sno acs;
//开始用sgender来排序当sgender相同时用sno排序
```

#### 限制查询`limit`

```js
select sno,sname,sgender from goudan LIMIT 4;//显示表前四条记录
```

第五位到第八位 

```js
select sno,sname,sgender from goudan limit 4,4//第五位到第八位
```

#### 联合查询

`union`不包含重复行
`union all`包含重复行

```js
select cno,cname,sgender,sdept from goudan where sgender='女' 
UNION 
select cno,cname,sgender,sdept grom goudan where sdept='d12001';
```

#### 多表查询

交叉查询
存在大量的重复数据

```js
select * from dachui CROSS JOIN goudan;//展示dachui表和goudan表的所有数据
```

##### 内连接

```js
select * from dachui INNER JOIN goudan
ON dachui.snum=goudan.snum;//dachui表的snum等于goudan的snum的数据
//只有两个表snum相等的数据才出现在结果当中
```

表别名

```js
select s.sno,sname from goudan s INNER JOIN dachui d ON s.sno=d.sno;
//简化数据表的名称 但是 一旦用了别名不能用原名 否则报错
```

##### 外连接

```js
select sname,deptname from dachui d LEFT JOIN goudan g
ON d.deptname=g.deptname;//左外连接
```

以谁为标准谁就在左
右链接 查询结果包括**右表中**的**所有**记录和**左表中符合**链接**条件**的记录
**如果**右表中的**某条记录在左表不存在**,则在左表中显示`null`

```js
select sname,deptname from goudan g RIGHT JOIN dachui d
ON g.deptname=d.deptname;
```

##### 自连接

自己链接自己
一定要用表别名

```js
select work.name,work.num,student.name,student.num from goudan work INNER JOIN goudan student ON work.num=student.name;
```

#### 子查询

一个查询语句嵌套在另一个查询语句的内部

```js
select * from goudan
where sdate>(select sdate from goudan where name='大锤');
//先查询名字叫大锤的日期然后比较大小
```

子查询的参数

| 操作符 | 含义                 |
| ------ | -------------------- |
| in     | 等于列表中的某一个值 |
| any    | 与列表中的任意值比较 |
| all    | 与列表中的所有值比较 |

```js
select name from goudan
where num in (select num from dachui where num='210203');
```

#### 相关子查询

```js
select * from goudan
where EXISTS (select num from goudan where year(date)=2000);//查询是否有2000年出生的人
```

`EXISTS`查询的结果返回真和假
如果为真继续执行
如果为加则不执行外层

### 修改

#### 将字符集编码改为`utf-8`

```js
alter database dachui character set utf8;
```

#### 添加数据表字段

先进入数据库`use goudan;`

```js
alter table 表名称 add 字段名 varchar(20);//自动添加到最后一个
```

添加到固定位置

```js
alter table 表名称 add 字段名 char(20) after goudan;//放到goudan后面
//写 first 就是第一个字段
```

#### 修改字段名称

```js
alter table 表名称 change 字段名称 新名称 旧规则;
```

可以修改位置

```js
alter table 表名称 change 字段名称 新名称 旧规则 after 字段名称;//放在了所需的字段后面
```

#### 修改字段规则

```js
alter table 表名称 modify 字段名 新类型;
```

#### 重命名数据表

```js
alter table 表名称 rename 新名称;
```

#### 将表中的一列设置为主键

```js
alter table goudan//回车
add primary key(con);//con列改为主键
```

会自动检查是否满足条件
不满足条件设置失败

#### 删除主键约束

```js
alter table dachui
drop primary key;
```

只是删除了主键约束没有删除字段或字段组

#### 唯一性约束

这个表中的唯一性

```js
alter table goudan
add unique(cname);//将unique设置为表中的唯一字段
```

也就是在`cname`这一列中不能有重复的

#### 修改创建外键约束

在已经添加的表中增加外键约束

```js
alter table goudan
add constraint sc_con foreign key(cno) references dachui(cno);
//创建外键名为sc_con 把自己的cno列参照dachui表中的cno列进行约束
```

#### 删除外键约束

只是删除了外键约束
并没有删除结构
删除外键只需要删除外键名即可

```js
alter table goudan drop foreign key sc_cno;//删除goudan表中的外键sc_cno
```

#### 表中数据的修改`update`

修改所有数据

```js
update dachui
set sname='狗蛋';//将sname这一列全部修改为狗蛋
```

##### 修改部分数据

```js
update goudan
set sname='大锤'
where sno='18101';//where判断 sno等于18102的这一条数据
```

如果表中有多个值满足判断的条件那么将一起更改

### 删除

#### 删除数据库

```js
drop database goudan;
```

删除不存在的数据库会报错

```js
drop database if exists gouda;//添加了 if exists 这个可选项之后报错信息不显示
```

#### 删除数据库中的数据表

```js
drop table goudan;//goudan表
```

#### 删除数据表中的字段

```js
alter table 表名称 drop 字段名称;
```

#### 表中数据的删除`delete`

删除全部数据

```js
delete from goudan//删除goudan里的全部数据
```

##### 删除部分数据

```js
delete from goudan
where sno='18101' and con='110013';
//删除表中cno为18101的数据和con为110013的数据
```



## 索引

### 索引的优点

1. 保证数据表中的数据的唯一性
2. 加快数据表的检索速度
3. 加速表和表之间的联系
4. 使用分组和排序子句进行数据检索时减少检索时间
5. 在查询过程中使用优化隐藏器提高系统的性能

### 索引的缺点

1. 创建索引耗费时间且随着数据量的增加而增加
2. 创建索引占物理空间
3. 索引需要动态的维护降低了数据的维护速度

### 适合创建索引的条件

1. 经常需要搜索的列上面
2. 作为主键的列上面
3. 经常作为连接的列上面
4. 经常需要根据范围进行搜素的列上
5. 经常需要排序的列上
6. 经常使用`where`子句中的列上

### 主索引

必须在主键字段创建的索引

### 普通索引

有`key`或`index`定义的索引,`MySQL`中的基本索引类型

### 唯一索引

由`unique`定义的索引 该索引值所在的字段的值必须是唯一的

### 全文索引

由`fulltext`定义的索引 创建在字符或文本类型的字段上 **注意**只有`MySAM`存储引擎支持

### 单列索引

是指在表中单个字段上创建的索引

### 组合索引

在表中将多个字段创建一个组合索引
在组合索引中 查询条件只有使用多个字段中的第一个字段时该索引才会被使用
一个表可以有多个单例索引,但这不是组合索引,因为组合索引是由多个字段创建的一个索引

## 创建索引

### 创建普通索引

```js
create index dachui ON goudan(name);
//给goudan数据表中的name字段创建了dachui索引
//默认长度不限制 升序索引
```

### 创建唯一索引

```js
create UNIQUE index 索引名 ON 表名(字段名[(长度)] [ASC|DESC]);
```

```js
create unique index goudan ON dachui(sno DESC);
//给dachui数据表中的sno字段创建了goudan唯一索引 索引降序排列
```

### 创建全文索引

```js
create FULLTEXT index 索引名 ON 表名(字段名[(长度)] [ASC|DESC]);
```

```js
create fulltext index goudan2 ON dachui(address);
//将dachui数据表的address字段创建了全文索引goudan2
```

### 创建多列索引

```js
create index dachui on goudan(sno,sname);
//在goudan数据表中创建名为dachui的多列索引
```

### `alter table`创建索引

```js
alter table 表名 add [UNIQUE|FULLTEXT] index 索引名 (字段名[(长度)] [ACS|DESC]);
```

`alter tablr`一次只能创建多个索引 `create index` 一次只能创建一个
`alter tablr`可以不指定索引名,默认为第一列列名 `create index`必须指定索引名

## 索引的查看和删除

数据库中索引文件的查看

```js
show index from 数据库表名
```

### 索引的删除

语法一

```js
ALTER TABLE 表名 DROP INDEX 索引名;
```

语法二

```js
DROP INDEX 索引名 ON 表名;
```

## 视图

视图是一个或多个数据表中导出来的一个虚拟存在的数据表,表的结构和数据都依赖于源表

作用
查看源表中的数据;对源表数据进行增删改查操作

优点

1. 简化查询语句,数据所见即所得
2. 数据安全性,用户只能查询或修改所能见得到的数据
3. 逻辑独立性,可以屏蔽真是表结构变化带来的影响

缺点

1. 性能: 查询速度慢
2. 表的依赖关系: 更改相关联的表,必须更改视图

### 创建视图

```js
create VIEW 视图名[(字段名列表)] AS SELECT 语句;//如果指定某一特定的数据库需要 
//数据库名.视图名
```

创建名为`view_stu`的视图,将`goudan`数据库中的s`sno,sname,sdata`三列导入视图,并分别命名为`学号,姓名,出生日期`

```js
create view view_stu(学号,姓名,出生日期) as select sno,sname,sdata from goudan;
```

### 查看

```js
select * from 视图名;
```

### 查看视图定义

```js
DESC 视图名;//更直观
//或者
show CREATE view 视图名;//更详细
```

### 修改

```js
ALTER VIEW 视图名[(字段名)] AS SELECT 语句;
```

### 更新视图

```js
//插入记录
INSERT INTO 视图名(字段名列表) VALUES(值列表);
//删除记录
DELETE from 视图名[WHERE 条件表达式];
//更新部分数据
UPDATE 视图名 SET 字段名1=表达式1[,字段名2=表达式2,...][WHERE 条件表达式]
```

### 删除视图

```js
DROP VIEW [IF EXISTS] 视图名1[,视图名2,...];//如果不写 IF EXISTS 要保证用户指定删除的视图已存在 否则报错
```

