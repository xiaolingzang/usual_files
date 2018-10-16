## Hive的like和rlike

### LIKE
语法格式为**A [NOT] LIKE B**，B是sql下的简单正则表达式，也叫**通配符模式**.
```
查用通配符
% : 匹配0个或任意多个字符
_ : 匹配任意一个字符
escape ： 转义字符，可匹配%和_。如select "11111112222123333%_%" like "%\%\_\%"
```
举个例子：
SELECT name LIKE ‘%Alice’ FROM table1,表示选择name列内以ALICE作为结尾的数据

### RLIKE
语法格式为**A [NOT] RLIKE B**，基于java的**正则表达式**接口实现，如果A中有与B匹配则返回TRUE
```
. : 匹配任意单个字符
* ： 匹配0个或多个前一个得到的字符
[] : 匹配任意一个[]内的字符，[ab]*可匹配空串、a、b、或者由任意个a和b组成的字符串。
^ : 匹配开头，如^s匹配以s或者S开头的字符串。
$ : 匹配结尾，如s$匹配以s结尾的字符串。
{n} : 匹配前一个字符反复n次。
?: 匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 或 "does" 中的"do" 。? 等价于 {0,1}。
x|y:匹配 x 或 y。例如，'z|food' 能匹配 "z" 或 "food"。'(z|f)ood' 则匹配 "zood" 或 "food"
\d:匹配一个数字字符。等价于 [0-9]。

```
### 区别
通配符匹配的是整个列，比如helloworld就无法和’world’通配，但是正则表达式则是在列值内进行匹配，helloworld就可以和’world’匹配返回TRUE
另外，引入RLIKE该操作符的目的是引入正则表达式这样一个更加强大的语言来匹配条件，举个简单的对比例子
```
A RLIKE '.*(Alice|Ben).*' 
匹配包含Alice或者Ben的字段，使用LIKE的话需要用两个LIKE来做组合，下面是使用LIKE改成相同效果 
A LIKE '%Alice%' OR A LIKE '%Ben%'
```

