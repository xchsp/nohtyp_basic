
## 01. 算数运算符

* 是完成基本的算术运算使用的符号，用来处理四则运算

| 运算符| 描述 | 实例 |
| :---: | :---: | --- |
| + | 加 | 10 + 20 = 30 |
| - | 减 | 10 - 20 = -10 |
| * | 乘 | 10 * 20 = 200 |
| / | 除 | 10 / 20 = 0.5 |
| // | 取整除 | 返回除法的整数部分（商） 9 // 2 输出结果 4 |
| % | 取余数 | 返回除法的余数 9 % 2 = 1 |
| ** | 幂 | 又称次方、乘方，2 ** 3 = 8 |

```
>>> a=10
>>> b=5
>>> print(a+b)
15
>>> print(a-b)
5
>>> print(a*b)
50
>>> print(a/b)
2.0
>>> print(a**b)
100000
>>> print(a//b)
2
>>> print(a%b)
0
```

## 02. 比较（关系）运算符

| 运算符 | 描述 |
| --- | --- |
| == | 检查两个操作数的值是否 **相等**，如果是，则条件成立，返回 True |
| != | 检查两个操作数的值是否 **不相等**，如果是，则条件成立，返回 True |
| > | 检查左操作数的值是否 **大于** 右操作数的值，如果是，则条件成立，返回 True |
| < | 检查左操作数的值是否 **小于** 右操作数的值，如果是，则条件成立，返回 True |
| >= | 检查左操作数的值是否 **大于或等于** 右操作数的值，如果是，则条件成立，返回 True |
| <= | 检查左操作数的值是否 **小于或等于** 右操作数的值，如果是，则条件成立，返回 True |

```
>>> a=10
>>> b=20
>>> a==b
False
>>> a!=b
True
>>> a>b
False
>>> a<b
True
>>> a>=b
False
>>> a<=b
True
```

## 03. 逻辑运算符

| 运算符 | 逻辑表达式 | 描述 |
| --- | --- | --- |
| and | x and y | 只有 x 和 y 的值都为 True，才会返回 True<br />否则只要 x 或者 y 有一个值为 False，就返回 False |
| or | x or y | 只要 x 或者 y 有一个值为 True，就返回 True<br />只有 x 和 y 的值都为 False，才会返回 False |
| not | not x | 如果 x 为 True，返回 False<br />如果 x 为 False，返回 True |
```
>>> a=True
>>> b=False
>>> a and b
False
>>> a or b
True
>>> not a
False
>>> not -1
False
>>> not 0
True
```
## 04. 赋值运算符

* 在 Python 中，使用 `=` 可以给变量赋值
* 在算术运算时，为了简化代码的编写，`Python` 还提供了一系列的 与 **算术运算符** 对应的 **赋值运算符**
* 注意：**赋值运算符中间不能使用空格**

| 运算符 | 描述 | 实例 |
| --- | --- | --- |
| = | 简单的赋值运算符 | c = a + b 将 a + b 的运算结果赋值为 c |
| += | 加法赋值运算符 | c += a 等效于 c = c + a |
| -= | 减法赋值运算符 | c -= a 等效于 c = c - a |
| *= | 乘法赋值运算符	 | c *= a 等效于 c = c * a |
| /= | 除法赋值运算符 | c /= a 等效于 c = c / a |
| //= | 取整除赋值运算符 | c //= a 等效于 c = c // a |
| %= | 取 **模** (余数)赋值运算符 | c %= a 等效于 c = c % a |
| **= | 幂赋值运算符 | c **= a 等效于 c = c ** a |
```
>>> a=10
>>> b=20
>>> c=0
>>> c=a+b
>>> print(c)
30
>>> c+=10
>>> print(c)
40
>>> c-=a
>>> print(c)
30
>>> c*=a
>>> print(c)
300
>>> c/=a
>>> print(c)
30.0
>>> c%=a
>>> print(c)
0.0
>>> c=a**5
>>> print(c)
100000
>>> c//=b
>>> print(c)
5000
>>> print(b)
20
```
## 05. 运算符的优先级

* 以下表格的算数优先级由高到最低顺序排列

| 运算符 | 描述 |
| --- | --- |
| ** | 幂 (最高优先级) |
| * / % // | 乘、除、取余数、取整除 |
| + - | 加法、减法 |
| <= < > >= | 比较运算符 |
| == != | 等于运算符 |
| = %= /= //= -= += *= **= | 赋值运算符 |
| not or and | 逻辑运算符 |

