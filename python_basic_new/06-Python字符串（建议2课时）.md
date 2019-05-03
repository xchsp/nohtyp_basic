
## 字符串的常用操作方法

###1、字符串的基本操作之切片
获取Python字符串中的某字符可以使用索引：
```
lang = '东软睿道'
print(lang[0])
print(lang[3])
```    
```
东
道
```
截取字符串中的一段字符串可以使用切片，切片在方括号中使用冒号:来分隔需要截取的首尾字符串的索引，方式是包括开头，不包括结尾
```
    lang[2:4]
    # 睿道
```    

当尾索引没有给出时，默认截取到字符串的末尾
```
    lang[2:]
    # 睿道
```    

当头索引没有给出的时候默认从字符串开头开始截取
```
    lang[:3]
    # 东软睿
```    

当尾索引和头索引都没有给出的时候，默认返回整个字符串，不过这只是一个浅拷贝
```
    lang[:]
    # 东软睿道
```    

当尾索引大于总的字符串长度时，默认只截取到字符串末尾，很明显使用这种方法来截取一段到字符串末尾的子字符串是非常不明智的，应该是不给出尾索引才是最佳实践
```
    lang[3:100]
    # 道
```    

当头索引为负数时，则是指从字符串的尾部开始计数，最末尾的字符记为-1，以此类推，因此此时应该注意尾索引的值，尾索引同样可以为负数，如果尾索引的值指明的字符串位置小于或等于头索引，此时返回的就是空字符串
```
    lang[-2:]
    # 睿道
    lang[-2,2]
    # ''
```
切片的第三个参数是步长
```
s = 'www.neuedu.com'

#### 切片取出来的字符串与原字符串无关，原字符串不变
print(s[6: 10])
print(s[7:: 2])
```
```
uedu
eucm
```
反向取数字需要加上反向步长
```
print(s[-1: -4: -1])
print(s[-1: 2]) #### 取不到数据，因为步长默认-1
print(s[-1: 2: -1])#### 可以
```
```
moc

moc.udeuen.
```
###2、把字符串全部大写或小写upper， lower
```
s = 'www.NEUEDU.com'
print(s.upper())    #### 全部大写
print(s.lower())    #### 全部小写
```
###3、判读以xx开头或结尾startswith，endswith
```
s = 'www.neuedu.com'
print(s.startswith('www'))    #### 判断是否以www开头
print(s.endswith('com'))    #### 判断是否以com结尾
```
```
True
True
```
###4、查找元素find ，index
```
s = 'chhengt'
print(s.find('h'))  #### 通过元素找索引找到第一个就返回（可切片）
print(s.find('b'))  #### 找不到返回 -1
print(s.index('b'))   #### 找不到会报错
```

```
Traceback (most recent call last):
1
  File "C:/Users/Administrator/PycharmProjects/textgame/tests/test_bag.py", line 4, in <module>
-1
    print(s.index('b'))   #### 找不到会报错
ValueError: substring not found
```
###5、strip 默认去除字符前后两端的空格， 换行符， tab
```
s = 'qqwalex qqwusir barryy'
print(s.strip('qqw'))
print(s.strip(''))
print(s.lstrip('yy'))
print(s.rstrip('yy'))
```
```
alex qqwusir barryy
qqwalex qqwusir barryy
qqwalex qqwusir barryy
qqwalex qqwusir barr
```
###6、split 把列表分割成字符串

```
#### 分割出的元素比分隔符数+1  ***
#### 字符串变成->>>列表
s = 'qqwalex qqwusir barryy'
s1 = 'qqwale;x qqwu;sir bar;ryy'
print(s.split())    #### 默认以空格分割
print(s1.split(';'))    #### 以指定字符分割
print(s1.split(';', 1)) #### 指定分割多少个
```
```
['qqwalex', 'qqwusir', 'barryy']
['qqwale', 'x qqwu', 'sir bar', 'ryy']
['qqwale', 'x qqwu', 'sir bar;ryy']
```
###7、join把字符串转成列表

```
#### 列表转化成字符串 list --> str
s = 'alex'####看成字符列表
li = ['aa', 'ddj', 'kk']    #### 必须全是字符串
s1 = '_'.join(s)
print(s1)
s2 = ' '.join(li)
print(s2)
```
```
a_l_e_x
aa ddj kk
```
###8、is系列

```
#### 字符串.isalnum()    所有字符都是数字或者字母，为真返回 Ture，否则返回 False。
#### 字符串.isalpha()     所有字符都是字母，为真返回 Ture，否则返回 False。
#### 字符串.isdigit()     所有字符都是数字，为真返回 Ture，否则返回 False。
#### 字符串.islower()    所有字符都是小写，为真返回 Ture，否则返回 False。
#### 字符串.isupper()    所有字符都是大写，为真返回 Ture，否则返回 False。
#### 字符串.istitle()            所有单词都是首字母大写，为真返回 Ture，否则返回 False。
#### 字符串.isspace()    所有字符都是空白字符，为真返回 Ture，否则返回 False。    

#### is 系列
name = 'taibai123'
print(name.isalnum())   #### 字符串由数字或字母组成时返回真
print(name.isalpha())   #### 字符只由字母组成时返回真
print(name.isdigit())   #### 字符只由数字组成时返回真
```
###9、count 计算字符串中某个字符出现的次数 ***
```
#### count 计算字符串中某个字符出现的次数  ***
s = 'www.neuedu.com'
print(s.count('u'))
print(s.count('u', 7))
```
```
2
1
```
###10、replace***  替换字符串中指定的字符
```
s = 'asdf 之一，asdf也，asdf'
#### replace  ***
s1 = s.replace('asdf', '日天')
print(s1)
s1 = s.replace('asdf', '日天', 1)
print(s1)
 ```
```
日天 之一，日天也，日天
日天 之一，asdf也，asdf
```

###11、format格式化输出

```
#### format 格式化输出  ***
#### 第一种方式：
s = '我叫{}, 今年{}， 性别{}'.format('小五', 25, '女')
print(s)
#### 第二种方式
s1 = '我叫{0}, 今年{1}， 性别{2}，我依然叫{0}'.format('小五', 25, '女')
print(s1)
#### 第三种方式
s2 = '我叫{name}, 今年{age}， 性别{sex}，我依然叫{name}'.format(age=25, sex='女',name='小五')
print(s2)
```
```
我叫小五, 今年25， 性别女
我叫小五, 今年25， 性别女，我依然叫小五
我叫小五, 今年25， 性别女，我依然叫小五
```

###12、capitalize() 首字母大写 **
```
s = 'chen wuang4fhsa￥fh。f'
#### capitalize() 首字母大写 **
s1 = s.capitalize()
print(s1)
 ```
```
Chen wuang4fhsa￥fh。f
```
###13、center() 将字符串居中可以设置总长度，可以设置填充物 *
```
#### center() 将字符串居中可以设置总长度，可以设置填充物 *
s = 'chenziwu'
s2 = s.center(50)
s2 = s.center(50, '*')
print(s2)
 ```
```
*********************chenziwu*********************
```
###14、title  非字母隔开的每个单词的首字母大写 *
```
s = 'chen wuang4fhsa￥fh。f'
#### title  非字母隔开的每个单词的首字母大写 *
s4 = s.title()
print(s4)
 ```
```
Chen Wuang4Fhsa￥Fh。F
```
###15.字符串是不可变变量，不支持直接通过下标修改
```
msg = 'abcdefg'
# msg[2] = 'z'

msg = msg[:2] + 'z' + msg[3:]

print(msg)
```
## 综合练习 龟兔赛跑
![image.png](https://upload-images.jianshu.io/upload_images/468490-db32339b34a0888f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 (模拟龟兔赛跑)本练习中要模拟龟兔赛跑的寓言故事。用随机数产生器建立模拟龟兔赛跑的程序。
 对手从70个方格的第1格开始起跑，每格表示跑道上的一个可能位置，终点线在第70格处。
 第一个到达终点的选手奖励一个新鲜萝卜和莴苣。兔子要在山坡上睡一觉，因此可能失去冠军。
    有一个每秒钟滴答一次的钟，程序应按下列规则调整动物的位置：

![image.png](https://upload-images.jianshu.io/upload_images/468490-845631e6c1731587.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

用变量跟踪动物的位置(即位置号1到70)。每个动物从位置1开始，如果动物跌到第1格以外，则移回第1格。
    产生随机整数1≤i≤10)，以得到上表中的百分比。对于乌龟，1≤i≤5时快走，6≤i≤7时跌跤，8≤i≤10时慢走，兔子也用相似的方法。
    起跑时，打印：
    BANG  !!!!!
    AND THEY' RE OFF  !!!!!
    时钟每次滴答一下(即每个重复循环)，打印第70格位置的一条线，显示乌龟的位置T和兔子的位置H。
 如果两者占用一格，则乌龟会咬兔子，程序从该位置开始打印 OUCH!!!。除T、H和OUCH!!!以外的其他打印位置都是空的。
    打印每一行之后，测试某个动物是否超过了第70格，如果是,则打印获胜者，停止模拟。
 如果乌龟赢，则打印TORTOISE WINS!!!YAY!!!。如果兔子赢，则打印Hare wins．Yush。
 如果两个动物同时赢，则可以同情弱者，让乌龟赢，或者打印It's a tie。如果两者都没有赢，则再次循环，模拟下一个时钟滴答。
 准备运行程序时，让一组拉拉队看比赛，你会发现观众有多么投入。

```

from random import randint
import time

print('begin')
hPos = 0
tPos = 0

while True:
    paodao = '_' * 70
    n = randint(0,10) + 1
    if n <= 5:
        tPos += 3
    elif 5 < n <= 7:
        tPos -= 6
    else:
        tPos += 1

    if n <= 2:
        hPos = hPos
    elif 3 <= n <= 4:
        hPos += 9
    elif n == 5:
        hPos -= 12
    elif 6 <= n <= 8:
        hPos += 1
    else:
        hPos -= 2

    if tPos >= 70 or hPos >= 70:
        break

    if tPos == hPos:
        paodao = paodao[:tPos] + '咬' + paodao[tPos:]
    else:
        paodao = paodao[:tPos] + '龟' + paodao[tPos:]
        paodao = paodao[:hPos] + '兔' + paodao[hPos:]

    print(paodao)
    time.sleep(0.5)

if tPos > 70:
    print('龟赢')
else:
    print('兔赢')

```
## 综合练习 猜单词游戏
![image](https://upload-images.jianshu.io/upload_images/468490-57c615cb4a569fea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/289/format/webp)

**任务目的**

1.掌握字符串常用操作
2.掌握随机数的用法
3.掌握控制台基本输入输出
4.掌握循环，分支条件的用法
5.培养编程思维，提高分析问题能力`

**任务描述**

![image](https://upload-images.jianshu.io/upload_images/468490-6cb9fda2be19a27e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/440/format/webp)

**需求**

给定单词数组（不少于10个），程序随机选择其中的一个，并显示单词字符长度个横线（-），用户有5次猜测机会，用户每次猜一个字母，如果正确，则将在相应的位置上显示出来；如错误则猜测机会减一，重复该过程，直至单词的全部字母全部猜出或者猜测次数用完，最后宣布用户胜利或失败。

**实例**

例如随机选出的单词是apple，程序先打印- - - - -
用户输入p，程序打印
*-pp--*
用户输入e，程序打印
*-pp-e*
用户输入t，程序打印
*-pp-e*
*您还有4次机会*
用户输入a，程序打印
*app-e*
用户输入l，程序打印
*apple*
*恭喜您，取得胜利。*

**任务注意事项**

请注意代码风格的整齐、优雅
代码中含有必要的注释
**代码**
```
import random
words = ['program','banana','tiger','policeman','interface']
index = random.randint(0,len(words) - 1)
word = words[index]
print(word)
wordbak = '-' * len(word)
print(wordbak)
guessTimes = 5
wordlst = list(wordbak)

while True:

    if guessTimes <= 0:
        break
    if '-' not in wordlst:
        break
    char = input('请输入一个字符：')
    if char in word:
        for i,c in enumerate(word):
            if c == char:
                wordlst[i] = char
    else:
        guessTimes -= 1
        print('你还剩下{}次机会'.format(guessTimes))

    print(''.join(wordlst))

if guessTimes > 0:
    print('you win')
else:
    print('you lose')
```
