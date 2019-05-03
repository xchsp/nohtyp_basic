## 什么是Pygame？

Pygame是一个“游戏开发库” - 一组帮助程序员制作游戏的工具。包含：

*   图形和动画
*   声音（包括音乐）
*   控制（键盘，鼠标，游戏手柄等）

### 游戏循环

每个游戏的核心都是一个循环，将其称为“游戏循环”。这个循环一直在不断运行，一遍又一遍地完成游戏工作所需的所有事情。每次循环显示一次游戏当前画面，称为*帧*。

游戏框架，主要处理3件事情

#### 1.程序输入（或发生的事件[鼠标点击或键盘按下]）

这意味着你想要关注游戏之外的其他东西，需要游戏回应的其他东西。这些可能是键盘上按下的键，点击鼠标等。

#### 2.更新游戏

如果角色在空中，重力需要将其拉下来。如果两个对象相互碰撞，则需要爆炸。

#### 3.渲染（或绘制）

此步骤中，在屏幕上绘制所有内容。必须在屏幕上的正确更新位置绘制背景，角色，菜单或玩家需要看到的任何其他内容。

### 时钟

循环的另一个重要方面是控制整个循环的运行速度。您可能听说过*FPS*这个术语，它代表每秒帧数。这意味着这个循环每秒应发生多少次。这很重要，因为您不希望游戏运行得太快或太慢。你也不希望它在不同的计算机上以不同的速度运行 。

## 构建Pygame游戏程序骨架

制作一个简单的pygame程序，除了打开一个窗口并运行游戏循环之外什么都不做。

在程序的顶部，将导入需要的库并为游戏设置一些变量：

```
# Pygame template - skeleton for a new pygame project
import pygame
import random

WIDTH = 360  # width of our game window
HEIGHT = 480 # height of our game window
FPS = 30 # frames per second
```

接下来需要打开游戏窗口：

```
# initialize pygame and create window
pygame.init()
pygame.mixer.init()  #声音初始化
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("My Game")
clock = pygame.time.Clock()
```

`pygame.init()`是启动pygame，并“初始化”它的命令。 `screen`指的是游戏屏幕，按照在配置常量中设置的大小创建它。最后，创建了一个，`clock`时钟以便能够确保游戏以想要的FPS运行。

让游戏循环：

```
# Game Loop
running = True
while running:
    # Process input (events)
    # Update
    # Render (draw)
```

这是游戏循环，它是`while`由变量控制的循环`running`。如果希望游戏结束，只需要设置`running`为`False`循环就会结束。现在可以用一些基本代码填写每个部分。

### 渲染/绘制部分

将从*Draw*部分开始。还没有任何角色，可以用纯色填充屏幕。
计算机如何处理颜色

电脑屏幕由像素组成，这些像素有3个部分：红色，绿色和蓝色。每个部分点亮多少会决定像素的颜色，如下所示：

![image](http://upload-images.jianshu.io/upload_images/468490-f0379f472cbc2f06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

三原色中的每一个可以具有介于0和255之间的值，因此对于三种基色中的每一种，存在256种不同的可能性。以下是可以制作的一些颜色示例：

![image](http://upload-images.jianshu.io/upload_images/468490-9c04638c13ffe1e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以通过乘法找到计算机可以显示的颜色总数：

```
    >>> 256 * 256 * 256 = 16,777,216
```

在程序的顶部定义一些颜色：

```
# Colors (R, G, B)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
```

让绘图部分填满屏幕。

```
    # Draw / render
    screen.fill(BLACK)
```

但是，由于计算机显示的工作方式，这还不够。更改屏幕上的像素意味着告诉视频卡告诉显示器更改实际像素。从计算机的角度来看，这是一个非常非常缓慢的过程。因此，如果你在屏幕上绘制了很多东西，那么绘制它们可能需要很长时间。可以通过使用称为*双缓冲*的东西，以巧妙的方式解决这个问题。

想象一下，有一个双面白板，可以翻转显示一侧或另一侧。前面是显示器（玩家看到的屏幕），而后面是隐藏的，只有计算机可以“看到”它。每一帧，都在背面绘制所有图画 - 每个角色，每个子弹，每个闪耀的光线等等。然后，当完成后，将棋盘翻转并显示。这意味着只是每帧执行*一次*与显示器交谈的过程。

所有这些都在pygame中自动发生。完成绘图后，您只需要告诉它翻转白板。命令为`flip()`：

```
    # Draw / render
    screen.fill(BLACK)
    # *after* drawing everything, flip the display
    pygame.display.flip()
```

只要确保你总是做到`flip()`放在 *最后*。
如果你试图在翻转后，再画一些东西，它将不会被看到！

### 输入/事件部分

如果现在尝试运行该程序，会发现遇到了问题：无法关闭窗口！单击角落中的“X”不起作用。那是因为那实际上是一个事件，需要告诉程序监听该事件并做出相应处理---退出游戏。

Pygame保存自上一帧以来发生的所有事件。可以通过以下代码检查发生了哪些事件

```
    for event in pygame.event.get():
        # check for closing window
        if event.type == pygame.QUIT:
            running = False
```

Pygame有很多事件。 `pygame.QUIT`是单击“X”时发生的事件。

### 控制FPS

还没有任何东西要放在“更新”部分，但仍然需要确保使用`FPS`设置来控制速度。可以这样做：

```
while running:
    # keep loop running at the right speed
    clock.tick(FPS)
```

该`tick()`命令告诉pygame一秒循环多少次。如果设置`FPS`为20，这意味着需要框架每个循环将持续<sup>1</sup> / <sub>20</sub>（0.05）秒。如果循环代码（更新，绘图等）只需要0.03秒，那么pygame将等待0.02秒。以上是计算机处理比较快的情况。如果电脑比较差，运行缓慢，一秒钟未必能执行20次循环--- clock.tick(20)是一个指导意见。

### 整合到一起

最后，让确保当游戏循环结束时，实际上会破坏游戏窗口。通过放在`pygame.quit()`代码的最后来实现这一点。所以最终代码如下所示：

```
# Pygame template - skeleton for a new pygame project
import pygame
import random

WIDTH = 360
HEIGHT = 480
FPS = 30

# define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# initialize pygame and create window
pygame.init()
pygame.mixer.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("My Game")
clock = pygame.time.Clock()

# Game loop
running = True
while running:
    # keep loop running at the right speed
    clock.tick(FPS)
    # Process input (events)
    for event in pygame.event.get():
        # check for closing window
        if event.type == pygame.QUIT:
            running = False

    # Update

    # Draw / render
    screen.fill(BLACK)
    # *after* drawing everything, flip the display
    pygame.display.flip()

pygame.quit()
```

