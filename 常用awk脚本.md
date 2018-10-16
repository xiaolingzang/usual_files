##  常用awk脚本

### 基本统计

- 将每一条记录根据分隔符重新按行输出
```
awk 'BEGIN{FS="\t"}{for(i=1;i<=NF;i++)print($i);printf("\n")}'
```
- 求最大值
```
awk 'BEGIN {max = 0} {if ($1+0 > max+0) max=$1} END {print "Max=", max}'
```
- 求最小值
```
awk 'BEGIN {min = 65536} {if ($1+0 < min+0) min=$1} END {print "Min=", min}'
```
- 求和
```
awk '{sum+=$1} END {print "Sum= ", sum}'
```
- 求平均值
```
awk '{sum+=$1} END {print "Avg= ", sum/NR}'
```
- 统计每个元素出现的次数并排序
```
awk  '{a[$1]++} END{for(i in a){print i,a[i] | "sort -r -n -k2"}}' 
```
- 统计每条记录包含的item
```
awk 'BEGIN{FS="\t"}{print (NF-4)/6.0}'
```

- 统计记录条数
```
awk -F"\t" '{if($2!='') pv+=1}END{print pv}'
awk -F"\t" '{if($2!='')print $0}'|grep -oE "series_alias_[^_]+"|sort|uniq |wc -l
```

### 分组统计功能
```
cat groupandsum.txt 
John|P|physics|2|02/12/2002
Rick|L|Mechanical|1|02/12/2002
Jack|T|electrical|3|03/12/2003
Phil|R|Electrucal|1|03/12/2003
Mike|T|mechamical|2|10/12/2003
Paul|R|chemical|2|10/12/2003
John|T|chemical|3|10/12/2002
Tony|N|chemical|2|10/11/2003
James|R|Electrucal|2|10/11/2003

```
- 分组求和   

```
awk -F "|" '{x[$5]+=$4} END{for( i in x ){print i,x[i]}}' groupandsum.txt 

03/12/2003 4
02/12/2002 3
10/12/2002 3
10/12/2003 4
10/11/2003 4
```

- 二次分组

```
awk -F "|" '{x[$5"-"$3]+=$4} END{for( i in x ){print i,x[i]}}' groupandsum.txt 
10/12/2002-chemical 3
03/12/2003-Electrucal 1
03/12/2003-electrical 3
02/12/2002-Mechanical 1
10/12/2003-mechamical 2
02/12/2002-physics 2
10/11/2003-Electrucal 2
10/11/2003-chemical 2
10/12/2003-chemical 2
```
- 格式化处理

```
awk -F'|' '{a[$5]+=$4}END{for(i in a)printf("%s\t%d\n",i,a[i])}' groupandsum.txt
```
- 分组求平均

```
[root@localhost wms]# awk -F'|' '{a[$5]+=$4;ca[$5]++}END{for(i in a)printf("%s\t%d\t%.2f\n",i,a[i],a[i]/ca[i])}' groupandsum.txt
03/12/2003      4       2.00
02/12/2002      3       1.50
10/12/2002      3       3.00
10/12/2003      4       2.00
10/11/2003      4       2.00
```
- 分组求最大最小值

```
awk -F'|' '{if($4>x[$5]){x[$5]=$4}} END{for (i in x) print i,x[i] }' groupandsum.txt

03/12/2003 3
02/12/2002 2
10/12/2002 3
10/12/2003 2
10/11/2003 2
```
- 分组处理字符

```
对以数据分类： 
a 111 
a 222 
a 333 
b 444 
d 555

awk '{x[$1]=x[$1]"\n"$2} END{for( i in x ){print i":", x[i]}}' juhe.txt
a:                                                                         
111                                                                        
222                                                                        
333                                                                        
b:                                                                         
444                                                                        
555 
```
