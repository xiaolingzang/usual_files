## python json模块的dumps,loads,dump,load方法

### json.dumps()

json.dumps()用于将特定的对象把序列化处理为字符串

```
>>> import json
>>> l1 = [1,2,3,454]   
>>> d1 = {'k1':'v1'}
>>> ret = json.dumps(l1)
>>> print(type(ret))
<type 'str'>
>>> ret = json.dumps(d1)
>>> print(type(ret))
<type 'str'>
>>> 
```

### json.loads()
外形和list和dict一样可以通过反序列化把这2个字符串转换成list和dict，这里如果外形不是list或者dict的形状，则不会转换成功的。

```
>>> l1 = '[1,2,3,4]'
>>> d1 = '{"k1":"v1"}'
>>> print(type(l1))
<type 'str'>
>>> print(type(d1))
<type 'str'>
>>> ret = json.loads(l1)
>>> print(ret,type(ret))
([1, 2, 3, 4], <type 'list'>)
>>> ret = json.loads(d1)
>>> print(ret,type(ret))
({u'k1': u'v1'}, <type 'dict'>)
```

### json.dump()
json.dump()用于将dict类型的数据转成str，并写入到json文件中。下面两种方法都可以将数据写入json文件
 
```
>>> import json
>>> name_emb={'a':'1111','b':'2222','c':'3333','d':'4444'}
>>> jsObj = json.dumps(name_emb)
>>> with open(emb_filename, "w") as f:
...     f.write(jsObj)
...     f.close()   
# solution2	
>>> json.dump(name_emb, open(emb_filename, "w"))
```

### json.load()
json.load()用于从json文件中读取数据。

```
>>> import json  
>>> emb_filename = ('./emb_json.json')  
>>> jsObj = json.load(open(emb_filename))    
>>> print(jsObj)
{u'a': u'1111', u'c': u'3333', u'b': u'2222', u'd': u'4444'}
>>> print(type(jsObj))
<type 'dict'>
>>> for key in jsObj.keys():
...     print('key: %s   value: %s' % (key,jsObj.get(key)))
... 
key: a   value: 1111
key: c   value: 3333
key: b   value: 2222
key: d   value: 4444
```

 