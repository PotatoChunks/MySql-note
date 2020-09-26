`MySql`数据库是关系型数据库
`MySQL`数据库不区分大小写

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



