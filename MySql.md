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
describe 数据表名;
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
drop table goudan;
```

#### 删除数据表中的字段

```js
alter table 表名称 drop 字段名称;
```



