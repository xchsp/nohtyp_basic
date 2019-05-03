
## 一.条件语句

Python 条件语句跟其他语言基本一致的，都是通过一条或多条语句的执行结果（ True 或者 False ）来决定执行的代码块。

Python 程序语言指定任何非 0 和非空（None）值为 True，0 或者 None为 False。



### 1、if 语句的基本形式

Python 中，if 语句的基本形式如下：

```
if 判断条件：
    执行语句……
else：
    执行语句……

```

前面也提到过，Python 语言有着严格的缩进要求，因此这里也需要注意缩进，也不要少写了冒号 `:` 。

if 语句的判断条件可以用>（大于）、<(小于)、==（等于）、>=（大于等于）、<=（小于等于）来表示其关系。

例如：

```
# -*-coding:utf-8-*-

results=59

if results>=60:
    print ('及格')
else :
    print ('不及格')

```

输出的结果为：

```
不及格

```

上面也说道，非零数值、非空字符串、非空 list 等，判断为True，否则为False。因此也可以这样写：

```
num = 6
if num :
    print('Hello Python')

```

### 2、if 语句多个判断条件的形式

有些时候，我们的判断语句不可能只有两个，有些时候需要多个，比如上面的例子中大于 60 的为及格，那我们还要判断大于 90 的为优秀，在 80 到 90 之间的良好呢？

这时候需要用到 if 语句多个判断条件，

用伪代码来表示：

```
if 判断条件1:
    执行语句1……
elif 判断条件2:
    执行语句2……
elif 判断条件3:
    执行语句3……
else:
    执行语句4……

```

实例：

```
# -*-coding:utf-8-*-

results = 89

if results > 90:
    print('优秀')
elif results > 80:
    print('良好')
elif results > 60:
    print ('及格')
else :
    print ('不及格')

```

输出的结果：

```
良好

```

### 3、if 语句多个条件同时判断

Python 不像 Java 有 switch 语句，所以多个条件判断，只能用 elif 来实现，但是有时候需要多个条件需同时判断时，可以使用 or （或），表示两个条件有一个成立时判断条件成功；使用 and （与）时，表示只有两个条件同时成立的情况下，判断条件才成功。

```
# -*-coding:utf-8-*-

java = 86
python = 68

if java > 80 and  python > 80:
    print('优秀')
else :
    print('不优秀')

```

输出结果：

```
不优秀
```

注意：if 有多个条件时可使用括号来区分判断的先后顺序，括号中的判断优先执行，此外 and 和 or 的优先级低于 >（大于）、<（小于）等判断符号，即大于和小于在没有括号的情况下会比与或要优先判断。

```
java = 86
python = 68

if (80 <= java < 90) or (80 <= python < 90):
    print('良好')
```

输出结果：

```
良好
```
***课上练习1***
```
我想买车，买什么车决定于我在银行有多少存款
如果我的存款超过500万，我就买路虎
否则，如果我的存款超过100万，我就买宝马
否则， 如果我的存款超过50万，我就买迈腾
否则， 如果我的存款超过10万，我就买福特
否则， 如果我的存款10万以下 ，我买比亚迪
```
***课上练习2***
```
输入小明的考试成绩，显示所获奖励
成绩==100分，爸爸给他买辆车
成绩>=90分，妈妈给他买MP4
90分>成绩>=60分，妈妈给他买本参考书
成绩<60分，什么都不买
```

## 二.循环语句

一般编程语言都有循环语句，循环语句允许我们执行一个语句或语句组多次。

循环语句的一般形式如下：

![](http://upload-images.jianshu.io/upload_images/2136918-eaaae2fbfec3330f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Python 提供了 for 循环和 while 循环，当然还有一些控制循环的语句：

| 循环控制语句 | 描述 |
| --- | --- |
| break | 在语句块执行过程中终止循环，并且跳出整个循环 |
| continue | 在语句块执行过程中终止当前循环，跳出该次循环，执行下一次循环 |
| pass | pass 是空语句，是为了保持程序结构的完整性 |

### 1、While 循环语句

```
count = 1
sum = 0
while count <= 100:
    sum = sum + count
    count = count + 1
print(sum)

```

输出的结果：

```
5050

```

当然 while 语句时还有另外两个重要的命令 continue，break 来跳过循环，continue 用于跳过该次循环，break 则是用于跳出本层循环

比如，上面的例子是计算 1 到 100 所有整数的和，当我们需要判断 sum 大于 1000 的时候，不在相加时，可以用到 break ，退出整个循环

```
count = 1
sum = 0
while  count <= 100:
    sum = sum + count
    if  sum > 1000:  #当 sum 大于 1000 的时候退出循环
        break
    count = count + 1
print(sum)

```

输出的结果：

```
1035

```

有时候，我们只想统计 1 到 100 之间的奇数和，那么也就是说当 count 是偶数，也就是双数的时候，我们需要跳出当次的循环，不想加，这时候可以用到 continue

```
count = 1
sum = 0
while  count <= 100:
    if count % 2 == 0:  # 双数时跳过输出
        count = count + 1
        continue
    sum = sum + count
    count = count + 1
print(sum)

```

输出的语句：

```
2500

```

在 Python 的 while 循环中，还可以使用 else 语句，while … else 在循环条件为 false 时执行 else 语句块

比如：

```
count = 0
while count < 5:
   print (count)
   count = count + 1
else:
   print (count)

```

输出的结果：

```
0
1
2
3
4
5

```

### 2、 for 循环语句

for循环可以遍历任何序列的项目，如一个字符串

它的流程图基本如下：

![](http://upload-images.jianshu.io/upload_images/2136918-a0728c1c488238af?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

基本的语法格式：

```
for iterating_var in sequence:
   statements(s)

```

实例：

```
for letter in 'www.neuedu.com':
    print(letter)

```

输出的结果如下：

```
w
w
w
.
n
e
u
e
d
u
.
c
o
m

```
`range()函数`

Python函数`range()`让你能够轻松地生成一系列的数字。例如，可以像下面这样使用函数`range()`来打印一系列的数字：

```
    for value in range(1,5):
        print(value)
```

上述代码好像应该打印数字1~5，但实际上它不会打印数字5：
```
    1
    2
    3
    4
```

在这个示例中，`range()`只是打印数字1~4，这是你在编程语言中经常看到的差一行为的结果。函数`range()`让Python从你指定的第一个值开始数，并在到达你指定的第二个值后停止，因此输出不包含第二个值（这里为5）。

要打印数字1~5，需要使用`range(1,6)`：
```
    for value in range(1,6):
        print(value)
```

这样，输出将从1开始，到5结束：
```
    1
    2
    3
    4
    5
```

### 3、嵌套循环

Python 语言允许在一个循环体里面嵌入另一个循环。上面的实例也是使用了嵌套循环的。

具体的语法如下：

**for 循环嵌套语法**

```
for iterating_var in sequence:
   for iterating_var in sequence:
      statements(s)
   statements(s)

```

**while 循环嵌套语法**

```
while expression:
   while expression:
      statement(s)
   statement(s)

```

除此之外，你也可以在循环体内嵌入其他的循环体，如在 while 循环中可以嵌入 for 循环， 反之，你可以在 for 循环中嵌入 while 循环

有 while … else 语句，当然也有 for … else 语句啦，for 中的语句和普通的没有区别，else 中的语句会在循环正常执行完（即 for 不是通过 break 跳出而中断的）的情况下执行，while … else 也是一样。

```
for num in range(10,20):  # 迭代 10 到 20 之间的数字
   for i in range(2,num): # 根据因子迭代
      if num%i == 0:      # 确定第一个因子
         j=num/i          # 计算第二个因子
         print ('%d 是一个合数' % num)
         break            # 跳出当前循环
   else:                  # 循环的 else 部分
      print ('%d 是一个质数' % num)

```

输出的结果：

```
10 是一个合数
11 是一个质数
12 是一个合数
13 是一个质数
14 是一个合数
15 是一个合数
16 是一个合数
17 是一个质数
18 是一个合数
19 是一个质数

```


**课上案例**
打印九九乘法表
```
# 打印九九乘法表
for i in range(1, 10):
        for j in range(1, i+1):
            print('{}x{}={}\t'.format(i, j, i*j), end='')
        print()
```

```
1x1=1   
2x1=2   2x2=4   
3x1=3   3x2=6   3x3=9   
4x1=4   4x2=8   4x3=12  4x4=16  
5x1=5   5x2=10  5x3=15  5x4=20  5x5=25  
6x1=6   6x2=12  6x3=18  6x4=24  6x5=30  6x6=36  
7x1=7   7x2=14  7x3=21  7x4=28  7x5=35  7x6=42  7x7=49  
8x1=8   8x2=16  8x3=24  8x4=32  8x5=40  8x6=48  8x7=56  8x8=64  
9x1=9   9x2=18  9x3=27  9x4=36  9x5=45  9x6=54  9x7=63  9x8=72  9x9=81
```


## 三.随机数的处理

* 在 `Python` 中，要使用随机数，首先需要导入 **随机数** 的 **模块** —— “工具包”

```python
import random
```

* 导入模块后，可以直接在 **模块名称** 后面敲一个 `.` ，会提示该模块中包含的所有函数

* `random.randint(a, b)` ，返回 `[a, b]` 之间的整数，包含 `a` 和 `b`
* 例如：

```python
random.randint(12, 20)  # 生成的随机数n: 12 <= n <= 20   
random.randint(20, 20)  # 结果永远是 20   
random.randint(20, 10)  # 该语句是错误的，下限必须小于上限
```



**综合练习---猜数字**

计算机要求用户输入数值范围的最小值和最大值。计算机随后“思考”出在这个范围之内的一个随机数，并且重复地要求用户猜测这个数，直到用户猜对了。在用户每次进行猜测之后，计算机都会给出一个提示，并且会在这个过程的最后显示出总的猜测次数。这个程序包含了几种类型的我们学过的 Python 语句，例如，输入语句、输出语句、赋值语句、循环和条件语句。

一个可能的游戏过程如下：
```
Enter the smaller number: 1
Enter the larger number: 20
Enter your guess: 5
Too small
Enter your guess: 9
Too small
Enter your guess: 15
Too small
Enter your guess: 17
Too large
Enter your guess: 16
You've got it in 5 tries!
```

```
import random

smaller = int(input("Enter the smaller number: "))
larger = int(input("Enter the larger number: "))
myNumber = random.randint(smaller, larger)
count = 0
while True:
    count += 1
    userNumber = int(input("Enter your guess: "))
    if userNumber < myNumber:
        print("Too small")
    elif userNumber > myNumber:
        print("Too large")
    else:
        print("You've got it in", count, "tries!")
        break
```
