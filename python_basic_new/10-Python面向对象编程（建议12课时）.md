##面向对象编程

### 类和对象
#### 万物皆对象

![image](https://upload-images.jianshu.io/upload_images/468490-7e1664c9e5bf1772.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/614/format/webp)

> 分类是人们认识世界的一个很自然的过程，在日常生活中会不自觉地将对象进行进行分类

**对象归类**

*   类是抽象的概念，仅仅是模板
    比如说：“人”
*   对象是一个你能够看得到、摸得着的具体实体
    赵本山，刘德华，赵丽颖

**举例**
```
user1 = 'zhangsan'
print(type(user1))
user2 = 'lisi'
print(type(user2))
```
输出
```
<class 'str'>
<class 'str'>
```

以上str是类（python中的字符串类型），user1和user2是对象（以前我们叫变量）

**研究对象**

![](https://upload-images.jianshu.io/upload_images/468490-9d6ce945053564ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/182/format/webp)

顾客类型
属性：
姓名—张浩
年龄—20
体重—60kg

行为：
购买商品

![](https://upload-images.jianshu.io/upload_images/468490-84c5cc086c47bd22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/191/format/webp)

收银员类型
属性：
员工号—10001
姓名—王淑华
部门—财务部

行为：
收款
打印账单

#### 对象的特征——属性

属性——对象具有的各种特征
每个对象的每个属性都拥有特定值
例如：顾客张浩和李明的年龄、姓名不一样



#### 对象的特征——方法（操作，行为）

方法——对象执行的操作（通常会改变属性的值）
顾客对象的方法---购买商品
收银员对象的方法----收款

> 对象：用来描述客观事物的一个实体，由一组属性和方法构成

**例子**
![](https://upload-images.jianshu.io/upload_images/468490-a4a3245ae0af2e62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/246/format/webp)

类型：狗
对象名：doudou
属性：
颜色：白色
方法：
叫，跑，吃


> 对象同时具有属性和方法两项特性
> 对象的属性和方法通常被封装在一起，共同体现事物的特性， 二者相辅相承，不能分割

**课上练习**
说一说教室里的对象
描述他们的属性和方法

#### 从对象抽象出“类”

抽取出下列对象的共同特征（属性和方法）

![](https://upload-images.jianshu.io/upload_images/468490-435122f628e17b70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620/format/webp)

![](https://upload-images.jianshu.io/upload_images/468490-58f565ed1bd38308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/549/format/webp)

> 类是模子，定义对象将会拥有的特征（属性）和行为（方法）

**再次强调**
*   类是抽象的概念，仅仅是模板
    比如说：“人”
    类定义了人这种类型属性（name，age...）和方法(study,work...)
*   对象是一个你能够看得到、摸得着的具体实体
    赵本山，刘德华，赵丽颖，这些具体的人都具有人类型中定义的属性和方法，不同的是他们各自的属性不同。


> 根据类来创建对象被称为**实例化**。

###类的定义

#### 定义只包含方法的类

在 Python 中要定义一个只包含方法的类，语法格式如下：
```
class 类名:

    def 方法1(self, 参数列表):
        pass
    
    def 方法2(self, 参数列表):
        pass
```
方法 的定义格式和之前学习过的函数 几乎一样
区别在于第一个参数必须是 self，大家暂时先记住，稍后介绍 self

#### 创建对象
当一个类定义完成之后，要使用这个类来创建对象，语法格式如下：
```
对象变量 = 类名()
```

***第一个面向对象程序***

**需求**

*   **小猫** 爱 **吃** 鱼，**小猫** 要 **喝** 水

**分析**

1.  定义一个猫类 `Cat`
2.  定义两个方法 `eat` 和 `drink`

```
class Cat:
    """这是一个猫类"""

    def eat(self):
        print("小猫爱吃鱼")

    def drink(self):
        print("小猫在喝水")

tom = Cat()
tom.drink()
tom.eat()
```
使用 Cat 类再创建一个对象
```
lazy_cat = Cat()
lazy_cat.eat()
lazy_cat.drink()
```
### self 参数
和我们以往的经验不同，我们在调用对象的方法是不需要传递self参数，这个self参数是系统自动传递到方法里的。我们需要知道这个self到底是什么？

#### 给对象增加属性

在 Python 中，要 给对象设置属性，非常的容易，但是不推荐使用
因为：对象属性的封装应该封装在类的内部
只需要在 类的外部的代码 中直接通过 . 设置一个属性即可

```
tom.name = "Tom"
```
```
lazy_cat.name = "大懒猫"
```
#### 理解self到底是什么
```
class Cat:
    """这是一个猫类"""

    def eat(self):
        print(f"小猫爱吃鱼,我是{self.name},self的地址是{id(self)}")

    def drink(self):
        print("小猫在喝水")

tom = Cat()
print(f'tom对象的id是{id(tom)}')
tom.name = "Tom"
tom.eat()
print('-'*60)
lazy_cat = Cat()
print(f'lazy_cat对象的id是{id(lazy_cat)}')
lazy_cat.name = "大懒猫"
lazy_cat.eat()
```
输出
```
tom对象的id是36120000
小猫爱吃鱼,我是Tom,self的地址是36120000
------------------------------------------------------------
lazy_cat对象的id是36120056
小猫爱吃鱼,我是大懒猫,self的地址是36120056
```
由输出可见，当我们使用
```
x对象.x方法()
```
的时候，`python会自动将x对象做为实参传给x方法的self参数`
也可以这样记忆`谁点（.）的方法，self就是谁`
>类中的每个实例方法的第一个参数都是self

### 初始化方法init()

**之前代码存在的问题 —— 在类的外部给对象增加属性**

*   将案例代码进行调整，**先调用方法 再设置属性**，观察一下执行效果

```
tom = Cat()
tom.drink()
tom.eat()
tom.name = "Tom"
print(tom)

```

*   程序执行报错如下：

```
AttributeError: 'Cat' object has no attribute 'name'
属性错误：'Cat' 对象没有 'name' 属性

```

**提示**

*   在日常开发中，不推荐在 **类的外部** 给对象增加属性
    *   如果**在运行时，没有找到属性，程序会报错**
*   对象应该包含有哪些属性，应该 **封装在类的内部**

**初始化方法**

*   当使用 `类名()` 创建对象时，会 **自动** 执行以下操作：
    1.  为对象在内存中 **分配空间** —— 创建对象（调用`__new__`，以后说）
    2.  为对象的属性 **设置初始值** —— 初始化方法(调用`__init__`并且将第一步创建的对象，通过self参数传给`__init__`)
*   这个 **初始化方法** 就是 `__init__` 方法，`__init__` 是对象的**内置方法**（有的书也叫**魔法方法**，**特殊方法**）

> `__init__` 方法是 **专门** 用来定义一个类 **具有哪些属性的方法**！

在 `Cat` 中增加 `__init__` 方法，验证该方法在创建对象时会被自动调用

```
class Cat:
    """这是一个猫类"""

    def __init__(self):
        print("初始化方法")

```

**在初始化方法内部定义属性**

*   在 `__init__` 方法内部使用 `self.属性名 = 属性的初始值` 就可以 **定义属性**
*   定义属性之后，再使用 `Cat` 类创建的对象，都会拥有该属性

```
class Cat:

    def __init__(self):

        print("这是一个初始化方法")

        # 定义用 Cat 类创建的猫对象都有一个 name 的属性
        self.name = "Tom"

    def eat(self):
        print("%s 爱吃鱼" % self.name)

# 使用类名()创建对象的时候，会自动调用初始化方法 __init__
tom = Cat()

tom.eat()

```

**改造初始化方法 —— 初始化的同时设置初始值**

*   在开发中，如果希望在 **创建对象的同时，就设置对象的属性**，可以对 `__init__` 方法进行 **改造**
    1.  把希望设置的属性值，定义成 `__init__` 方法的参数
    2.  在方法内部使用 `self.属性 = 形参` 接收外部传递的参数
    3.  在创建对象时，使用 `类名(属性1, 属性2...)` 调用

```
class Cat:

    def __init__(self, name):
        print("初始化方法 %s" % name)
        self.name = name
    ...

tom = Cat("Tom")
...

lazy_cat = Cat("大懒猫")
...

```

###  \__str\__ 方法

*   在 `Python` 中，使用 `print` 输出 **对象变量**，默认情况下，会输出这个变量的类型，以及 **在内存中的地址**（**十六进制表示**）
```
class Cat:

    def __init__(self, new_name):

        self.name = new_name

        print("%s 来了" % self.name)


    def __str__(self):
        return "我是小猫：%s" % self.name

tom = Cat("Tom")
print(tom)

```
输出
```
Tom 来了
<__main__.Cat object at 0x0000000002852278>
```

*   如果在开发中，希望使用 `print` 输出 **对象变量** 时，能够打印 **自定义的内容**，就可以利用 `__str__` 这个内置方法了

> 注意：`__str__` 方法必须返回一个字符串
```
class Cat:

    def __init__(self, new_name):

        self.name = new_name

        print("%s 来了" % self.name)


    def __str__(self):
        return "我是小猫：%s" % self.name

tom = Cat("Tom")
print(tom)
```
```
Tom 来了
我是小猫：Tom
```

### 面向对象vs面向过程
```
def CarInfo(type,price):
    print ("the car's type %s,price:%d"%(type,price))

print('函数方式（面向过程）')
CarInfo('passat',250000)
CarInfo('ford',280000)


class Car:
    def __init__(self,type,price):
        self.type = type
        self.price = price

    def printCarInfo(self):
        print ("the car's Info in class:type %s,price:%d"%(self.type,self.price))
print('面向对象')
carOne = Car('passat',250000)
carTwo = Car('ford',250000)
carOne.printCarInfo()
carTwo.printCarInfo()

```
输出
```
函数方式（面向过程）
the car's type passat,price:250000
the car's type ford,price:280000
面向对象
the car's Info in class:type passat,price:250000
the car's Info in class:type ford,price:250000
```
类能实现的功能，用函数也可以实现，那么类的存在还有什么特殊的意义呢？
**新需求---加一个行驶里程的功能**
`面向过程实现`
```
def CarInfo(type,price):
    print ("the car's type %s,price:%d"%(type,price))

def driveDistance(oldDistance,distance):
        newDistance = oldDistance + distance
        return newDistance

print('函数方式（面向过程）')
CarInfo('passat',250000)
distance = 0
distance = driveDistance(distance,100)
distance = driveDistance(distance,200)
print(f'passat已经行驶了{distance}公里')
```
输出
```
函数方式（面向过程）
the car's type passat,price:250000
passat已经行驶了300公里
```

`面向对象实现`
```
class Car:
    def __init__(self,type,price):
        self.type = type
        self.price = price
        self.distance = 0 #新车

    def printCarInfo(self):
        print ("the car's Info in class:type %s,price:%d"%(self.type,self.price))

    def driveDistance(self,distance):
        self.distance += distance

print(f'面向对象')
carOne = Car('passat',250000)
carOne.printCarInfo()
carOne.driveDistance(100)
carOne.driveDistance(200)
print(f'passat已经行驶了{carOne.distance}公里')
print(f'passat的价格是{carOne.price}')
```
输出
```
the car's Info in class:type passat,price:250000
passat已经行驶了300公里
passat的价格是250000
```
通过对比我们可以发现：

>面向对象的实现方式封装性更好，已经行驶的公里数是对象内部的属性，对象自身负责管理，外部调用代码无需管理。我们随时可以调用对象的方法和属性得知对象当前的各种信息。而面向过程的方式而言，外部调用代码会“手忙脚乱”

**再加一个需求：**
车每行驶1公里，车的价值贬值10元
面向对象的实现方式，只需添加一行代码：
```
    def driveDistance(self,distance):
        self.distance += distance
        self.price -= distance*10
```
全部代码
```
class Car:
    def __init__(self,type,price):
        self.type = type
        self.price = price
        self.distance = 0 #新车

    def printCarInfo(self):
        print ("the car's Info in class:type %s,price:%d"%(self.type,self.price))

    def driveDistance(self,distance):
        self.distance += distance
        self.price -= distance*10

print(f'面向对象')
carOne = Car('passat',250000)
carOne.printCarInfo()
carOne.driveDistance(100)
carOne.driveDistance(200)
print(f'passat已经行驶了{carOne.distance}公里')
print(f'passat的价格是{carOne.price}')
```
输出
```
面向对象
the car's Info in class:type passat,price:250000
passat已经行驶了300公里
passat的价格是247000
```
面向过程的方式，请自行实现（比较麻烦）
**小结**
**可以拿公司运营作为比喻来说明面向对象开发方式的优点**
>外部调用代码好比老板
面向过程中函数好比员工，让员工完成一个任务，需要老板不断的干涉，大大影响了老板的工作效率。
面向对象中对象好比员工，让员工完成一个任务，老板只要下命令即可，员工可以独挡一面，大大节省了老板的时间。

**还有一种说法**

>世界上本没有类，代码写多了，也就有了类

面向对象开发方式是晚于面向过程方式出现的，几十年前，面向对象还没有出现以前，很多软件系统的代码量就已经达到几十上百万行，科学家为了组织这些代码，将各种关系密切的数据，和相关的方法分门别类，放到一起管理，就形成了面向对象的开发思想和编程语言语法。

### 私有属性---封装

在实际开发中，对象 的 某些属性或方法 可能只希望 在对象的内部被使用，而 不希望在外部被访问到

**定义方式**

在 定义属性或方法时，在 属性名或者方法名前 增加 两个下划线__
实际开发中私有属性也不是一层不变的。所以要给私有属性提供外部能够操作的方法。

####通过自定义get,set方法提供私有属性的访问
```

class Person:
    def __init__(self, name, age):
        self.name = name
        self.__age = age
 
    #定义对私有属性的get方法，获取私有属性
    def getAge(self):
        return self.__age
 
    #定义对私有属性的重新赋值的set方法，重置私有属性
    def setAge(self,age):
        self.__age = age
 
person1 = Person("tom",19)
person1.setAge(20)
print(person1.name,person1.getAge())  #tom 20
 ```
####调用property方法提供私有属性的访问
```
class Student:
    def __init__(self, name, age):
        self.name = name
        self.__age = age
 
    #定义对私有属性的get方法，获取私有属性
    def getAge(self):
        return self.__age
 
    #定义对私有属性的重新赋值的set方法，重置私有属性
    def setAge(self,age):
        self.__age = age
 
    p = property(getAge,setAge) #注意里面getAge,setAge不能带()
 
s1 = Student("jack",22)
s1.p = 23 #如果使用=,则会判断为赋值，调用setAge方法。
print(s1.name,s1.p)  #jack 23   ，直接使用s1.p会自动判断会取值，调用getAge
print(s1.name,s1.getAge()) #jack 23,这个时候set,get方法可以单独使用。
 ```
####使用property标注提供私有属性的访问。
```
class Teacher:
    def __init__(self, name, age,speak):
        self.name = name
        self.__age = age
        self.__speak = speak
 
 
    @property      #注意1.@proterty下面默认跟的是get方法，如果设置成set会报错。
    def age(self):
        return self.__age
 
    @age.setter    #注意2.这里是使用的上面函数名.setter，不是property.setter.
    def age(self,age):
        if age > 150 and age <=0:  #还可以在setter方法里增加判断条件
            print("年龄输入有误")
        else:
            self.__age = age
 
    @property
    def for_speak(self):  #注意2.这个同名函数名可以自定义名称，一般都是默认使用属性名。
        return self.__speak
 
    @for_speak.setter
    def for_speak(self, speak):
        self.__speak = speak
 
t1 = Teacher("herry",45,"Chinese")
t1.age = 38    #注意4.有了property后，直接使用t1.age,而不是t1.age()方法了。
t1.for_speak = "English"  
 
print(t1.name,t1.age,t1.for_speak)  #herry 38 English
```

### 将实例用作属性---对象组合

使用代码模拟实物时，你可能会发现自己给类添加的细节越来越多：属性和方法清单以及文件都越来越长。在这种情况下，可能需要将类的一部分属性和方法作为一个独立的类提取出来。你可以将大型类拆分成多个协同工作的小类。

####实例1摆放家具

**需求**

1.  **房子(House)** 有 **户型**、**总面积** 和 **家具名称列表**
    *   新房子没有任何的家具
2.  **家具(HouseItem)** 有 **名字** 和 **占地面积**，其中
    *   **席梦思(bed)** 占地 `4` 平米
    *   **衣柜(chest)** 占地 `2` 平米
    *   **餐桌(table)** 占地 `1.5` 平米
3.  将以上三件 **家具** **添加** 到 **房子** 中
4.  打印房子时，要求输出：**户型**、**总面积**、**剩余面积**、**家具名称列表**


**剩余面积**

1.  在创建房子对象时，定义一个 **剩余面积的属性**，**初始值和总面积相等**
2.  当调用 `add_item` 方法，向房间 **添加家具** 时，让 **剩余面积** -= **家具面积**

**思考**：应该先开发哪一个类？

**答案** —— **家具类**

1.  家具简单
2.  房子要使用到家具，**被使用的类**，通常应该先开发

**创建家具**

```
class HouseItem:

    def __init__(self, name, area):
        """

        :param name: 家具名称
        :param area: 占地面积
        """
        self.name = name
        self.area = area

    def __str__(self):
        return "[%s] 占地面积 %.2f" % (self.name, self.area)

# 1\. 创建家具
bed = HouseItem("席梦思", 4)
chest = HouseItem("衣柜", 2)
table = HouseItem("餐桌", 1.5)

print(bed)
print(chest)
print(table)

```

**小结**

1.  创建了一个 **家具类**，使用到 `__init__` 和 `__str__` 两个内置方法
2.  使用 **家具类** 创建了 **三个家具对象**，并且 **输出家具信息**

**创建房间**

```
class House:

    def __init__(self, house_type, area):
        """

        :param house_type: 户型
        :param area: 总面积
        """
        self.house_type = house_type
        self.area = area

        # 剩余面积默认和总面积一致
        self.free_area = area
        # 默认没有任何的家具
        self.item_list = []

    def __str__(self):

        # Python 能够自动的将一对括号内部的代码连接在一起
        return ("户型：%s\n总面积：%.2f[剩余：%.2f]\n家具：%s"
                % (self.house_type, self.area,
                   self.free_area, self.item_list))

    def add_item(self, item):

        print("要添加 %s" % item)

...

# 2\. 创建房子对象
my_home = House("两室一厅", 60)

my_home.add_item(bed)
my_home.add_item(chest)
my_home.add_item(table)

print(my_home)

```

**小结**

1.  创建了一个 **房子类**，使用到 `__init__` 和 `__str__` 两个内置方法
2.  准备了一个 `add_item` 方法 **准备添加家具**
3.  使用 **房子类** 创建了 **一个房子对象**
4.  让 **房子对象** 调用了三次 `add_item` 方法，将 **三件家具** 以实参传递到 `add_item` 内部

**添加家具**

**需求**

*   1> **判断** **家具的面积** 是否 **超过剩余面积**，**如果超过**，提示不能添加这件家具
*   2> 将 **家具的名称** 追加到 **家具名称列表** 中
*   3> 用 **房子的剩余面积** - **家具面积**

```
    def add_item(self, item):

        print("要添加 %s" % item)
        # 1\. 判断家具面积是否大于剩余面积
        if item.area > self.free_area:
            print("%s 的面积太大，不能添加到房子中" % item.name)

            return

        # 2\. 将家具的名称追加到名称列表中
        self.item_list.append(item.name)

        # 3\. 计算剩余面积
        self.free_area -= item.area

```

**小结**

*   主程序只负责创建 **房子** 对象和 **家具** 对象
*   让 **房子** 对象调用 `add_item` 方法 **将家具添加到房子**中
*   **面积计算**、**剩余面积**、**家具列表** 等处理都被 **封装** 到 **房子类的内部**

####实例2英雄pk怪物

hero.py
```
from random import randint


class Hero:
    def __init__(self, name,flood,strength):
        self.name = name
        self.flood = flood
        self.strength = strength

    def calc_health(self):
        return self.flood

    def take_damage(self, monster):
        dmage = randint(monster.strength - 5, monster.strength + 5)
        self.flood -= dmage
        print(f"{self.name}你被{monster.name}攻击，受到了{str(dmage)}点伤害!还剩{str(self.flood)}滴血")
        if self.calc_health() <= 0:
            print(f"{self.name}你被杀死了!胜败乃兵家常事 请重新来过。")
            return True
        else:
            return False
```
monster.py
```
from random import randint


class Monster:
    def __init__(self, name,flood,strength):
        self.name = name
        self.flood = flood
        self.strength = strength

    def calc_health(self):
        return self.flood

    def take_damage(self, hero):
        dmage = randint(hero.strength - 5, hero.strength + 5)
        self.flood -= dmage
        print(f"{self.name}你被{hero.name}攻击，受到了{str(dmage)}点伤害!还剩{str(self.flood)}滴血")
        if self.calc_health() <= 0:
            print(f"{self.name}你被杀死了!胜败乃兵家常事 请重新来过。")
            return True
        else:
            return False

```

pkgame.py
```


from hero import Hero
from monsters import Monster

hero = Hero('张三',100, 10)
monster = Monster('小强',60,20)

while True:
    is_monster_died = monster.take_damage(hero)
    if is_monster_died:
        break
    is_hero_died = hero.take_damage(monster)
    if is_hero_died:
        break
```
某次输出:
```
小强你被张三攻击，受到了6点伤害!还剩54滴血
张三你被小强攻击，受到了25点伤害!还剩75滴血
小强你被张三攻击，受到了9点伤害!还剩45滴血
张三你被小强攻击，受到了19点伤害!还剩56滴血
小强你被张三攻击，受到了10点伤害!还剩35滴血
张三你被小强攻击，受到了23点伤害!还剩33滴血
小强你被张三攻击，受到了11点伤害!还剩24滴血
张三你被小强攻击，受到了22点伤害!还剩11滴血
小强你被张三攻击，受到了10点伤害!还剩14滴血
张三你被小强攻击，受到了24点伤害!还剩-13滴血
张三你被杀死了!胜败乃兵家常事 请重新来过。
```


### 类属性 类方法 静态方法
#### 类对象，实例对象

类对象：类名
实例对象：类创建的对象

类属性就是`类对象`所拥有的属性，它被所有`类对象`的`实例对象`所共有，在内存中只存在一个副本，这个和C++、Java中类的静态成员变量有点类似。对于公有的类属性，在类外可以通过`类对象`和`实例对象`访问

```
# 类属性  
class people:  
    name="Tom"    #公有的类属性  
    __age=18      #私有的类属性  
  
p=people()  
print(p.name)   #实例对象  
print(people.name)  #类对象  
  
# print(p.__age)    #错误 不能在类外通过实例对象访问私有的类属性  
print(people.__age) #错误 不能在类外同过类对象访问私有的类属性 
```
实例属性

```
class people:  
    name="tom"  
  
p=people()  
p.age=18  
  
print(p.name)  
print(p.age)    #实例属性是实例对象特有的，类对象不能拥有  
  
print(people.name)  
#print(people.age)  #错误：实例属性，不能通过类对象调用  
```
我们经常将实例属性放在构造方法中 

```
class people:  
    name="tom"  
    def  __init__(self,age):  
        self.age=age  
  
p=people(18)  
  
print(p.name)  
print(p.age)    #实例属性是实例对象特有的，类对象不能拥有  
  
print(people.name)  
# print(people.age)  #错误：实例属性，不能通过类对象调用  
```

 类属性和实例属性混合

```
class people:  
    name="tom"      #类属性:实例对象和类对象可以同时调用  
    def  __init__(self,age):    #实例属性  
        self.age=age  
  
p=people(18)    #实例对象  
p.sex="男"       #实例属性  
  
print(p.name)  
print(p.age)    #实例属性是实例对象特有的，类对象不能拥有  
print(p.sex)  
  
print(people.name)  #类对象  
# print(people.age)  #错误：实例属性，不能通过类对象调用  
# print(people.sex)  #错误：实例属性，不能通过类对象调用  
```
如果在类外修改类属性，必须通过类对象去引用然后进行修改。如果通过实例对象去引用，会产生一个同名的实例属性，这种方式修改的是实例属性，不会影响到类属性，并且如果通过实例对象引用该名称的属性，实例属性会强制屏蔽掉类属性，即引用的是实例属性，除非删除了该实例属性


```
class Animal:  
    name="Panda"  
  
print(Animal.name)  #类对象引用类属性  
p=Animal()  
print(p.name)       #实例对象引用类属性时，会产生一个同名的实例属性  
p.name="dog"        #修改的只是实例属性的，不会影响到类属性  
print(p.name)         #dog  
print(Animal.name)    #panda  
  
# 删除实例属性  
del p.name  
print(p.name)  
```
#### 类属性的应用场景
比如，我们有一个班级类，创建班级对象的时候，需要按序号指定班级名称，我们就需要知道当前已经创建了多少个班级对象，这个数量可以设计成类属性
```
class NeuEduClass:
    class_num = 0

    def __init__(self):
        self.class_name = f'东软睿道Python{NeuEduClass.class_num+1}班'
        NeuEduClass.class_num += 1


classList = [NeuEduClass() for i in range(10)]
for c in classList:
    print(c.class_name)
```
输出
```
东软睿道Python1班
东软睿道Python2班
东软睿道Python3班
东软睿道Python4班
东软睿道Python5班
东软睿道Python6班
东软睿道Python7班
东软睿道Python8班
东软睿道Python9班
东软睿道Python10班
```

#### 类方法

类对象所拥有的方法，需要用修饰器`@classmethod`来标识其为类方法，对于类方法，第一个参数必须是类对象，一般以`cls`作为第一个参数（当然可以用其他名称的变量作为其第一个参数，但是大部分人都习惯以'cls'作为第一个参数的名字），能够通过实例对象和类对象去访问。 

```
class people:  
    country="china"  
  
    @classmethod  
    def getCountry(cls):  
        return cls.country  
  
p=people()  
print(p.getCountry())   #实例对象调用类方法  
print(people.getCountry())  #类对象调用类方法  
```

 类方法还有一个用途就是可以对类属性进行修改： 

```
class people:  
    country="china"  
  
    @classmethod  
    def getCountry(cls):  
        return cls.country  
    @classmethod  
    def setCountry(cls,country):  
        cls.country=country  
  
  
p=people()  
print(p.getCountry())   #实例对象调用类方法  
print(people.getCountry())  #类对象调用类方法  
  
p.setCountry("Japan")  
  
print(p.getCountry())  
print(people.getCountry())  
```

####静态方法

需要通过修饰器`@staticmethod`来进行修饰，静态方法不需要定义参数 

```
class people3:  
    country="china"  
  
    @staticmethod  
    def getCountry():  
        return people3.country  
  
p=people3()  
print(p.getCountry())   #实例对象调用类方法  
print(people3.getCountry())  #类对象调用类方法  
```

### 继承
继承的概念：子类 自动拥有（继承） 父类 的所有 方法 和 属性
1) 继承的语法
```
class 类名(父类名):
    pass
```
- 子类 继承自 父类，可以直接 享受 父类中已经封装好的方法，不需要再次开发
- 子类 中应该根据 职责，封装 子类特有的 属性和方法
- 当 父类 的方法实现不能满足子类需求时，可以对方法进行 重写(override)

举一个常见的例子。Circle 和 Rectangle ，不同的图形，面积（area）计算方式不同。

```
import math


class Circle:
    def __init__(self, color, r):
        self.r = r
        self.color = color

    def area(self):
        return math.pi * self.r * self.r

    def show_color(self):
        print(self.color)


class Rectangle:
    def __init__(self, color, a, b):
        self.color = color
        self.a, self.b = a, b

    def area(self):
        return self.a * self.b

    def show_color(self):
        print(self.color)

circle = Circle('red', 3.0)
print(circle.area())
circle.show_color()
rectangle = Rectangle('blue', 2.0, 3.0)
print(rectangle.area())
rectangle.show_color()
```
输出
```
28.274333882308138
red
6.0
blue
```
我们看到Rectangle和Circle有同样的属性color和方法show_color
我们可以定义一个父类Shape，将Rectangle和Circle通用的部分提取到Shape类中，然后在子类的init方法中，通过 调用super()\__init\__(color). 把 color 传给父类的 \__init\__()
```
import math


class Shape:
    def __init__(self, color):
        self.color = color

    def area(self):
        return None

    def show_color(self):
        print(self.color)


class Circle(Shape):
    def __init__(self, color, r):
        super().__init__(color)
        # Shape.__init__(self,color) #这样也行，但是不好（考虑父类Shape的名字改变了，怎么办）
        self.r = r

    def area(self):
        return math.pi * self.r * self.r




class Rectangle(Shape):
    def __init__(self, color, a, b):
        super().__init__(color)
        # Shape.__init__(self, color)   #这样也行，但是不好（考虑父类Shape的名字改变了，怎么办）
        self.a, self.b = a, b

    def area(self):
        return self.a * self.b



circle = Circle('red', 3.0)
print(circle.area())
circle.show_color()
rectangle = Rectangle('blue', 2.0, 3.0)
print(rectangle.area())
rectangle.show_color()


```
输出
```
28.274333882308138
red
6.0
blue
```
子类Circle和Rectangle本身并没有定义show_color方法， 从父类Shape继承了show_color方法。子类Circle和Rectangle改写(Override)了父类的area方法，分别提供了自己不同的实现。

注释掉Circle类中init方法中super().\__init\__()这行代码：
```
class Circle(Shape):
    def __init__(self, color, r):
        # super().__init__(color)
        # Shape.__init__(self,color) #这样也行，但是不好（考虑父类Shape的名字改变了，怎么办）
        self.r = r
```
再次运行代码，报错：
```
  File "C:/Users/Administrator/PycharmProjects/untitled16/shape.py", line 12, in show_color
    print(self.color)
AttributeError: 'Circle' object has no attribute 'color'
```
错误输出指定的出错位置在：`print(self.color)`，说Circle类型对象没有color属性
```
class Shape:
    def show_color(self):
        print(self.color)
```
问题原因是，Circle类型重写了父类Shape的\__init\__()方法，导致父类Shape的\__init\__()方法没有被调用到，所以
>一定不要忘记在子类的init方法中调用super().\__init\__()


#### 重构英雄pk怪物
我们发现Hero类和Monster类中有很多代码是重复的，我们把这些重复的代码提取到一个父类Sprite（精灵）中。
sprite.py
```
from random import randint


class Sprite:

    def __init__(self, flood,strength):
        self.flood = flood
        self.strength = strength

    def calc_health(self):
        return self.flood

    def take_damage(self, attack_sprite):
        damage = randint(attack_sprite.strength - 5, attack_sprite.strength + 5)
        self.flood -= damage
        print(f"{self.name}你被{attack_sprite.name}攻击，受到了{str(damage)}点伤害!还剩{str(self.flood)}滴血")
        if self.calc_health() <= 0:
            print(f"{self.name}你被杀死了!胜败乃兵家常事 请重新来过。")
            return True
        else:
            return False

```
hero.py
```

from sprite import Sprite


class Hero(Sprite):
    def __init__(self, name,flood,strength):
        self.name = name
        super().__init__(flood,strength)


```
monster.py
```


from sprite import Sprite


class Monster(Sprite):
    def __init__(self, name,flood,strength):
        self.name = name
        super().__init__(flood, strength)
```
pkgame.py不变
```

from hero import Hero
from monsters import Monster

hero = Hero('张三',100, 10)
monster = Monster('小强',60,20)

while True:
    is_monster_died = monster.take_damage(hero)
    if is_monster_died:
        break
    is_hero_died = hero.take_damage(monster)
    if is_hero_died:
        break
```
输出
```
小强你被张三攻击，受到了5点伤害!还剩55滴血
张三你被小强攻击，受到了19点伤害!还剩81滴血
小强你被张三攻击，受到了9点伤害!还剩46滴血
张三你被小强攻击，受到了25点伤害!还剩56滴血
小强你被张三攻击，受到了12点伤害!还剩34滴血
张三你被小强攻击，受到了21点伤害!还剩35滴血
小强你被张三攻击，受到了5点伤害!还剩29滴血
张三你被小强攻击，受到了23点伤害!还剩12滴血
小强你被张三攻击，受到了13点伤害!还剩16滴血
张三你被小强攻击，受到了16点伤害!还剩-4滴血
张三你被杀死了!胜败乃兵家常事 请重新来过。
```
**进一步改造**
假设我们想伤害值damage的大小，不在Sprite类里统一处理，而由各个具体的精灵（Hero或Monster）自己来决定，代码可以改造成如下：
在Sprite类中添加get_damage_value方法，不提供实现
```
from random import randint


class Sprite:

    def __init__(self, flood,strength):
        self.flood = flood
        self.strength = strength

    def calc_health(self):
        return self.flood

    def get_damage_value(self):
        pass

    def take_damage(self, attack_sprite):
        # damage = randint(attack_sprite.strength - 5, attack_sprite.strength + 5)
        damage = attack_sprite.get_damage_value();
        self.flood -= damage
        print(f"{self.name}你被{attack_sprite.name}攻击，受到了{str(damage)}点伤害!还剩{str(self.flood)}滴血")
        if self.calc_health() <= 0:
            print(f"{self.name}你被杀死了!胜败乃兵家常事 请重新来过。")
            return True
        else:
            return False

```
hero.py提供自己get_damage_value方法的实现
```
from random import randint

from sprite import Sprite


class Hero(Sprite):
    def __init__(self, name,flood,strength):
        self.name = name
        super().__init__(flood,strength)

    def get_damage_value(self):
        return randint(self.strength - 3, self.strength + 3)
```
monster.py提供自己get_damage_value方法的实现
```
from random import randint

from sprite import Sprite


class Monster(Sprite):
    def __init__(self, name,flood,strength):
        self.name = name
        super().__init__(flood, strength)

    def get_damage_value(self):
        return randint(self.strength - 7, self.strength + 7)

```
输出
```
小强你被张三攻击，受到了8点伤害!还剩52滴血
张三你被小强攻击，受到了13点伤害!还剩87滴血
小强你被张三攻击，受到了11点伤害!还剩41滴血
张三你被小强攻击，受到了19点伤害!还剩68滴血
小强你被张三攻击，受到了7点伤害!还剩34滴血
张三你被小强攻击，受到了22点伤害!还剩46滴血
小强你被张三攻击，受到了9点伤害!还剩25滴血
张三你被小强攻击，受到了25点伤害!还剩21滴血
小强你被张三攻击，受到了9点伤害!还剩16滴血
张三你被小强攻击，受到了24点伤害!还剩-3滴血
张三你被杀死了!胜败乃兵家常事 请重新来过。
```

###\__new\__方法
python中定义的类在创建实例对象的时候，会自动执行__init__()方法，但是在执行__init__()方法之前，会执行__new__()方法。

\__new\__()的作用主要有两个。

1.在内存中为对象分配空间
2.返回对象的引用。（即对象的内存地址）

python解释器在获得引用的时候会将其传递给__init__()方法中的self。

```
class A:
    def __new__(cls,*args,**kwargs):
        print('__new__')
        return super().__new__(cls)#这里一定要返回，否则__init__()方法不会被执行
    def __init__(self):#这里的self就是new方法中的return返回值
        print('__init__')

a = A()
 ```
输出结果
```
__new__
__init__
```
我们一定要在\__new\__方法中最后调用
```
return super().__new__(cls)
```
否则init方法不会被调用
```
class A:
    def __new__(cls,*args,**kwargs):
        print('__new__')
        # return super().__new__(cls)#这里一定要返回，否则__init__()方法不会被执行
    def __init__(self):#这里的self就是new方法中的return返回值
        print('__init__')


a = A()

```
输出
```
__new__
```
像以前一样，我们不写\__new\__方法试试
```
class A:
    # def __new__(cls,*args,**kwargs):
    #     print('__new__')
        # return super().__new__(cls)#这里一定要返回，否则__init__()方法不会被执行
    def __init__(self):#这里的self就是new方法中的return返回值
        print('__init__')


a = A()

```
输出
```
__init__
```
### object---所有python类型的父类
在Python中，所有类型有个隐式的父类（`object`），上面的代码相当于
```
class A(object):
    # def __new__(cls,*args,**kwargs):
    #     print('__new__')
        # return super().__new__(cls)#这里一定要返回，否则__init__()方法不会被执行
    def __init__(self):#这里的self就是new方法中的return返回值
        print('__init__')

a = A()
```
当我们的类中没有定义\__new\__方法时，创建对象时，Python会先调用父类（object）的\__new\__方法，在内存中创建一个实例，然后传递给我们的init方法。如果我们的自定义类型定义了\__new\__方法，就会覆盖父类（object）的\__new\__方法（父类的\__new\__方法不会被自动调用），所以我们要显式调用父类（object）的\__new\__方法，在内存中创建一个实例。

那么new有什么作用呢？我们可以改写它，举个例子，就比如说单例模式。
### 单例模式
如果我们创建两个实例：
```
a1 = A()
a2 = A()
```
那么id(a1)和id(a2)的值不一样，也就是说python在内容当中创建了两个实例对象，用了两份内存。同样的东西创建了两份

如果想不管创建多少个实例对象，我们都让它的id是一样的。

也就是说，先创建一个实例对象，之后不管创建多少个，返回的永远都是第一个实例对象的内存地址。可以这样实现：

```
# 重写new方法很固定，返回值必须是这个
# 这样就避免了创建多份。
# 创建第一个实例的时候，_instance是None，那么会分配空间创建实例。
# 此时的类属性已经被修改，_instance不再为None
# 那么当之后实例属性被创建的时候，由于_instance不为None。
# 则返回第一个实例对象的引用，即内存地址。
# 这样就应用了单例模式。
class A():
    _instance = None
    def __new__(cls,*args,**kwargs):
        if A._instance == None:
            A._instance = super().__new__(cls)
        return A._instance


a1 = A()
print(id(a1))
a2 = A()
print(id(a2))
```

### 函数参数注解

你写好了一个函数，然后想为这个函数的参数增加一些额外的信息（每个参数的类型），这样的话其他调用者就能清楚的知道这个函数应该怎么使用。

**解决方案**

使用函数参数注解是一个很好的办法，它能提示程序员应该怎样正确使用这个函数。 例如，下面有一个被注解了的函数：
```
def add(x:int, y:int) -> int:
    return x + y
```

python解释器不会对这些注解添加任何的语义。它们不会被类型检查，运行时跟没有加注解之前的效果也没有任何差距。 然而，对于那些阅读源码的人来讲就很有帮助啦。第三方工具和框架可能会对这些注解添加语义。同时它们也会出现在文档中。
```
>>> help(add)
Help on function add in module __main__:
add(x: int, y: int) -> int
>>>
```

尽管你可以使用任意类型的对象给函数添加注解(例如数字，字符串，对象实例等等)，不过通常来讲使用类或者字符串会比较好点。


函数注解只存储在函数的 __annotations__ 属性中。例如：
```
>>> add.__annotations__
{'y': <class 'int'>, 'return': <class 'int'>, 'x': <class 'int'>}
>>>
```

尽管注解的使用方法可能有很多种，但是它们的主要用途还是文档。 因为python并没有类型声明，通常来讲仅仅通过阅读源码很难知道应该传递什么样的参数给这个函数。 这时候使用注解就能给程序员更多的提示，让他们可以正确的使用函数。

>函数参数注解还给我们带来的好处有使得pycharm这样的IDE可以自动提示该类型对象拥有的方法。
```
class A:
    def dosomething(self):
        print('hello')


def foo(a : A):
    a.dosomething()

a = A()
foo(a)

```
在foo函数的定义中，如果我们没有指定a:A，当我们输入a.时，IDE提示不出来dosomething方法。

### 扑克牌模拟
![image.png](http://upload-images.jianshu.io/upload_images/468490-153c0918ba206add.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**任务目的**
0.`培养编程思维，提高分析问题能力`
1.掌握类的抽象与设计
2.掌握循环，分支条件的用法
3.掌握各种集合类的使用（Map，List，Set）


**任务描述**

1.定义一个单张扑克类（考虑需要哪些属性），定义一个一副扑克牌类，该类包含一个单张扑克对象的数组（不考虑大小王）。实现一个模拟扑克发牌洗牌的算法；
2.电脑随机发出5张牌，判断是以下哪种牌型？（提示，利用Map，List，Set等各种集合的特性可以简化判断）

![image.png](http://upload-images.jianshu.io/upload_images/468490-994b5d07768fe4f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/468490-afdb08f8d8cc8b01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


输出
![image.png](https://upload-images.jianshu.io/upload_images/468490-b035af1dc3cd52a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**参考代码**
```
import random

colorLst = ['红桃', '方片', '黑桃', '草花']


class Card:
    def __init__(self, color, value):
        self.value = value
        self.color = color

    def __str__(self):
        strvalue = str(self.value)
        if self.value == 11:
            strvalue = 'J'
        elif self.value == 12:
            strvalue = 'Q'
        elif self.value == 13:
            strvalue = 'K'
        elif self.value == 1:
            strvalue = 'A'

        return self.color + str(strvalue)


class Poke:
    def __init__(self):
        self.cards = []

        for color in colorLst:
            for value in range(1, 14):
                card = Card(color, value)
                self.cards.append(card)

    def output(self):
        index = 1
        for card in self.cards:
            print(card, end='\t')

            if index % 13 == 0:
                print()

            index += 1


poke = Poke()
poke.output()

random.shuffle(poke.cards)
print('洗牌之后')
poke.output()

hands = poke.cards[:5]

print('分到的一手牌是')
for card in hands:
    print(card, end='\t')

print('\n牌型是：', end='')

bSameColor = False
bShunZi = False
cardColors = [card.color for card in hands]
cardValues = [card.value for card in hands]


cardValuesSet = set(cardValues)
cardColorsSet = set(cardColors)
if len(cardColorsSet) == 1:
    bSameColor = True

cardValuesSorted = sorted(cardValues)


if cardValuesSorted[4] - cardValuesSorted[0] == 4 and len(cardValuesSet) == 5:
    bShunZi = True

if bSameColor and bShunZi:
    print('同花顺')
    exit(0)
if bSameColor:
    print('同花')
    exit(0)
if bShunZi:
    print('顺子')
    exit(0)


if len(cardValuesSet) == 4:
    print('一对')
if len(cardValuesSet) == 5:
    print('杂牌')

if len(cardValuesSet) == 2:
    if cardValues.count(cardValues[0]) == 1 or cardValues.count(cardValues[0]) == 4:
        print('四带一')
    else:
        print('三带二')

bisThreeOneOne = False
if len(cardValuesSet) == 3:
    for value in cardValues:
        if cardValues.count(value) == 3:
            bisThreeOneOne = True
            break

    if bisThreeOneOne == False:
        print('221')
    else:
        print('311')

```
### 综合实战---东软睿道运营仿真
**需求**

使用python面向对象思想，模拟仿真东软睿道公司运营整个过程
两个目的
- 综合运用学习过的知识点
- 运用面向对象思维解决实际问题

模拟睿道培训机构，从招生，到开班，到学员学习课程，到学员毕业，计算公司总收入，统计学员人数

一.类的识别
1.学生类
2.班级类
3.课程类
4.睿道公司类
5.收入明细类：

二.类的成员的定义（包括属性和方法）
1.学生类Student：姓名，经验值，勤奋程度（0.9），聪明程度（1.9），是否进入班级（true）
2.班级类Class：学员列表（List<Student>），开班日期，
3.课程类Course：课程名称，课时。
4.睿道公司类Company：班级列表(List<Class>)
,课程列表(List<Course>),总收入，收入明细。学员列表，未进入班级的学员列表
5.收入明细类：收入金额，收入日志
三.类的成员方法的定义
1.学生类：学习，缴费
2.班级类：开班，关班，上课
3.课程类：课程名，课时
4.睿道公司类：统计在校人数，统计全年收入，统计班级数....
5.收入明细类：

四.启动一个定时器，模拟东软睿道公司的运营情况
每一秒执行一个timer函数，模拟睿道一天的运营状况（睿道的一天）：
1.招生（随机生成一个个学员对象，加入未开班班级，调用学员的缴费方法）
2.开班（需要判断是否满足条件[学员数量达到某个阈值]）
3.各班级上课。（判断关班[所有课程全部完毕]）
course.py
```
class Course:
    def __init__(self,course_name,hour):
        self.hour = hour
        self.course_name = course_name
    def __str__(self):
        return self.course_name
```
course_table.py
```
from course import Course


class CourseTable:
    def __init__(self):
        self.tables = []
        self.tables.append(Course("python", 5))
        self.tables.append(Course("mysql", 3))
        self.tables.append(Course("flask", 5))
        self.tables.append(Course("django", 3))
        self.tables.append(Course("tensorflow", 5))
        self.tables.reverse()
```
student.py
```
# 学生类Student：姓名，经验值，勤奋程度（0.9），聪明程度（1.9），是否进入班级（true）
# 学生类：学习，缴费

class Student:
    def __init__(self):
        self.name = ''
        self.point = 0
        self.hard_degree = 0
        self.smart_degree = 0
        self.is_enter_class = False

    def pay_money(self):
        return 20000

    def study(self):
        if self.is_enter_class:
            self.point += 10 * self.hard_degree * self.smart_degree
            print(f'{self.name}的经验值已经增长到{self.point}')

```
neuedu_class.py
```
from datetime import datetime

from course_table import CourseTable


# // 2.班级类Class：学员列表（List<Student>），开班日期，
# //2.班级类：开班，关班，上课

class NeuEduClass:
    class_num = 0

    def __init__(self):
        self.class_name = ''
        self.start_date = datetime.now()
        self.students = []
        self.course_table = CourseTable()

        # //从课程表里取出一个课程，该课程课时减1，判断课时是否为0，如果大于0，重新放回课程表，
        # //如果课程表为空，关闭班级

    def stand_up(self):
        is_closed = False
        c = self.course_table.tables.pop()
        print(f'{self.class_name}正在上{c.course_name}课，还有{c.hour}课时')
        hour = c.hour - 1
        if hour > 0:
            c.hour = hour
            self.course_table.tables.append(c)

        if len(self.course_table.tables) == 0:
            is_closed = True

        return is_closed

```

income_record.py
```
# //收入金额，收入日志
# //收到张三 10000
# //收到李四 10000
# //收到王五 10000

class IncomeRecord:
    def __init__(self):
        self.income = 0
        self.log = ''

```
company.py
```
# //4.睿道公司类Company：班级列表(List<Class>)
# //,课程列表(List<Course>),总收入，收入明细(List<IncomeRecord>)。学员列表，未进入班级的学员列表
#     //睿道公司类：统计在校人数，统计全年收入，统计班级数....
from random import randint

from income_record import IncomeRecord
from neuedu_class import NeuEduClass
from student import Student


from time import time

class Company:
    def __init__(self):
        self.classes = []
        self.courses = []
        self.income = 0
        self.income_records = []
        self.inschool_students = []
        self.noclass_students = [] #交费了但还没有没有上课的学生列表（开班人数不够）
        self.num = 5 #达到多少人开班

    def add_money(self, student:Student):
        print(f'财务部收款{student.pay_money()}元')
        self.income += student.pay_money()
        ir = IncomeRecord()
        ir.income = student.pay_money()
        ir.log = f'收到{student.name} 学费 {student.pay_money()}元'
        self.income_records.append(ir)
        # 加入一个学生到未上课学员列表，考察当前列表长度，如果大于等于30人，开班
    def add_student_to_noclasslist(self, student:Student):
        self.noclass_students.append(student)
        size = len(self.noclass_students)
        print(f'当前招生人数{size}')
        if size >= self.num:
            # 新建一个班
            c = NeuEduClass()
            c.class_name = f'{NeuEduClass.class_num}班'
            NeuEduClass.class_num += 1
            self.classes.append(c)
            print(f'{c.start_date}:{c.class_name}开班了')

            # 转移学生，从lstNoClassStudents -->c
            for stud in self.noclass_students:
                stud.is_enter_class = True
                c.students.append(stud)

            self.noclass_students.clear()
    # //招生
    # //new 一个学生对象，返回。
    # //随机产生一个学员，先生成一个0-1之间的随机数
    # //如果该随机数小于0.7，就返回一个新学员对象
    # //否则，None
    # 招生成功率70 %
    def get_student(self):
        n = randint(1, 10)
        if n <= 7:
            # // 根据系统当前时间生成一个时间戳，保证用户名不重复
            # // 时间戳是当前时间距离19700101经过的毫秒数
            stu = Student()
            stu.name = f'张{time()}'
            stu.point = n * 10
            stu.hard_degree = randint(1, 10) #初始勤奋度是1-10,5属于正常水平
            stu.smart_degree = randint(1, 10) #初始聪明度是1-10,5属于正常水平
            stu.is_enter_class = False
            print(f'学员{stu.name}加入睿道学习，等待新班')
            return stu;
        else:
            print(f'很遗憾，本次招生没有成功')
            return None
```
day_of_neuedu.py
```
import time

from company import Company

import threading

# //        四.在main函数中启动一个定时器，模拟我们睿道公司的运营情况
# //        每一秒执行一个timer函数模拟睿道一天的运营状况（睿道的一天）。
# //            1.招生（随机生成一个个学员对象，加入为开班班级，调用学员的缴费方法）；
# //            2.开班（需要判断）
# //            3.各班级上课。（判断关班）
index = 0
def day_of_neuedu(company:Company):
    global index
    index += 1
    if index % 7 == 0:
        student_num = len(company.income_records)
        total_income = sum(map(lambda x:x.income,company.income_records))

        print('-' * 30 + f'周末休息' + '-' * 30)
        print(f'\033[1;35m截止目前共有{student_num}名学员来到睿道学习，公司总营收收入{total_income}元\033[0m')
        print('-' * 30 + f'周末休息' + '-' * 30)
        time.sleep(3)

    print('-' * 30 + f'东软睿道美好的一天开始了，各部门员工紧张忙碌了起来' + '-' * 30)

    print('市场部开始接待客户')
    student = company.get_student()
    if student:
        company.add_money(student)
        company.add_student_to_noclasslist(student)


    print(f"当前有{len(company.classes)}个班级在上课")
    # 存储需要关闭的班级
    class_removed = []

    if len(company.classes) > 0:
        print('实施部的大咖们开始给学员上课了')

    for c in company.classes:
        is_closed = c.stand_up()
        for stud in c.students:
            stud.study()
        if is_closed:
            class_removed.append(c)

    for c_del in class_removed:
        print(f'{c_del.class_name}学生结业了，全部找到高薪工作，走上人生巅峰，迎娶白富美')
        company.classes.remove(c_del)

    global timer
    timer = threading.Timer(1, day_of_neuedu, [company])
    timer.start()



company = Company()
timer = threading.Timer(1, day_of_neuedu, [company])  #首次启动
#启用定时器
timer.start()
```

**补充---python中的定时器**
```
#引入库 threading
import threading
#定义函数
def fun_timer(msg,count):
    print('hello timer'+msg+str(count))   #打印输出
    global timer  #定义变量
    timer = threading.Timer(1,fun_timer,[msg,count])   
    timer.start()    #启用定时器
    
    
#定时器构造函数主要有2个参数，第一个参数为时间处理函数，第二个参数为时间处理函数函数名
#第三个参数为传递给fun_timer的参数列表
#1秒后调用函数fun_timer
timer = threading.Timer(1,fun_timer,['hello',5])  #首次启动
timer.start()
```

