#Linux grep命令

##grep命令简介

grep（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

### 命令选项
```
-a 不要忽略二进制数据。
-A<显示行数> 除了显示符合范本样式的那一行之外，并显示该行之后的内容。
-b 在显示符合范本样式的那一行之外，并显示该行之前的内容。
-c 计算符合范本样式的行数。
-C<显示行数>或-<显示行数>  除了显示符合范本样式的那一行之外，并显示该行之前后的内容。
-d<进行动作> 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep命令将回报信息并停止动作。
-e<范本样式> 指定字符串作为查找文件内容的范本样式。
-E 将范本样式为延伸的普通表示法来使用，意味着能使用扩展正则表达式。
-f<范本文件> 指定范本文件，其内容有一个或多个范本样式，让grep查找符合范本条件的文件内容，格式为每一列的范本样式。
-F 将范本样式视为固定字符串的列表。
-G 将范本样式视为普通的表示法来使用。
-h 在显示符合范本样式的那一行之前，不标示行所属的文件名称。
-H 在显示符合范本样式的那一行之前，标示该行的文件名称。
-l 列出文件内容符合指定的范本样式的文件名称。
-L 列出文件内容不符合指定的范本样式的文件名称。
-i 忽略字符大小写的差别。
-n 在显示符合范本样式的那一行之前，标示出该行的编号。
-o 只输出文件中匹配到的部分。
-R/-r 此参数的效果和指定“-d recurse”参数相同。
-v 反转查找。

```

## grep 常见用法

1. 在文件中**搜索一个单词**，命令会返回一个包含“match_pattern”的文本行：
```
grep match_pattern file_name
grep "match_pattern" file_name
```
2. 在多个文件中查找：
```
grep "match_pattern" file_1 file_2 file_3 ...
```
3. 输出除之外的所有行 **-v** 选项：
```
grep -v "match_pattern" file_name
```
4. 标记匹配颜色 **--color=auto** 选项：
```
grep "match_pattern" file_name --color=auto
```
5. 使用正则表达式**-E** 选项:
```
grep -E "[1-9]+"
或
egrep "[1-9]+"
```
6. 只输出文件中匹配到的部分**-o** 选项：
```
echo this is a test line. | grep -o -E "[a-z]+\."
line.
echo this is a test line. | egrep -o "[a-z]+\."
line.
```
7. 统计文件或者文本中包含匹配字符串的行数** -c** 选项：  
```
grep -c "text" file_name
``` 
8. 输出包含匹配字符串的行以及对应行号** -n** 选项：   
```
grep "text" -n file_name
或
cat file_name | grep "text" -n
 #多个文件
grep "text" -n file_1 file_2
```

9. 搜索多个文件并查找匹配文本在哪些文件中：
```
grep -l "text" file1 file2 file3...
```

10. grep**递归搜索**文件  
在多级目录中对文本进行递归搜索**-r**:    
```
grep "text" . -r -n
# .表示当前目录。
```

11. 忽略匹配样式中的字符大小写**-i**：    
```
echo "hello world" | grep -i "HELLO"
hello world
```

12. 选项**-e** 制动多个匹配样式
```
echo this is a text line | grep -e "is" -e "line" -o
is
line
```

13. 同样可以使用**-f**选项来匹配多个样式，在样式文件中逐行写出需要匹配的字符 
	```
	cat patfile
	aaa
	bbb
	
	echo aaa bbb ccc ddd eee | grep -f patfile -o
	```

14. 在grep搜索结果中**包括或者排除指定文件**： 
	```
	#只在目录中所有的.php和.html文件中递归搜索字符"main()"
	grep "main()" . -r --include *.{php,html}
	#在搜索结果中排除所有README文件
	grep "main()" . -r --exclude "README"
	#在搜索结果中排除filelist文件列表里的文件
	grep "main()" . -r --exclude-from filelist
	```

15. 打印出匹配文本之前或者之后的行:

	```
	#显示匹配某个结果之后的3行，使用 -A 选项：
	seq 10 | grep "5" -A 3
	5
	6
	7
	8
	
	#显示匹配某个结果之前的3行，使用 -B 选项：
	seq 10 | grep "5" -B 3
	2
	3
	4
	5
	
	#显示匹配某个结果的前三行和后三行，使用 -C 选项：
	seq 10 | grep "5" -C 3
	2
	3
	4
	5
	6
	7
	8
	
	#如果匹配结果有多个，会用“--”作为各匹配结果之间的分隔符：
	echo -e "a\nb\nc\na\nb\nc" | grep a -A 1
	a
	b
	--
	a
	b
	```


