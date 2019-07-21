### 数值类型

##### 整数

![](http://ww1.sinaimg.cn/large/a1963118ly1g4v095twc4j20n70gdgn6.jpg)

在整数类型中，按照取值范围和存储方式不同，分为 tinyint、smallint、mediumint、int 和 bigint 5个类型。

如果超出类型范围，会提示"Out of range"错误提示。所以要根据应用实际情况确定取值范围，根据取值范围进行类型的选择。

对于整形数据，MySQL 还支持类型名称后的小括号内指定显示宽度，例如int(5)表示数值宽度小于5位时在数字前面用空格填满宽度，如果不显示指定宽度则默认为int(11)，一般配合zerofill使用，也就是如果位数不够则用 “0” 填充

```
create table t1(id1 int,id int(5);
insert into t1 vlaues(1,1);
```
查询结果如图：

![](http://ww1.sinaimg.cn/large/a1963118ly1g4v18utq3jj203e01u0ky.jpg)

修改id1列为zerofill
```
alter table t1 modify id1 int(11) zerofill;
```
再次查询：

![](http://ww1.sinaimg.cn/large/a1963118ly1g4v19khn37j204t0230sh.jpg)

修改列id为zerofill
```
alter table t1 modify id int(5) zerofill;
```

![](http://ww1.sinaimg.cn/large/a1963118ly1g4v17xjpe9j20500273y9.jpg)

如果id列插入数值超过5位是否会报错？
答案是不会，int类型的大小已经固定，()括号里的数值只是代表了填充宽度。

插入6位数字111111查询结果如下：

![](http://ww1.sinaimg.cn/large/a1963118ly1g4v1cn04e1j205602hjr5.jpg)

**UNSIGNED(无符号)**

所有整数类型都有一个可选属性UNSIGNET属性，如果在字段里保存非负数或需要较大的上限时，可以用此选项，它的取值范围下限是0，上限时原范围的2倍，例如tinyint有符号范围是-128-127，无符号范围是0-255.

如果一个列倍指定为zerofill，则MySQL自动为该列添加UNSIGNED属性。

**AUTO_INCREMENT(自增)**

在需要产生位移标识符或顺序值时可以使用此属，只适用于整数类型。

一个表中最多只能有一个AUTO_INCREMENT列，对于任何需要使用此属性的列，应该定义为NOT NULL,并定义为PRIMARY KEY(主键)或定义为UNIQUE(唯一)键。

示例：

```
create table ai(id int auto_increment not null primary key);
create table ai(id int auto_increment not null, primary key(id));
create table ai(id int auto_increment not null, unique(id));
```

##### 小数

小数包括浮点数和定点数

浮点数包括 float 和 double，定点数只有decimal一种

==**定点数在mysql中以字符串的形式存放比浮点数精度更加精确，适合用来表示货币等精度高的数据**==

浮点数和定点数都可以用类型名称后加"(M,D)"表示，代表该值一共显示M位数字(整数位+小数位)，其中D位于小数点后面，M和D又称为精度和标度。

例如float(7,4) 可以显示"-999.9999",mysql会采用四舍五入保存，因此如果插入999.00009，近似结果是999.0001，此用法非浮点数标准用法，因此不建议使用。

float和double在不指定精度使默认会按照实际精度(由实际的硬件和操作系统决定)来显示。而decimal在不指定精度时，默认的整数位为10，小数位为0。

BIT类型，用于存放位字段值：

bit(M) 可以用来存放多位2进制数，M范围从0~64，如果不写则默认1位，对于位字段，直接使用SELECT命令将不会看到结果，可以使用**bin()**(显示为2进制)或**hex()**(显示为16进制)函数进行读取
```
CREATE TABLE `t3` (
  `id` bit(1) DEFAULT NULL,
  `id1` bit(2) DEFAULT NULL
)

insert into t3 values(1,1);

select id,id1 from t3;
```
显示结果：
![](http://ww1.sinaimg.cn/large/a1963118ly1g4v2g4vsg8j203d0220lt.jpg)
```
select bin(id),bin(id1) from t3;
```
显示结果：
![](http://ww1.sinaimg.cn/large/a1963118ly1g4v2grrg9gj204y0213y9.jpg)

如果向列id中插入2，由于2对应二进制是10，而id列定义的是bit(1)，因此无法插入。

对于hex()同理，由2进制过度换为16进制即可

### 日期类型

![](http://ww1.sinaimg.cn/large/a1963118ly1g4vo851b51j20n608bmxu.jpg)

##### timestamp 探究
```
create table t7(id timestamp);

insert into t7 vlues(null);

select * from t7;
```

查询结果：![](http://ww1.sinaimg.cn/large/a1963118ly1g4w939mgglj2055025a9t.jpg)

mysql给timestamp类型的字段插入了系统默认当前时间，通过desc t7; 可以看到：

![](http://ww1.sinaimg.cn/large/a1963118ly1g4w94ovn8oj20ir024web.jpg)

**重点1**：首个timestamp列默认current_timestamp(当前系统时间)

**==系统为表中首个timestamp列设置了default current_timestamp(当前系统时间)，如果同一个表中有第二个或以上timestamp类型字段，则会默认为0值，可以通过语句：alter table t7 add id2 default current_timestamp on update current_timestamp;添加列的默认系统时间。常用于记录INSERT或UPDATE操作的时间戳==**

current_timestamp 通常在insert时不指定字段，默认插入系统时间

on update current_timestamp 通常在update时不管有没有显示指定列值，都会设为当前系统时间。

**重点2**：时区相关

当插入日期时，会先转换为本地时区后存放，从数据库中取出时，同样需要将日期转换为本地时区后显示，这样两个不同时区的用户看到的同一个日期可能不一样。

查看当前时区：
```
show variables like 'time_zone';
```
![](http://ww1.sinaimg.cn/large/a1963118ly1g4wac8qc6yj208m02ot8i.jpg)

**重点3**：timestamp取值范围是19700101080001到2038年的某一天，因此它不适合存放比较久远的日期。如果超过范围将会存入0值，如：0000-00-00 00:00:00

```
? timestamp
```
![](http://ww1.sinaimg.cn/large/a1963118ly1g4wamp0nxfj20fv0d53z5.jpg)


### 字符串类型

![](http://ww1.sinaimg.cn/large/a1963118ly1g4wan4kdj8j20n80c5dgq.jpg)

##### char和varchar类型

char和varchar类似，都用来保存较短字符串，他们的主要区别是存储方式不同。

vhar列长度为创建时声明的长度，范围是0~255；而varchar值为可变字符串，长度可以指定为0~255或65535之间的值。

检索的时候，char列删除了尾部空格，而varchar保留了这些空格。

创建两个列，分别为char(4)和varchar(4)并插入数值'ab   '

通过查询语句：

```
select concat(v,'+'),concat(c,'+') from vc;
```
![](http://ww1.sinaimg.cn/large/a1963118ly1g4waxomqvuj20b702m0sj.jpg)

可以看出char删除了尾部空格，而varchar保留了标度范围内的的空格。

##### binary和varbinary类型

binary和varbinary类似于char和varchar，不同的是它们保存的是二进制字符串。

```
create table t8(c binary(3));

insert into t8 set c = 'a';

select * ,hex(c),c='a',c='a\0',c='a\0\0' from t8;
```
![](http://ww1.sinaimg.cn/large/a1963118ly1g4wb5rryd5j20cm02idfn.jpg)

##### enum类型

美剧类型，取值范围在创建时通过美剧方式显示指定，对1~255个成员的美剧需要1个字节，对于255~65535个成员需要2个字节存储，最多允许有65535个成员。

示例：

```
create table t9(gender enum('M','F'));

insert into t9 values('M'),('l'),('f'),(null);

insert into t9 values('M'),('f')

select * from t9;
```
![](http://ww1.sinaimg.cn/large/a1963118ly1g4wbc0v83lj20eb012t8i.jpg)

如果插入不符合枚举规则的值，会报data truncated

![](http://ww1.sinaimg.cn/large/a1963118ly1g4wbda0sw7j209000zwe9.jpg)

![](http://ww1.sinaimg.cn/large/a1963118ly1g4wbdrza7cj205n036we9.jpg)

##### set类型

和enum类型，也是字符串对象，可以包含0~64个成员，根据成员不同存储也有所不同。

1~8个成员，占1字节 

9~16个成员，占2个字节

17~24个成员，占3个字节

25~32个成员，占4个字节

33~64个成员，占8个字节

set和enum最大区别在于set类型一次可以选取多个成员，而enum只能选取一个。

```
create table t10 (col set ('a','b','c','d'));

insert into t1 values('a,b'),('a,d,a'),('a,c'),('a');
```

![](http://ww1.sinaimg.cn/large/a1963118ly1g4wbkpz5amj205r03xa9u.jpg)

==注意：对于插入了重复值例如('a,d,a'),将只存储为'a,d'==


