### 变量引用

人们经常使用“变量是盒子”这样的比喻，但是这有碍于理解面向对象语言中的引用式变量。Python 变量类似于 Java 中的引用式变量，因此最好把它们理解为附加在对象上的标注或便签。

在示例中所示的交互式控制台中，无法使用“变量是盒子”做解释。示意图说明了在 Python 中为什么不能使用盒子比喻，而便签则指出了变量的正确工作方式。

> **示例**　变量 `a` 和 `b` 引用同一个列表，而不是那个列表的副本

```
>>> a = [1, 2, 3]
>>> b = a
>>> a.append(4)
>>> b
[1, 2, 3, 4]
```

![image.png](https://upload-images.jianshu.io/upload_images/468490-00c4e74cafbec44c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**如果把变量想象为盒子，那么无法解释 Python 中的赋值；应该把变量视作便利贴，这样示例中的行为就好解释了**

刚刚我们说明了两个变量引用同一个数据的情况，再看一种情况：一个变量先后引用不同的数据

* 定义一个整数变量 `a`，并且赋值为 `1`

| 代码 | 图示 |
| :---: | :---: |
| a = 1 | ![image.png](https://upload-images.jianshu.io/upload_images/468490-e5bd656df3ce9e1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
|

* 将变量 `a` 赋值为 `2`

| 代码 | 图示 |
| :---: | :---: |
| a = 2 |![image.png](https://upload-images.jianshu.io/upload_images/468490-6dbef6333a31e174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![image.png](https://upload-images.jianshu.io/upload_images/468490-739b8d643c37c5ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
|

* 定义一个整数变量 `b`，并且将变量 `a` 的值赋值给 `b`

| 代码 | 图示 |
| :---: | :---: |
| b = a |![image.png](https://upload-images.jianshu.io/upload_images/468490-55592a901fe63139.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
|

> 变量 `b` 是第 2 个贴在数字 `2` 上的标签

如果变量已经被定义，当给一个变量赋值的时候，本质上是 修改了数据的引用

- 变量 不再 对之前的数据引用
- 变量 改为 对新赋值的数据引用

再看一看字典的例子：
```
>>> charles = {'name': 'Charles L. Dodgson', 'born': 1832}
>>> lewis = charles  
>>> lewis is charles
True
>>> id(charles), id(lewis) 
(4300473992, 4300473992)
>>> lewis['balance'] = 950  
>>> charles
{'name': 'Charles L. Dodgson', 'balance': 950, 'born': 1832}
```

```
>>> alex = {'name': 'Charles L. Dodgson', 'born': 1832, 'balance': 950}  
>>> alex == charles  
True
>>> alex is not charles  
True
```
![image.png](https://upload-images.jianshu.io/upload_images/468490-730e4c76e58c4b11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
charles 和 lewis 绑定同一个对象，alex 绑定另一个具有相同内容的对象

###==和is
== 运算符比较两个对象的值（对象中保存的数据），而 is 比较对象的引用（标识）。
通常，我们关注的是值，而不是标识，因此 Python 代码中 == 出现的频率比 is 高。

### 函数的参数作为引用
Python 唯一支持的参数传递模式是共享传参（call by sharing）。

共享传参指函数的各个形式参数获得实参中各个引用的副本。也就是说，函数内部的形参是实参的别名。

这种方案的结果是，函数可能会修改作为参数传入的可变对象，但是无法修改那些对象的标识（即不能把一个对象替换成另一个对象）。下面示例中有个简单的函数，它在参数上调用 += 运算符。分别把数字、列表和元组传给那个函数，实际传入的实参会以不同的方式受到影响。
```
>>> def f(a, b):
...     a += b
...     return a
...
>>> x = 1
>>> y = 2
>>> f(x, y)
3
>>> x, y  
(1, 2)
>>> a = [1, 2]
>>> b = [3, 4]
>>> f(a, b)
[1, 2, 3, 4]
>>> a, b  
([1, 2, 3, 4], [3, 4])
>>> t = (10, 20)
>>> u = (30, 40)
>>> f(t, u)
(10, 20, 30, 40)
>>> t, u 
((10, 20), (30, 40))
```

数字 x 没变。
列表 a 变了。
元组 t 没变。

#### 数字的情况
![image.png](https://upload-images.jianshu.io/upload_images/468490-bcd521869eff42ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


相当于有隐含的代码
a=x
b=y
函数调用结束后，a和b因为是临时变量，将不在指向数据1和2，但x，y依旧在引用着1和2，数据1和数据2因为还有变量在引用着自己所以并没有销毁
#### 列表的情况
![image.png](https://upload-images.jianshu.io/upload_images/468490-8887811247bb54d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里主要的区别之处在于
```
a += b
```
这行代码的不同。
因为数字是不可变变量，所以，a += b使得a指向了新的数据3；
而列表是可变变量，所以a += b的结果是就地修改列表，追加数据。

###可变和不可变类型
- 不可变类型，内存中的数据不允许被修改：
  - 数字类型 int, bool, float, complex, long(2.x)
  - 字符串 str
  - 元组 tuple
- 可变类型，内存中的数据可以被修改：
  - 列表 list
  - 字典 dict
  - 自定义类型（class定义的类型，后面讲到）

