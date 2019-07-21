> 运算符包括：算数运算符、比较运算符、逻辑运算符、位运算符

### 算数运算符

div和'/'一样，都是取商

mod和'%'一样，都是取余数

### 比较运算符

regexp 或 rlike 正则表达式匹配

使用格式：str regexp str_pat，当str中含有str_pat相匹配的字符串时返回1否则0。

示例：

```
select 'abcdef' regexp 'ab' ,'abcdefg' regexp 'k';
```
![](http://ww1.sinaimg.cn/large/a1963118ly1g4wbwtjtxuj20cq02qt8j.jpg)

### 逻辑运算符

![](http://ww1.sinaimg.cn/large/a1963118ly1g4wbztx65wj20n805adfu.jpg)

逻辑异或：当任意操作数为null，返回null。对于非null操作数，如果两个值不同返回1，否则返回0。

### 位运算符

位运算符是在二进制数上进行计算的运算符。位运算会先将操作数变成二进制数，进行位运算。然后再将计算结果从二进制数变回十进制数。

![](http://ww1.sinaimg.cn/large/a1963118ly1g4wc3278h9j20n707hmx7.jpg)