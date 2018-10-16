#linux awk

- [linux awk命令简介](#linux_awk命令简介)
	- [语法](#语法)
	- [参数选项说明](#参数选项说明)
- [基本用法](#基本用法)
- [运算符](#运算符)
- [内建变量](#内建变量)
- [awk脚本](#awk脚本)
- [awk 的if、循环和数组](#awk的if、循环和数组)
	- [条件语句](#条件语句)
	- [循环结构](#循环结构)
		- [for循环](#for循环)
		- [while循环](#while循环)
		- [break 和 continue](#break和continue)
- [awk内置字符串函数](#awk内置字符串函数)

## linux awk命令简介
AWK是一种处理文本文件的语言，是一个强大的文本分析工具

### 语法
```
awk [选项参数] 'script' var=value file(s)
awk [选项参数] -f scriptfile var=value file(s)
```
### 参数选项说明
- -F fs or --field-separator=fs
指定输入文件折分隔符，fs是一个字符串或者是一个正则表达式，如-F:。

- -v var=value or --asign=var=value
赋值一个用户定义变量。

- -f scripfile or --file=scriptfile
从脚本文件中读取awk命令。

- -W compact or --compat, -W traditional or --traditional  
在兼容模式下运行awk。所以gawk的行为和标准的awk完全一样，所有的awk扩展都被忽略。

- -W copyleft or --copyleft, -W copyright or --copyright
打印简短的版权信息。

- -W help or --help, -W usage or --usage
打印全部awk选项和每个选项的简短说明。

- -W lint or --lint
打印不能向传统unix平台移植的结构的警告。

- -W re-interval or --re-inerval
允许间隔正则表达式的使用，参考(grep中的Posix字符类)，如括号表达式[[:alpha:]]。

- -W posix
打开兼容模式。但有以下限制，不识别：/x、函数关键字、func、换码序列以及当fs是一个空格时，将新行作为一个域分隔符；操作符`**`和`**=`不能代替^和^=；fflush无效。

- -W source program-text or --source=program-text
使用program-text作为源代码，可与-f命令混用。

- -W version or --version
打印bug报告信息的版本。

## 基本用法
log.txt文本内容如下：  
```
2 this is a test
3 Are you like awk
This's a test
10 There are orange,apple,mongo
```

- 用法一：  

```
awk '{[pattern] action}' {filenames} # 行匹配语句 awk '' 只能用单引号
```

实例：

```
# 每行按空格或TAB分割，输出文本中的1、4项
 $ awk '{print $1,$4}' log.txt
 ---------------------------------------------
 2 a
 3 like
 This's
 10 orange,apple,mongo
 # 格式化输出
 $ awk '{printf "%-8s%-10s\n",$1,$4}' log.txt
 2        a
 3        like
 This's
 10       orange,apple,mongo
```

- 用法二


```
awk -F  #-F相当于内置变量FS, 指定分割字符
```

实例：

```
# 使用","分割
$  awk -F, '{print $1,$2}'   log.txt
 2 this is a test
 3 Are you like awk
 This's a test
 10 There are orange apple
# 或者使用内建变量
 $ awk 'BEGIN{FS=","} {print $1,$2}'     log.txt
 ---------------------------------------------
 2 this is a test
 3 Are you like awk
 This's a test
 10 There are orange apple
# 使用多个分隔符.先使用空格分割，然后对分割结果再使用","分割
 $ awk -F '[ ,]'  '{print $1,$2,$5}'   log.txt
 ---------------------------------------------
 2 this test
 3 Are awk
 This's a
 10 There apple
```

- 用法三：

```
awk -v  # 设置变量
```

实例：

```
 $ awk -v a=1 '{print $1,$1+a}' log.txt
 ---------------------------------------------
 2 3
 3 4
 This's 1
 10 11
 $ awk -v a=1 -v b=s '{print $1,$1+a,$1b}' log.txt
 ---------------------------------------------
 2 3 2s
 3 4 3s
 This's 1 This'ss
 10 11 10s
```
- 用法四

```
awk -f {awk脚本} {文件名}
```
实例：

```
 $ awk -f cal.awk log.txt
```

## 运算符  
| 运算符 | 描述 |
| :--------|:-------------        |
| = += -= *= /= %= ^= **= | 赋值 |
| ?: | C条件表达式 |
| || | 逻辑或 |
| && | 逻辑与 |
| ~ ~! | 匹配正则表达式和不匹配正则表达式 |
| < <= > >= != == | 关系运算符 |
| 空格 | 连接 |
| + - | 加，减 |
| * / % | 乘，除与求余 |
| + - ! | 一元加，减和逻辑非 |
| ^ *** | 求幂 |
| ++ --| 增加或减少，作为前缀或后缀 |
| $ | 字段引用 |
| in | 数组成员 |

实例1：
```
#过滤第一列大于2的行
$ awk '$1>2' log.txt    #命令
#输出
3 Are you like awk
This's a test
10 There are orange,apple,mongo
#过滤第一列等于2的行
$ awk '$1==2 {print $1,$3}' log.txt    #命令
#输出
2 is
# 过滤第一列大于2并且第二列等于'Are'的行
$ awk '$1>2 && $2=="Are" {print $1,$2,$3}' log.txt    #命令
3 Are you
```
实例二：使用正则，字符串匹配

```
# 输出第二列包含 "th"，并打印第二列与第四列
$ awk '$2 ~ /th/ {print $2,$4}' log.txt
---------------------------------------------
this a
```
注：~ 表示模式开始。// 中是匹配模式。

```
# 输出包含"re" 的行
$ awk '/re/ ' log.txt
---------------------------------------------
3 Are you like awk
10 There are orange,apple,mongo
```

实例三：模式取反
```
$ awk '$2 !~ /th/ {print $2,$4}' log.txt
---------------------------------------------
Are like
a
There orange,apple,mongo
$ awk '! /th/ {print $2,$4}' log.txt
---------------------------------------------
Are like
a
There orange,apple,mongo
```
## 内建变量

| 变量 | 描述 |
| :---------| :-----------|
| **$n** | 当前记录的第n个字段，字段间由FS分隔 |
| **$0** | 完整的输入记录 |
| ARGC | 命令行参数的数目 |
| ARGIND | 命令行中当前文件的位置(从0开始算) |
| ARGV | 包含命令行参数的数组 |
| CONVFMT | 数字转换格式(默认值为%.6g)ENVIRON环境变量关联数组 |
| ERRNO | 最后一个系统错误的描述 |
| FIELDWIDTHS | 字段宽度列表(用空格键分隔) |
| **FILENAME** | 当前文件名 |
| FNR | 各文件分别计数的行号 |
| **FS** | 字段分隔符(默认是任何空格) |
| **IGNORECASE** | 如果为真，则进行忽略大小写的匹配 |
|** NF** | 一条记录的字段的数目,就是有多少列 |
| **NR** | 已经读出的记录数，就是行数，从1开始 |
| OFMT | 数字的输出格式(默认值是%.6g) |
| **OFS** | 输出字段分隔符（输出换行符），输出时用指定的符号代替换行符 |
| **ORS** | 输出记录分隔符(默认值是一个换行符) |
| RLENGTH | match函数所匹配的字符串的长度 |
| **RS**	| 输入记录分隔符(默认是一个换行符)|
| RSTART | 由match函数所匹配的字符串的第一个位置 |
| SUBSEP | 数组下标分隔符(默认值是/034) |

实例一：
```
$ awk 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt
FILENAME ARGC  FNR   FS   NF   NR  OFS  ORS   RS
---------------------------------------------
log.txt    2    1         5    1
log.txt    2    2         5    2
log.txt    2    3         3    3
log.txt    2    4         4    4
$ awk -F\' 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt
FILENAME ARGC  FNR   FS   NF   NR  OFS  ORS   RS
---------------------------------------------
log.txt    2    1    '    1    1
log.txt    2    2    '    1    2
log.txt    2    3    '    2    3
log.txt    2    4    '    1    4
# 输出顺序号 NR, 匹配文本行号
$ awk '{print NR,FNR,$1,$2,$3}' log.txt
---------------------------------------------
1 1 2 this is
2 2 3 Are you
3 3 This's a test
4 4 10 There are
# 指定输出分割符
$  awk '{print $1,$2,$5}' OFS=" $ "  log.txt
---------------------------------------------
2 $ this $ test
3 $ Are $ awk
This's $ a $
10 $ There $
```

实例：字段分隔符

```
#FS="\t+" 一个或多个 Tab 分隔
$ cat tab.txt
ww   CC        IDD
$ awk 'BEGIN{FS="\t+"}{print $1,$2,$3}' tab.txt
ww   CC        IDD
#FS="[[:space:]+]" 一个或多个空白空格，默认的 
$ cat space.txt
we are    studing awk now!
$ awk -F [[:space:]+] '{print $1,$2}' space.txt
we are
# FS="[" ":]+" 以一个或多个空格或：分隔 
$ cat hello.txt
root:x:0:0:root:/root:/bin/bash
$awk -F [" ":]+ '{print $1,$2,$3}' hello.txt
root x 0

```

实例:忽略大小写  

```
$ awk 'BEGIN{IGNORECASE=1} /this/' log.txt
---------------------------------------------
2 this is a test
This's a test
```

## awk脚本

关于awk脚本，我们需要注意两个关键词BEGIN和END。

- BEGIN{ 这里面放的是执行前的语句 }
- END {这里面放的是处理完所有的行后要执行的语句 }
- {这里面放的是处理每一行时要执行的语句}

假设有这么一个文件（学生成绩表）：

```
$ cat score.txt
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62
```

我们的awk脚本如下：

```
$ cat cal.awk
#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0
 
    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}

```
来看一下运行结果：
```
$ awk -f cal.awk score.txt
NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL
---------------------------------------------
Marry  2143     78       84       77      239
Jack   2321     66       78       45      189
Tom    2122     48       77       71      196
Mike   2537     87       97       95      279
Bob    2415     40       57       62      159
---------------------------------------------
  TOTAL:       319      393      350
AVERAGE:     63.80    78.60    70.00
```
## awk的if、循环和数组
###条件语句
awk 提供了非常好的类似于 C 语言的 if 语句。  
```
{
        if ($1=="foo"){
                if($2=="foo"){
                        print "uno"
                }else{
                        print "one"
                }
        }elseif($1=="bar"){
                print "two"
        }else{
                print "three"
        }
}
```
使用 if 语句还可以将代码： 
```
! /matchme/ { print $1 $3 $4 }
```
转换成：
```
{
　　if ( $0 !~ /matchme/ ) {
　　　　print $1 $3 $4
　　}
}
```
### 循环结构

- for循环
awk 允许创建 for 循环，它就象 while 循环，也等同于 C 语言的 for 循环：
```
for ( initial assignment; comparison; increment ) {
    code block
}
```
例子：
```
for ( x=1;x<=4;x++ ) {
    print "iteration", x
}
iteration1
iteration2
iteration3
iteration4
```
- while循环
awk 的 while 循环结构，它等同于相应的 C 语言 while 循环。 awk 还有"do...while"循环，它在代码块结尾处对条件求值，而不像标准 while 循环那样在开始处求值。
```
{
    count=1 do {
        print "I get printed at least once no matter what"
    } while ( count !=1 )
}
```
与一般的 while 循环不同，由于在代码块之后对条件求值， "do...while"循环永远都至少执行一次。换句话说，当第一次遇到普通 while 循环时，如果条件为假，将永远不执行该循环。

- break和continue
awk 提供了 break 和 continue 语句。使用这些语句可以更好地控制 awk 的循环结构
```
#break 语句示例
x=1
while(1) {
　　print "iteration", x
　　if ( x==10 ) {
　　　　break
　　}
　　x++
}
```
这里， break 语句用于“逃出”最深层的循环。 "break"使循环立即终止，并继续执行循环代码块后面的语句。
continue 语句补充了 break，其作用如下：
```
x=1
while (1) {
    if ( x==4 ) {
        x++
        continue
    }
    print "iteration", x
    if ( x>20 ) {
        break
    }
    x++
}
```
这段代码打印"iteration1"到"iteration21"， "iteration4"除外。如果迭代等于 4，则增加 x并调用 continue 语句，该语句立即使 awk 开始执行下一个循环迭代，而不执行代码块的其余部分。

###数组

- 定义

1:可以用数值作数组索引(下标)
```
Tarray[1]=“cheng mo”
Tarray[2]=“800927”
```
2:可以用字符串作数组索引(下标)
```
Tarray[“first”]=“cheng ”
Tarray[“last”]=”mo”
Tarray[“birth”]=”800927”
```
使用中 print Tarray[1] 将得到”cheng mo” 而 print Tarray[2] 和 print[“birth”] 都将得到 ”800927” 。

- 数组相关函数
1. 得到数组长度  
```
#split进行分割字符串为数组，也会返回分割得到数组长度。
$ awk 'BEGIN{info="it is a test";lens=split(info,tA," ");print lens;}'
4 4
```
2. 输出数组内容(无序，有序输出）：

```
$ awk 'BEGIN{info="it is a test";split(info,tA," ");for(k in tA){print k,tA[k];}}'
4 test
1 it
2 is
3 a
```

for…in 输出，因为数组是关联数组，默认是无序的。所以通过for…in 得到是无序的数组。如果需要得到有序数组，需要通过下标获得。

```
$ awk 'BEGIN{info="it is a test";tlen=split(info,tA," ");for(k=1;k<=tlen;k++){print k,tA[k];}}' 
1 it
2 is
3 a
4 test
```

3. 判断键值存在以及删除键值  
通过if(key in array) 通过这种方法判断数组中是否包含”key”键值。

```
$ awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";if( "c" in tB){print "ok";};for(k in tB){print k,tB[k];}}'  
a a1
b b1
```

delete array[key]可以删除，对应数组key的，序列值。  

```
$ awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";delete tB["a"];for(k in tB){print k,tB[k];}}'                     
b b1
```

##awk内置字符串函数


| 函数 | 功能 |
| :------------| :--------------------------------------------------------------|
| gsub(r,s) | 在整个$0中用s替代r |
| gsub(r,s,t) | 在整个t中用s替代r |
| index(s,t) | 函数返回目标字符串s中查询字符串t的首位置 |
| length(s) | 返回s长度 |
| match(s,r) | 测试s是否包含匹配r的字符串,返回值为成功出现的字符排列数。如果未找到,返回0 |
| split(s,a,fs) | 在fs上将s分成序列a |
| sprint(fmt,exp) | 返回经fmt格式化后的exp |
| sub(r,s) | 将用s替代$0中最左边最长的子串，该子串被(r)匹配 |
| substr(s,p) | 返回字符串s中从p开始的后缀部分 |
| substr(s,p,n) | 返回字符串s中从p开始长度为n的后缀部分 |

实例：  
```
#替换
$ awk 'BEGIN{info="this is a test2010test!";gsub(/[0-9]+/,"!",info);print info}' 
this is a test!test!
# 查找
$ awk 'BEGIN{info="this is a test2010test!";print index(info,"test")"}'
11
# 匹配查找
$ awk 'BEGIN{info="this is a test2010test!";print match(info,/[0-9]+/)?"ok":"no found";}'
ok
# 截取 
$ awk 'BEGIN{info="this is a test2010test!";print substr(info,4,10);}'
s is a tes
# 分割 
awk 'BEGIN{info="this is a test";split(info,tA," ");print length(tA);for(k in tA){print k,tA[k];}}' 
4
4 test
1 this
2 is
3 a
```

## 另外一些实例

AWK的hello world程序为：

```
BEGIN { print "Hello, world!" }
```

计算文件大小  

```
$ ls -l *.txt | awk '{sum+=$6} END {print sum}'
```

从文件中找出长度大于80的行

```
awk 'length>80' log.txt
```

打印九九乘法表

```
seq 9 | sed 'H;g' | awk -v RS='' '{for(i=1;i<=NF;i++)printf("%dx%d=%d%s", i, NR, i*NR, i==NR?"\n":"\t")}'
```
