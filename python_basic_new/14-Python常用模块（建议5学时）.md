## 时间处理模块
### time.time()
time time() 返回当前时间的时间戳（1970纪元后经过的浮点秒数）。

```
import time
print(time.time())
```
输出：
```
1556846505.9781783
```

**简介**

在Python的时间处理模块中，`time`这个模块主要侧重于时间戳格式的处理，而`datetime`则相当于`time`模块的高级封装，提供了更多关于日期处理的方法。

并且`datetime`的接口使用起来更加的直观，方便。

------

**结构**

`datetime`主要由五个模块组成：

-  **datetime.date**：表示日期的类。常用的属性有year, month, day。
-  **datetime.time**：表示时间的类。常用的属性有hour, minute, second, microsecond。
-  **datetime.datetime**：表示日期+时间。
-  **datetime.timedelta**：表示时间间隔，即两个时间点之间的长度，常常用来做时间的加减。
-  **datetime.tzinfo**：与时区有关的相关信息。

在`datetime`中，使用的最多的就是`datetime.datetime`模块，而`datetime.timedelta`常常被用来修改时间。

最后，`datetime`的时间显示是与时区有关系的，所以还有一个处理时区信息的模块`datetime.tzinfo`。

------

### datetime.datetime

```
class datetime.datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None, *, fold=0)
```

- MINYEAR <= year <= MAXYEAR
- 1 <= month <= 12
- 1 <= day <= number of days in the given month and year
- 0 <= hour < 24
- 0 <= minute < 60
- 0 <= second < 60
- 0 <= microsecond < 1000000
- fold in [0, 1]

这是`datetime.datetime`参数的取值范围，如果设定的值超过这个范围，那么就会抛出`ValueError`异常。

其中`year`，`month`，`day`是必须参数。

```
In [1]: import datetime

In [2]: datetime.datetime(year=2000, month=1, day=1, hour=12)
Out[2]: datetime.datetime(2000, 1, 1, 12, 0)
```

### datetime类方法

这些方法大多数用来生成一个`datetime`对象。

- classmethod datetime.**today()**

  获取今天的时间。

  ```
  In [1]: datetime.datetime.today()
  Out[1]: datetime.datetime(2018, 7, 2, 15, 5, 17, 127663)
  ```

- classmethod datetime.**now(tz=None)**

  获取当前的时间，可以传入一个`tzinfo`对象来指定某一个时区。

  ```
  In [2]: datetime.datetime.now()
  Out[2]: datetime.datetime(2018, 7, 2, 15, 8, 30, 593801)
  ```

- classmethod datetime.**fromtimestamp(timestamp, tz=None)**

  用一个时间戳来生成`datetime`对象，同样可以指定时区。

  **注：时间戳需要为10位的**

  ```
  In [3]: datetime.datetime.fromtimestamp(1530515475.18224)
  Out[3]: datetime.datetime(2018, 7, 2, 15, 11, 15, 182240)
  ```

### datetime实例方法

这些方法大多是一个`datetime`对象能进行的操作。

- datetime.**date()** 和 datetime.**time()**

  获取`datetime`对象的日期或者时间部分。

  ```
  In [1]: datetime.datetime.now().date()
  Out[1]: datetime.date(2018, 7, 2)
  
  In [1]: datetime.datetime.now().time()
  Out[1]: datetime.time(15, 24, 37, 355514)
  ```

- datetime.**replace(year=self.year, month=self.month, day=self.day, hour=self.hour, minute=self.minute, second=self.second, microsecond=self.microsecond, tzinfo=self.tzinfo, \* fold=0)**

  替换`datetime`对象的指定数据。

  ```
  In [1]: now = datetime.datetime.now()
  
  In [2]: now
  Out[2]: datetime.datetime(2018, 7, 2, 15, 26, 45, 116239)
  
  In [3]: now.replace(year=2000)
  Out[3]: datetime.datetime(2000, 7, 2, 15, 26, 45, 116239)
  ```

- datetime.`timestamp()`

  转换成时间戳。

  ```
  In [1]: datetime.datetime.now().timestamp()
  Out[1]: 1530515994.798248
  ```

- datetime.**weekday()**

  返回一个值，表示日期为星期几。0为星期一，6为星期天。

  ```
  In [1]: datetime.now().weekday()
  Out[1]: 1
  ```

------

###  datetime.timedelta

在实际的使用中，我们常常会遇到这样的需求：需要给某个时间增加或减少一天，甚至是增加或减少一天三小时二十分钟。

那么在遇到这样的需求时，去计算时间戳是非常的麻烦的，所以`datetime.timedelta`这个模块使我们能够非常方便的对时间做加减。

```
class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
```

`datetime`是对某些运算符进行了重载的，所以我们可以如下操作。

```
In [1]: from datetime import timedelta

In [2]: now
Out[2]: datetime.datetime(2018, 7, 2, 15, 26, 45, 116239)

In [3]: now - timedelta(days=1)
Out[3]: datetime.datetime(2018, 7, 1, 15, 26, 45, 116239)

In [4]: now + timedelta(days=1)
Out[4]: datetime.datetime(2018, 7, 3, 15, 26, 45, 116239)

In [5]: now + timedelta(days=-1)
Out[5]: datetime.datetime(2018, 7, 1, 15, 26, 45, 116239)
```

------

###  strftime() 和 strptime()

`datetime`中提供了两个方法，可以方便的把`datetime`对象转换成格式化的字符串或者把字符串转换成`datetime`对象。

- 由`datetime`转换成字符串：**datetime.strftime()**

  `strftime()`是datetime对象的实例方法：

  ```
  In [1]: datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S %f')
  Out[1]: '2018-07-02 15:26:45 116239'
  ```

- 由字符串转换成`datetime`：**datetime.datetime.strptime()**

  `strptime()`则是一个类方法：

  ```
  In [1]: newsTime='Sun, 23 Apr 2017 05:15:05 GMT'
  
  In [2]: GMT_FORMAT = '%a, %d %b %Y %H:%M:%S GMT'
  
  In [3]: datetime.datetime.strptime(newsTime,GMT_FORMAT)
  Out[3]: datetime.datetime(2017, 4, 23, 5, 15, 5)
  ```

以下是格式化的符号：

- %y 两位数的年份表示（00-99）
- %Y 四位数的年份表示（000-9999）
- %m 月份（01-12）
- %d 月内中的一天（0-31）
- %H 24小时制小时数（0-23）
- %I 12小时制小时数（01-12）
- %M 分钟数（00=59）
- %S 秒（00-59）
- %a 本地简化星期名称
- %A 本地完整星期名称
- %b 本地简化的月份名称
- %B 本地完整的月份名称
- %c 本地相应的日期表示和时间表示
- %j 年内的一天（001-366）
- %p 本地A.M.或P.M.的等价符
- %U 一年中的星期数（00-53）星期天为星期的开始
- %w 星期（0-6），星期天为星期的开始
- %W 一年中的星期数（00-53）星期一为星期的开始
- %x 本地相应的日期表示
- %X 本地相应的时间表示
- %Z 当前时区的名称
- %% %号本身

## 目录操作模块

- 获取当前工作目录
- 获取执行命令的位置
- 路径拼接
- 路径拆分
- 文件重命名
- 删除文件
- 复制文件
- 遍历文件夹下的文件
- 判断文件是否存在
- 判断目录是否存在

###  获取当前工作目录

```
import sys

print(sys.path[0])
```

### 获取执行命令的位置

```
import os

print(os.getcwd())
```

### 路径拼接

由于不同的操作系统的路径分隔符不同，因此在做路径拼接时不要直接拼接字符串，而是通过 `os.path.join()` 函数，如下：

```
import os

os.path.join('/Users/pangao', 'test.txt')

# /Users/pangao/test.txt'
```

###  路径拆分

同理，使用 `os.path.split()` 函数拆分路径，如下：

```
import os

os.path.split('/Users/pangao/test.txt')

# ('/Users/pangao/', 'test.txt')
```

`os.path.splitext()` 可以直接获取文件扩展名，很方便，如下：

```
import os

os.path.splitext('/Users/pangao/test.txt')

# ('/Users/pangao/test', '.txt')
```

**这些合并、拆分路径的函数并不会检测目录和文件是否真实存在，他们仅仅是对字符串进行操作。**

###  文件重命名

假定当前目录下有一个 `test.txt` 文件，如下：

```
import os

os.rename('test.txt', 'test.py')    #重命名
```

###  删除文件

假定当前目录下有一个 `test.txt` 文件，如下：

```
import os

os.remove('test.txt')    #删除
```

###  复制文件

`os` 模块中没有复制函数，幸运的是shutil模块提供了copyfile()的函数，你还可以在shutil模块中找到很多实用函数，它们可以看做是os模块的补充，如下：

```
import shutil

shutil.copyfile('test.txt', 'test.py')
```

###  遍历文件夹下的文件

- 方法1：
   使用 `os.listdir` 获取当前目录下的文件和文件夹，如下：

```
import os

for filename in os.listdir('./'):
    print(filename)
```

- 方法2：
   使用 `glob` 模块，可以设置文件过滤，如下：

```
import glob

for filename in glob.glob('*.py'):
    print(filename)
```

- 方法3：
   通过 `os.walk` ，可以访问子文件夹，如下：

```
import os

for fpathe, dirs, fs in os.walk('./'):
    for f in fs:
        print(fpathe)
        print(dirs)
        print(fs)
        print(os.path.join(fpathe, f))
```

###  判断文件是否存在

```
import os

os.path.isfile('test.txt') # 如果不存在就返回False
```

###  判断目录是否存在

```
import os

os.path.exists(directory) #如果目录不存在就返回False
```
## 随机数模块

- random模块
   Python中产生随机数需要导入random模块：

```
import random
```

### 随机数模块常用方法

- random.randint(a, b)：返回a和b之间的随机整数；

```
>>> import random
>>> random.randint(0, 10)
8
>>> random.randint(0, 10)
7
>>> random.randint(0, 10)
10
>>> 
```

- random.random()：返回０到１之间随机数（不包括1）;

```
>>> random.random()
0.8836361984681352
>>> random.random()
0.013648077769505496
>>> random.random()
0.7267135453127417
>>> 
```

- random.choice(seq)：在不为空的序列中随机选择一个元素；

```
>>> s = 'helloWorld'
>>> random.choice(s)
'o'
>>> random.choice(s)
'r'
```

- random.sample(population, k)：在一个序列或者集合中选择k个随机元素()，返回由K个元素组成新的列表；（k的值小于population的长度）

```
>>> random.sample('12345', 2)
['1', '2']
>>> random.sample('12345', 5)
['2', '1', '3', '4', '5']
>>> random.sample('12345', 6)    #k值超出population范围导致程序异常
Traceback (most recent call last):
  File "<pyshell#41>", line 1, in <module>
    random.sample('12345', 6)
  File "D:\python36\lib\random.py", line 317, in sample
    raise ValueError("Sample larger than population or is negative")
ValueError: Sample larger than population or is negative
>>> 
```

- random.uniform(a, b)：产生一个指定范围内的随机浮点数
   若a < b，随机数n范围：a <= n <= b；
   若a > b，随机数n范围：a<= n <= b；

```

>>> random.uniform(1,10)
1.9304756617571137
>>> random.uniform(10, 1)
2.872422460231057
>>> 
```

- random.randrange(start, stop=None, step=1, _int=<class 'int'>) ：在rang(start, stop,step)中选择一个随机数;

```
>>> random.randrange(1, 10, 1)   #[1,10)之间随机整数
5
>>> random.randrange(1, 10, 1)
2
>>> random.randrange(1, 100, 2)  #[1, 100)之间随机奇数
33
>>> random.randrange(1, 100, 2)
5
```

- random.shuffle(x, random=None)：将列表顺序打乱；

```
>>> l = ['C', 'C++', 'Java', 'C#', 'Python']
>>> random.shuffle(l)
>>> l
['C++', 'C', 'Java', 'C#', 'Python']
>>> random.shuffle(l)
>>> l
['C', 'Python', 'C++', 'C#', 'Java']
>>> 
```

## Collections模块

该模块实现了专门的容器数据类型，为Python的通用内置容器提供了替代方案。
以下几种类型用的很多：

- **defaultdict** (dict子类调用工厂函数来提供缺失值)
- **counter** (用于计算可哈希对象的dict子类)
- **deque** (类似于列表的容器，可以从两端操作)
- **namedtuple** (用于创建具有命名字段的tuple子类的工厂函数)
- **OrderedDict** (记录输入顺序的dict)


### defaultdict

 **其实就是一个查不到key值时不会报错的dict**

**应用实例**

首先我们来看一个用正常dict的例子，如果我们创建了一个叫person的字典，里面存储的key值为name，age，如果这时候尝试调用person['city']，会抛出KeyError错误，因为没有city这个键值：

```
person = {'name':'xiaobai','age':18}
print ("The value of key  'name' is : ",person['name'])
print ("The value of key  'city' is : ",person['city'])

Out: The value of key  'name' is :  xiaobai
Traceback (most recent call last):
  File "C:\Users\E560\Desktop\test.py", line 17, in <module>
    print ("The value of key  'city' is : ",person['city'])
KeyError: 'city'
```

现在如果我们用defaultdict再试试呢？

```
from collections import defaultdict
person = defaultdict(lambda : 'Key Not found') # 初始默认所有key对应的value均为‘Key Not Found’

person['name'] = 'xiaobai'
person['age'] = 18

print ("The value of key  'name' is : ",person['name'])
print ("The value of key  'adress' is : ",person['city'])

Out：The value of key  'name' is :  xiaobai
     The value of key  'adress' is :  Key Not found
```

大家可以发现，这次没有问题了，其实最根本的原因在于当我们创建defaultdict时，首先传递的参数是所有key的默认value值，之后我们添加name，age进去的时候才会有所改变，当我们最终查询时，如果key存在，那就输出对应的value值，如果不存在，就会输出我们事先规定好的值‘Key Not Found’

除此之外外，我们还可以利用defaultdict创建时，传递参数为所有key默认value值这一特性，实现一些其他的功能,比如：

```
from collections import defaultdict
d = defaultdict(list)
d['person'].append("xiaobai")
d['city'].append("paris")
d['person'].append("student")

for i in d.items():
    print(i)

Out: ('person', ['xiaobai', 'student'])
     ('city', ['paris'])
```

一个道理，我们默认所有key对应的是一个list，自然就可以在赋值时使用list的append方法了。再比如下面这个例子：

```
from collections import defaultdict
food = (
    ('jack', 'milk'),
    ('Ann', 'fruits'),
    ('Arham', 'ham'),
    ('Ann', 'soda'),
    ('jack', 'dumplings'),
    ('Ahmed', 'fried chicken'),
)

favourite_food = defaultdict(list)

for n, f in food:
    favourite_food[n].append(f)

print(favourite_food)

Out：defaultdict(<class 'list'>, {'jack': ['milk', 'dumplings'], 'Ann': ['fruits', 'soda'], 'Arham': ['ham'], 'Ahmed': ['fried chicken']})
```


道理和上面差不多，这里大家可以自己拓展，展开想象，相信可能在某个时刻可以用的上defaultdict这个容器

### counter

Counter是dict的子类。因此，它是一个无序集合，其中元素及其各自的计数存储为字典。

就是一个计数器，**一个字典，key就是出现的元素，value就是该元素出现的次数**

**应用实例**

```
from collections import Counter

count_list = Counter(['B','B','A','B','C','A','B','B','A','C'])  #计数list
print (count_list)


count_tuple = Counter((2,2,2,3,1,3,1,1,1))  #计数tuple
print(count_tuple)

Out：Counter({'B': 5, 'A': 3, 'C': 2})
     Counter({1: 4, 2: 3, 3: 2})
```

**Counter一般不会用于dict和set的计数，因为dict的key是唯一的，而set本身就不能有重复元素**

现在我们也可以直接把在defaultdict例子中用过food元组拿来计数：

```
from collections import Counter
food = (
    ('jack', 'milk'),
    ('Ann', 'fruits'),
    ('Arham', 'ham'),
    ('Ann', 'soda'),
    ('jack', 'dumplings'),
    ('Ahmed', 'fried chicken'),
)

favourite_food_count = Counter(n for n,f in food)  #统计name出现的次数
print(favourite_food_count)

Out: Counter({'jack': 2, 'Ann': 2, 'Arham': 1, 'Ahmed': 1})
```

### deque


在我们需要在容器两端的更快的添加和移除元素的情况下，可以使用deque.

deque就是一个可以两头操作的容器，类似list但比列表速度更快

 **应用实例**

deque的方法有很多，很多操作和list类似，也支持切片

```
from collections import deque
d = deque()
d.append(1)
d.append(2)
d.append(3)

print(len(d))
print(d[0])
print(d[-1])

Out: 3
     1
     3
```

deque最大的特点在于我们可以从两端操作：

```
d = deque([i for i in range(5)])
print(len(d))
# Output: 5

d.popleft()   # 删除并返回最左端的元素
# Output: 0

d.pop()       # 删除并返回最右端的元素
# Output: 4

print(d)
# Output: deque([1, 2, 3])

d.append(100)  # 从最右端添加元素

d.appendleft(-100) # 从最左端添加元素

print(d)
# Output: deque([-100, 1, 2, 3, 100])
```

除了这些deque的方法实在太多了，比如我再举几个常用的例子，首先我们定义一个deque时可以规定它的最大长度，deque和list一样也支持extend方法，方便列表拼接，但是deque提供双向操作：

```
from collections import deque
d = deque([1,2,3,4,5], maxlen=9)  #设置总长度不变
d.extendleft([0])  # 从左端添加一个list
d.extend([6,7,8])   # 从右端拓展一个list
print(d)

Out：deque([0, 1, 2, 3, 4, 5, 6, 7, 8], maxlen=9)
```

现在d已经有9个元素了，而我们规定的maxlen=9，这个时候如果我们从左边添加元素，会自动移除最右边的元素，反之也是一样：

```
d.append(100)
print(d)
d.appendleft(-100)
print(d)

Out: deque([1, 2, 3, 4, 5, 6, 7, 8, 100], maxlen=9)
     deque([-100, 1, 2, 3, 4, 5, 6, 7, 8], maxlen=9)
```

### namedtuple

**基础概念**

名称元组。大家一看名字就会感觉和tuple元组有关，没错，它是元组的强化版
namedtuple可以将元组转换为方便的容器。使用namedtuple，我们不必使用整数索引来访问元组的成员。

我觉得可以把namedtuple 视为 **不可变的** 字典

**应用实例**

首先，让我们先回顾一下普通元组是如何访问成员的：

```
person = ('xiaobai', 18)
print(person[0])

out：xiaobai
```

现在我们看看namedtuple（名称元组）的强大之处：

```
from collections import namedtuple

Person = namedtuple('Person', 'name age city')        # 类似于定义class
xiaobai = Person(name="xiaobai", age=18, city="paris") # 类似于新建对象
print(xiaobai)

Out：Person(name='xiaobai', age=18, city='paris')
```

我们创建namedtuple时非常像定义一个class，这里Person好比是类名，第二个参数就是namedtuple的值的名字了，我感觉很像class里的属性，不过这里不用加逗号分离，下面让我们看看如何访问namedtuple的成员：

```
print(xiaobai.name)
print(xiaobai.age)
print(xiaobai.city)

out：xiaobai
     18
     paris
```

这种无限接近class调用属性的方式还是非常不错的，在一些实际场景很有用。
最后还有一点千万不要忘了，我们不能修改namedtuple里的值：

```
xiaobai.name = 'laobai'
Out：Traceback (most recent call last):
  File "C:\Users\E560\Desktop\test.py", line 5, in <module>
    xiaobai.name = 'laobai'
AttributeError: can't set attribute
```
再看一例
```
import collections
# 将纸牌定义为具名元组，每个纸牌都有等级和花色
Card = collections.namedtuple('Card', 'rank suit')

class FrenchDeck:
    # 等级2-A
    ranks = [str(n) for n in range(2,11)] + list('JQKA')
    # 花色红黑方草
    suits = 'spades diamonds clubs hearts'.split()
    # 构建纸牌
    def __init__(self):
        self.cards = [Card(rank, suit) for suit in self.suits for rank in self.ranks]


french_deck = FrenchDeck()
print(french_deck.cards[0])
print(french_deck.cards[0].rank)
print(french_deck.cards[0].suit)

```


### OrderedDict

**基础概念**

“OrderedDict” 本身就是一个dict，但是它的特别之处在于会记录插入dict的key和value的顺序

**应用实例**

```
from collections import OrderedDict
d = {}
d['a'] = 1
d['b'] = 2
d['c'] = 3
d['d'] = 4
print(d)

Out:{'a': 1, 'c': 3, 'b': 2, 'd': 4}
```

大家可以看到，这是一个普通的dict，因为无序，即使我们依次添加了a,b,c,d 四个键并赋予value，但是输出的顺序并不可控。OrderedDict的出现就是为了解决这个问题：

```
from collections import OrderedDict
d = OrderedDict()
d['a'] = 1
d['b'] = 2
d['c'] = 3
d['d'] = 4
print(d)

Out：OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])
```

这回输出时好多了，因为会自动记录插入的顺序，同理，如果我们删除一个key, OrderedDict的顺序不会发生变化：

```
from collections import OrderedDict

print("Before deleting:\n")
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3
od['d'] = 4

for key, value in od.items():
    print(key, value)

print("\nAfter deleting:\n")
od.pop('c')
for key, value in od.items():
    print(key, value)

print("\nAfter re-inserting:\n")
od['c'] = 3
for key, value in od.items():
    print(key, value) 
    

Out：Before deleting:

    ('a', 1)
    ('b', 2)
    ('c', 3)
    ('d', 4)
    
    After deleting:
    
    ('a', 1)
    ('b', 2)
    ('d', 4)
    
    After re-inserting:
    
    ('a', 1)
    ('b', 2)
    ('d', 4)
    ('c', 3)
```
## pickle模块

python对象的序列化与反序列化

特点

1、只能在python中使用，只支持python的基本数据类型。

2、可以处理复杂的序列化语法。（例如自定义的类的方法，游戏的存档等）

3、序列化的时候，只是序列化了整个序列对象，而不是内存地址。

 一、内存中操作：
dumps方法将对象转成字节（序列化）
loads方法将字节还原为对象（反序列化）
```
import pickle
#dumps
li = [11,22,33]
r = pickle.dumps(li)
print(r)


#loads
result = pickle.loads(r)
print(result)
```

二、文件中操作：



```
#dump：
import pickle

li = [11,22,33]
pickle.dump(li,open('db','wb'))

#load
ret = pickle.load(open('db','rb'))
print(ret)
```


三、练习：

pickle的序列化：
格式：pickle.dumps(序列化对象)

```

import pickle


test = r'test.txt'

#反序列化代码中也要定义相同的函数名称，函数体没限制
def sayhi(name):
    print("hello",name)

info = {
    '':'',
    'age':32,
    'func':sayhi
}

print(pickle.dumps(info))

with open(test,'wb') as f:
    f.write( pickle.dumps(info) )
```

pickle反序列化：
格式：pickle.loads(读取文件逐行记录)

```
import pickle

test = r'test.txt'

#需要定义序列化代码中同样的函数名，函数体没限制
def sayhi(name):
    print("hello",name)
    print("hello2",name)

with open(test,'rb') as f:
    data = pickle.loads(f.read())
    print('data>>>',data)


print(data['func']("Alex"))

```
输出
```
data>>> {'': '', 'age': 32, 'func': <function sayhi at 0x000000000231C1E0>}
hello Alex
hello2 Alex
None
```
注意：
1、print(data['func']("Alex"))时，调用了pickle的反序列化变量data。

2、需要在序列化和反序列化定义相同的函数名称，但内容可以不一样。否则报错如下：
```
AttributeError: Can't get attribute 'sayhi' on <module '__main__' 
```


写入文件并序列化

格式：pickle.dump(序列对象变量，文件名)

```
import pickle


test = r'test.txt'

#反序列化代码中也要定义相同的函数名称，函数体没限制
def sayhi(name):
    print("hello",name)

info = {
    '':'',
    'age':32,
    'func':sayhi
}

print(pickle.dumps(info))

with open(test,'wb') as f:
    #f.write( pickle.dumps(info) )
    pickle.dump(info,f)  #跟上面的f.write( pickle.dumps(info) )语意完全一样。
```

从文件中读取，并反序列化：
格式：pickle.load(文件名)

```
import pickle

test = r'test.txt'

#需要定义序列化代码中同样的函数名，函数体没限制
def sayhi(name):
    print("hello",name)
    print("hello2",name)



with open(test,'rb') as f:
    # data = pickle.loads(f.read())
    data = pickle.load(f)  #跟上面的data = pickle.loads(f.read())语意完全一样。
    print('data>>>',data)


print(data['func']("Alex"))
```
## 综合实例---猜数游戏
```
import random
import pickle
from collections import deque

n = random.randint(0, 100)  # 随机找出0-100之中的数
history_list = deque([], maxlen=5)  # 限制最大长度
try_num = 0
print(n)


def pk(m):
    if m != n:
        if m > n:
            print("大了")
        else:
            print("小了")
        return False
    return True


def h_print():# pickle取
    ret = pickle.load(open('history', 'rb'))
    return ret


def history(history_list): # pickle存
    history_list = list(history_list)
    pickle.dump(history_list, open('history', 'wb'))


while True:
    try_num += 1
    if try_num > 10:
        print("尝试次数过多，您已经笨死了！！！")
        break
    m = input("输入您的答案:")
    try:                               # 异常处理 防止用户输入字母
        # m = input("输入您的答案:")
        if len(m) == 0:
            print("not null")
        m = int(m)
        history_list.append(m)
        history(history_list)
        if pk(m) == True:
            print("以您的智商，居然TM答对了")
            break
    except ValueError:
        if m == "h":
            print("您猜数的历史记录：")
            print(h_print())
        else:
            print("格式有误，请输入整数字")
```




