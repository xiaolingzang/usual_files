## linux uniq命令
用uniq命令可以删除相邻的重复行：

uniq [file]
但如果一文本中有重复却不相邻的行则无法删除，需要结合sort命令：
```
sort [file]|uniq
等效的sort命令是：
sort -u [file]
```

另外uniq命令有4个有用的选项:
```
$uniq -d file 只输出file中的重复行，且只输出一次，但不输出唯一的行
$uniq -u file 只输出file中的唯一行（当然是一次啦）
$uniq -c file 在每行前显示重复次数，可与其他选项结合，例如-cu或-cd或-c
$uniq -i file 比较时忽略大小写
```
注：
-d 的结果和-u 的结果合并起来就是uniq的结果了。