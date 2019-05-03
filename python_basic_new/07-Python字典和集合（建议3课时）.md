##  字典
###  字典定义
字典(dictionary)是包含若干“键：值”元素的无序可变序列，字典中的每个元素包含“键”和“值”两部分，定义字典时，每个元素的键和值用冒号分隔，元素之间用逗号分隔，所有的元素放在一对大括号“｛｝”中。字典中的键可以为任意不可变数据，比如整数、实数、字符串、元组等等。

###  字典的创建
**创建**
使用“=”将一个字典赋值给一个变量

例如：
```
>>> a_dict = {'server': 'db.neuedu.com', 'database': 'mysql'}

>>> a_dict

{'database': 'mysql', 'server': 'db.neuedu.com'}

>>> x = {}                     #空字典

>>> x

{}
```
使用dict()利用已有数据创建字典

例如：
```
>>> keys = ['a', 'b', 'c', 'd']

>>> values = [1, 2, 3, 4]

>>> dictionary = dict(zip(keys, values))

>>> dictionary

{'a': 1, 'c': 3, 'b': 2, 'd': 4}

>>> x = dict() #空字典

>>> x

{}
```
使用dict()根据给定的键、值创建字典

例如：
```
>>> d = dict(name='Dong', age=37)

>>> d

{'age': 37, 'name': 'Dong'}
```
以给定内容为键，创建值为空的字典

例如：
```
>>> adict = dict.fromkeys(['name', 'age', 'sex'])

>>> adict

{'age': None, 'name': None, 'sex': None}
```


### 字典元素的读取
- 以键作为下标可以读取字典元素，若键不存在则抛出异常

例如：
```
>>> aDict = {'name':'Dong', 'sex':'male', 'age':37}

>>> aDict['name']

'Dong'

>>> aDict['tel']                                 #键不存在，抛出异常

Traceback (most recent call last):

  File "<pyshell#53>", line 1, in <module>

    aDict['tel']

KeyError: 'tel'
```
- 使用字典对象的get方法获取指定键对应的值，并且可以在键不存在的时候返回指定值。

例如：
```
>>> print(aDict.get('address'))

None

>>> print(aDict.get('address', 'SDIBT'))

SDIBT

>>> aDict['score'] = aDict.get('score',[])

>>> aDict['score'].append(98)

>>> aDict['score'].append(97)

>>> aDict

{'age': 37, 'score': [98, 97], 'name': 'Dong', 'sex': 'male'}
```
- 使用字典对象的items()方法可以返回字典的键、值对列表

例如：

 
```
>>> aDict={'name':'Dong', 'sex':'male', 'age':37}

>>> for item in aDict.items():         #输出字典中所有元素

     print(item)

('age', 37)

('name', 'Dong')

('sex', 'male')

 

>>> for key in aDict:                       #不加特殊说明，默认输出键

  print(key)

age

name

sex
```
- 使用字典对象的keys()方法可以返回字典的键列表

例如：
```
>>> aDict.keys()                                          #返回所有键

dict_keys(['name', 'sex', 'age']
```
- 使用字典对象的values()方法可以返回字典的值列表

例如：
```
>>> aDict.values()                                       #返回所有值

dict_values(['Dong', 'male', 37])
```
- 使用字典对象的setdefault()方法返回指定“键”对应的“值”，如果字典中不存在该“键”，就添加一个新元素并设置该“键”对应的“值”。

例如：
```
>>aDict ={'name' : 'Wang','sex' : 'male'}

>>>aDict.setdefault('age','28')     #增加新元素

'28'

>>>aDic

{'name' : 'Wang','sex' : 'male','age':'28'}
```
###  字典元素的添加
- 当以指定键为下标为字典赋值时，若键存在，则可以修改该键的值；若不存在，则表示添加一个键、值对。

例如：
```
>>> aDict['age'] = 38                      #修改元素值

>>> aDict

{'age': 38, 'name': 'Dong', 'sex': 'male'}

>>> aDict['address'] = 'SDIBT'        #增加新元素

>>> aDict

{'age': 38, 'address': 'SDIBT', 'name': 'Dong', 'sex': 'male'}
```
- 使用字典对象的update方法将另一个字典的键、值对添加到当前字典对象

例如：
```
>>> aDict

{'age': 37, 'score': [98, 97], 'name': 'Dong', 'sex': 'male'}

>>> aDict.items()

dict_items([('age', 37), ('score', [98, 97]), ('name', 'Dong'), ('sex', 'male')])

>>> aDict.update({'a':'a','b':'b'})

>>> aDict

{'a': 'a', 'score': [98, 97], 'name': 'Dong', 'age': 37, 'b': 'b', 'sex': 'male'}
```
### 字典删除
- 使用del删除整个字典，或者字典中的指定元素

- 使用pop()和popitem()方法弹出并删除指定元素

 - 使用clear()方法清空字典中所有元素

#### 字典clear()方法

clear()方法是用来清除字典中的所有数据，因为是原地操作，所以返回None（也可以理解为没有返回值）
```
>>> x['name'] = 'lili'
>>> x['age'] = 20
>>> x
{'age': 20, 'name': 'lili'}
>>> x.clear()
>>> x
{ }

```

#### 字典pop()方法

移除字典数据pop()方法的作用是：删除指定给定键所对应的值，返回这个值并从字典中把它移除。注意字典pop()方法与[列表pop()方法](http://www.iplaypython.com/jinjie/list-pop.html)作用完全不同。
```
>>> x = {'a':1,'b':2}
>>> x.pop('a')
1
>>> x
{'b': 2}
```
#### 字典popitem()方法

字典popitem()方法作用是：随机返回并删除字典中的一对键和值（项）。为什么是随机删除呢？因为字典是无序的，没有所谓的“最后一项”或是其它顺序。在工作时如果遇到需要逐一删除项的工作，用popitem()方法效率很高。
```
>>> x
{'url': 'www.neuedu.com', 'title': 'python web site'}
>>> x.popitem()
('url', 'www.neuedu.com')
>>> x
{'title': 'python web site'}
```

### 判断一个key是否在字典中
#### 使用in方法:
```
#生成一个字典
d = {'name':'tom', 'age':10, 'Tel':110}
#打印返回值，其中d.keys()是列出字典所有的key
print ('name'  in d.keys())
print ('name' in d)
#两个的结果都是返回True
```
除了使用in还可以使用not in，判定这个key不存在。

### 遍历字典
（1）遍历key值
```
>>> a
{'a': '1', 'b': '2', 'c': '3'}
>>> for key in a:
       print(key+':'+a[key])
 
a:1
b:2
c:3
>>> for key in a.keys():
       print(key+':'+a[key])
 
a:1
b:2
c:3
```
在使用上，for key in a和 for key in a.keys():完全等价。

（2）遍历value值
```
>>> for value in a.values():
       print(value)
 
```
（3）遍历字典项
```
>>> for kv in a.items():
       print(kv)
 
('a', '1')
('b', '2')
('c', '3')
```
（4）遍历字典健值

```
>>> for key,value in a.items():
       print(key+':'+value)
 
a:1
b:2
c:3
>>> for (key,value) in a.items():
       print(key+':'+value)
 
a:1
b:2
c:3
```
在使用上for key,value in a.items()与for (key,value) in a.items()完全等价
### 有序字典
Python内置字典是无序的，如果需要一个可以记住元素插入顺序的字典，可以使用collections.OrderedDict。

例如：
```
>>> x = dict()                   #无序字典

>>> x['a'] = 3

>>> x['b'] = 5

>>> x['c'] = 8

>>> x

{'b': 5, 'c': 8, 'a': 3}

>>> import collections

>>> x = collections.OrderedDict() #有序字典

>>> x['a'] = 3

>>> x['b'] = 5

>>> x['c'] = 8

>>> x

OrderedDict([('a', 3), ('b', 5), ('c', 8)])
```
### 字典推导式 
1.前言     

可以对比列表推导式的思路，与字典推导式的进行对比，训练自己的学习迁移能力。

2.表达式
```
{ key_expr: value_expr for value in collection if condition }
```
3.实例

例1: 
```
# 用字典推导式以字符串以及其索引位置建字典
# 代码如下:
strings = ['import','is','with','if','file','exception','liuhu']
d = {key: val for val,key in enumerate(strings)}
# 用字典推导式以字符串以及其长度位置建字典
s = {strings[i]: len(strings[i]) for i in range(len(strings))}
k = {k:len(k)for k in strings}  #相比上一个写法简单很多
print(d)
# {'import': 0, 'is': 1, 'with': 2, 'if': 3, 'file': 4, 'exception': 5, 'liuhu': 6}
print(s)
# {'import': 6, 'is': 2, 'with': 4, 'if': 2, 'file': 4, 'exception': 9, 'liuhu': 5}
print(k)
# {'import': 6, 'is': 2, 'with': 4, 'if': 2, 'file': 4, 'exception': 9, 'liuhu': 5}
```



示例2：同一个字母但不同大小写的值合并起来了。
```
mc = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}
mca = {k.lower(): mc.get(k.lower(), 0) + mc.get(k.upper(), 0) for k in mc.keys()}

# mcase_frequency == {'a': 17, 'z': 3, 'b': 34} she
```

##  集合
###  集合定义
集合是无序可变序列，使用一对大括号界定，元素不可重复，同一个集合中每个元素都是唯一的。集合中只能包含数字、字符串、元组等不可变类型的数据，而不能包含列表、字典、集合等可变类型的数据。


###  集合的创建
直接将集合赋值给变量

例如：
```
>>> a = {3, 5}

>>> a.add(7)                  #向集合中添加元素

>>> a

{3, 5, 7}
```
使用set将其他类型数据转换为集合

例如：
```
>>> a_set = set(range(8,14))

>>> a_set

{8, 9, 10, 11, 12, 13}

>>> b_set = set([0, 1, 2, 3, 0, 1, 2, 3, 7, 8])     #自动去除重复

>>> b_set

{0, 1, 2, 3, 7, 8}

>>> c_set = set()                                              #空集合

>>> c_set

set()
```
###  集合的删除 
使用del删除整个集合

###    集合元素的增加与删除
**增加元素**
使用add()方法为集合添加新元素，如果该元素已存在于集合中则忽略该操作。

例如：
```
>>>s = {1,2,3}

>>>s.add(3)

>>>s

{1,2,3}
```
使用update()方法合并另外一个集合中的元素到当前集合中

例如：
```
>>>s = {1,2,3}

>>>s.update({3,4,5})

>>>s

{1,2,3,4,5}
```
**删除元素**
当不再使用某个集合时，可以使用del命令删除整个集合。集合对象的pop()方法弹出并删除其中一个元素，remove()方法直接删除指定元素，clear()方法清空集合。

例如：
```
>>> a = {1, 4, 2, 3}

>>> a.pop()

1

>>> a.pop()

2

>>> a

{3, 4}

>>> a.add(2)

>>> a

{2, 3, 4}

>>> a.remove(3)

>>> a

{2, 4}
```
###    集合操作 
Python集合支持交集、并集、差集等运算
例如：
```
>>> a_set = set([8, 9, 10, 11, 12, 13])

>>> b_set = {0, 1, 2, 3, 7, 8}

>>> a_set | b_set                             #并集

{0, 1, 2, 3, 7, 8, 9, 10, 11, 12, 13}

>>> a_set.union(b_set)                   #并集

{0, 1, 2, 3, 7, 8, 9, 10, 11, 12, 13}

>>> a_set & b_set                           #交集

{8}

>>> a_set.intersection(b_set)          #交集

{8}

>>> a_set.difference(b_set)             #差集

{9, 10, 11, 12, 13}

>>> a_set - b_set

      {9, 10, 11, 12, 13}

 

>>> x = {1, 2, 3}

>>> y = {1, 2, 5}

>>> z = {1, 2, 3, 4}

>>> x.issubset(y)                                     #测试是否为子集

False

>>> x < y                                        #比较集合大小/包含关系

False

>>> x.issubset(z)

True


>>> x < z                                         #真子集

True
```

 使用集合快速提取序列中单一元素
 
```
>>> import random

>>> listRandom = [random.choice(range(500)) for i in range(100)]

>>> noRepeat = []

>>> for i in listRandom :

    if i not in noRepeat :

   noRepeat.append(i)

>>> len(listRandom)

>>> len(noRepeat)

>>> newSet = set(listRandom)
```
###   集合推导式
Python也支持集合推导式。
```
>>> s = {x.strip() for x in ('  he  ', 'she    ', '    I')}

>>> s

{'I', 'she', 'he'}
```

## 综合练习
###统计单词出现次数
```
word="I'm a boby, I'm a girl. When it is true, it is ture. that are cats, the red is red."
word=word.replace(',','').replace('.','')
word=word.split()
print(word)
print('第1种方法')
setword=set(word)
for i in setword:
    count=word.count(i)
    print(i,'出现次数：',count)
print('第2种方法')
dict = {}
for key in word:
    dict[key] = dict.get(key, 0) + 1
print(dict)
print('第3种方法')
from collections import Counter

result = Counter(dict)
print(result)
print(result.most_common(3))
```
### 奥运会足球分组
 已知有十六支男子足球队参加2008 北京奥运会。写一个程序，把这16 支球队随机分为4 个组。采用List集合和随机数

2008 北京奥运会男足参赛国家：

科特迪瓦，阿根廷，澳大利亚，塞尔维亚，荷兰，尼日利亚，日本，美国，中国，新西兰，巴西，比利时，韩国，喀麦隆，洪都拉斯，意大利

提示：分配一个，删除一个

![image](https://upload-images.jianshu.io/upload_images/468490-3355a278fb9ca16e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/201/format/webp)

```
# // map，保存最终分组结果
# // key保存的是第几组，value是该组对应的国家集合
import random
groupNum2Countrys = {}

strCountrys = "科特迪瓦，阿根廷，澳大利亚，塞尔维亚，荷兰，尼日利亚，日本，美国，中国，新西兰，巴西，比利时，韩国，喀麦隆，洪都拉斯，意大利";
countryList = strCountrys.split("，")

for i in range(4):
    lstGroup = []
    # // 分第1组
    # // 随机从集合中选出一个国家，放到第1组里；然后将这个选出的国家，从原来集合中干掉（删除）
    # // 重复以上步骤4次
    for j in range(4):
        selectIndex = random.randint(0,len(countryList)-1)
        lstGroup.append(countryList[selectIndex])
        countryList.remove(countryList[selectIndex])

    groupNum2Countrys[i+1] = lstGroup

for key,value in groupNum2Countrys.items():
    print('第' + str(key) + '组')
    print(value)


```
 
