## 模块的概念：

一个.py文件就称为一个模块

## 导入模块中类或函数的方式：
![](https://upload-images.jianshu.io/upload_images/468490-8d043258743ebe04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

module1.py
```
def output():
    print('hello neuedu')
```
###方式一：import  模块名

使用时：模块名.函数名()

```
import module1
module1.output()
```
###方式二 :from 模块名 import  函数名

使用时：函数名()
```
from module1 import output
output()
```

###方式三: from 模块名 import *

使用时：函数名()
```
from module1 import *
output()
```

###方式四:from 模块名 import 函数名 as  tt(自定义)     
注意原来的函数名将失效

使用时：tt()
```
from module1 import output as tt
tt()
```
>可以在模块当中定义一个变量__all__，指定导出的函数子集：

使用__all__的影响:  后面的[]里面写什么函数名，使用from 模块名 import *方式导入时导入什么  __all__如果没有这个变量将全部导入(__all__仅限 于from 模块名 import *这种导入方式)

加__all__示例：

```
__all__=['output']
def output():
    print('hello neuedu')

def output2():
        print('hello output2')
```
以下代码报错
```
from module1 import *
output2()
```

## 包的概念

　　概念：包就是一个文件夹，里面包含了若干py文件以及一个\__init\__.py文件。

![](https://upload-images.jianshu.io/upload_images/468490-33c43b08fe3d0022.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

module3.py
```
def output():
    print('hello neuedu')

def output2():
        print('hello output2')
```
## 导入包中函数，以及模块的方式：

### 方式一：from 包名  import  模块名

使用时：模块名.函数名()
```
from neuedu import module3
module3.output()
```
### 方式二：from 包名.模块名  import 函数名
使用时：函数名()
```
from neuedu.module3 import output
output()
```
### 方式三 ：import  包名.模块名      
使用的时候   包名.模块名.函数名()
```
import neuedu.module3
neuedu.module3.output()
```
### 方式四：from  包名  import  *    
前提是：将 __init__.py 文件中写入__all__变量(写入方式同模块导入的写入方式) 。 变量当中写入哪个模块则导入哪个模块，不写则什么都不导入
使用时：模块名.函数名()
\__init\__.py
```
__all__ = ['module3']
```

```
from neuedu import *
module3.output()
```

### 方式五：import 包名    

前提是：在包里面的__init__.py   文件里写入    from \. import  模块名   __init__.py里面导入哪个模块     通过本方式就能使用哪个模块

使用时：包名.模块名.函数名()
\__init\__.py
```
from . import module3
```
```
import neuedu
neuedu.module3.output()
```


