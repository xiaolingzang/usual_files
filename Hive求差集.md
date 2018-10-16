# Hive求差集运算

## 差集
一般地,设A,B是两个集合,由所有属于A且不属于B的元素组成的集合,叫做集合A减集合B(或集合A与集合B之差),类似地,对于集合A.B,我们把集合{x/x∈A,且x￠B}叫做A与B的差集,记作A－B记作A－B(或A\B),即A－B＝{x|x∈A,且x ￠B}(或A\B＝{x|x∈A,且x ￠B} B－A＝{x/x∈B且x￠A} 叫做B与A的差集。

## 实例

- 比如有如下两个表
```
hive> select * from A;
1	2
1	3
2	1
2	3
3	1
Time taken: 0.3 seconds, Fetched: 5 row(s)
hive> select * from B;
1	2
1	4
2	2
2	3
Time taken: 0.086 seconds, Fetched: 4 row(s)
```
- 要取出A与B的差集（A-B）
```
1	3
2	1
3	1
```
- Hive可不可以用not in? 可以，但是只能用于单个字段。
- select * from A where (uid,goods) not in (select uid,goods from B);这个oracle是支持的，但hive不行。
```
hive> select * from A  where uid not in (select uid from B);
3	1
Time taken: 46.09 seconds, Fetched: 1 row(s)
```
- Hive可不可以用not exists？显然也可以！
```
hive> select * from A  where not exists (select * from B where A.uid=B.uid and A.goods=B.goods);
1	3
2	1
3	1
Time taken: 12.989 seconds, Fetched: 3 row(s)
```
- 不过前两种貌似很费资源，在ODPS里都有限制，下面来介绍一下hive常用的求差集方法，左（右）连接 left outer join先看一下左连接之后表是什么样的
```
hive> select * from A a left outer join B b on a.uid=b.uid and a.goods=b.goods;
1	2	1	2
1	3	NULL	NULL
2	1	NULL	NULL
2	3	2	3
3	1	NULL	NULL
Time taken: 12.735 seconds, Fetched: 5 row(s)
```
- 现在只要取出B的uid和goods为null的行就可以了
```
hive> select a.* from A a left outer join B b on a.uid=b.uid and a.goods=b.goods where b.uid is null and b.goods is null;
1	3
2	1
3	1
Time taken: 13.023 seconds, Fetched: 3 row(s)
```



