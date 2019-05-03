### 什么是精灵？

当您玩任何2D游戏时，您在屏幕上看到的所有对象都是精灵。精灵可以是动画的，它们可以由玩家控制，甚至可以互相交互。

将在游戏循环的*UPDATE*和*DRAW*部分更新和绘制精灵。你可以想象，如果你的游戏中有大量的精灵，那么游戏循环的这些部分可能会变得非常冗长和复杂。幸运的是，Pygame有一个很好的解决方案：*精灵组*。

精灵组只是精灵的集合，您可以同时对所有精灵进行操作。让创建一个sprite组来保存游戏中的所有精灵：

```
 clock = pygame.time.Clock()
 all_sprites = pygame.sprite.Group()

```

现在可以通过在循环中添加以下内容来利用该组：

```
    # Update
    all_sprites.update()

    # Draw / render
    screen.fill(BLACK)
    all_sprites.draw(screen)

```

现在，对于创建的每个精灵，只需确保将它添加到`all_sprites`组中，它将自动在屏幕上绘制，并在每次循环时更新。

### 创建一个精灵

现在准备好制作第一个精灵了。在Pygame中，精灵是*对象*。
首先定义新精灵：

```
class Player(pygame.sprite.Sprite):

```

`class`告诉Python正在定义一个新对象，它将成为玩家精灵，它的父类型是`pygame.sprite.Sprite`，这意味着它将基于Pygame的预定义`Sprite`类。

在`class Player`定义中需要的第一个函数是特殊`__init__()`函数，它定义了在创建此类型的新对象时，需要初始化的一些属性。每个Pygame精灵都*必须*拥有两个属性： `image`和 `rect`：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((50, 50))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()

```

第一行`pygame.sprite.Sprite.__init__(self)`是Pygame所必需的 - 它运行内置`Sprite`类初始化程序。接下来，定义`image`属性 - 在这种情况下，只是创建一个简单的50 x 50正方形并用颜色填充它`GREEN`。稍后将学习如何让精灵`image`变得更加漂亮，比如角色或宇宙飞船，但现在一个稳固的方块已经足够好了。

接下来，必须定义精灵`rect`，它是“矩形”的缩写。在Pygame中使用矩形来跟踪对象的坐标。该`get_rect()`命令只是查看`image`并计算将它包围它的矩形。

可以使用`rect`它将精灵放在想要的任何地方在屏幕上。让从中心开始：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((50, 50))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)

```

现在已经定义了Player精灵，需要通过创建一个Player类的*实例*来“生成”（意思是创建）它。还需要确保将精灵添加到`all_sprites`组中：

```
all_sprites = pygame.sprite.Group()
player = Player()
all_sprites.add(player)

```

现在，如果你运行你的程序，你会看到屏幕中央的绿色方块。来吧，增加`WIDTH`和`HEIGHT`设置，这样你将有足够的窗口空间，使得精灵在接下来的步骤中移动。

![image](http://upload-images.jianshu.io/upload_images/468490-f260f36ff0648184.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 精灵运动

请记住，在游戏循环中，有`all_sprites.update()`。这意味着对于组中的每个sprite，Pygame将查找一个`update()`函数并运行它。所以为了让精灵移动，只需要定义它的更新规则（定义update函数）：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((50, 50))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)

    def update(self):
        self.rect.x += 5

```

这意味着每次循环时，都会将精灵的x坐标增加5个像素。继续运行它，你会看到精灵向屏幕右侧移动，但是会移出窗口：

![image](http://upload-images.jianshu.io/upload_images/468490-bcae437706b7d836.gif?imageMogr2/auto-orient/strip)

让通过使精灵环绕来解决这个问题 - 只要它到达屏幕的右侧，就会将它移到左侧。看一下矩形rect的结构：

![image](http://upload-images.jianshu.io/upload_images/468490-54067995bd9087db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因此，如果`rect`屏幕的左边缘离开屏幕，会将右边缘设置为0：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((50, 50))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)

    def update(self):
        self.rect.x += 5
        if self.rect.left > WIDTH:
            self.rect.right = 0

```

现在可以看到精灵会一直在屏幕上滚动：

![image](http://upload-images.jianshu.io/upload_images/468490-25be197bc43403f1.gif?imageMogr2/auto-orient/strip)

### 整合在一起
```
# Pygame sprite Example
import pygame
import random

WIDTH = 800
HEIGHT = 600
FPS = 30

# define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

class Player(pygame.sprite.Sprite):
    # sprite for the Player
    def __init__(self):
        # this line is required to properly create the sprite
        pygame.sprite.Sprite.__init__(self)
        # create a plain rectangle for the sprite image
        self.image = pygame.Surface((50, 50))
        self.image.fill(GREEN)
        # find the rectangle that encloses the image
        self.rect = self.image.get_rect()
        # center the sprite on the screen
        self.rect.center = (WIDTH / 2, HEIGHT / 2)

    def update(self):
        # any code here will happen every time the game loop updates
        self.rect.x += 5
        if self.rect.left > WIDTH:
            self.rect.right = 0

# initialize pygame and create window
pygame.init()
pygame.mixer.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Sprite Example")
clock = pygame.time.Clock()

all_sprites = pygame.sprite.Group()
player = Player()
all_sprites.add(player)
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
    all_sprites.update()

    # Draw / render
    screen.fill(BLACK)
    all_sprites.draw(screen)
    # *after* drawing everything, flip the display
    pygame.display.flip()

pygame.quit()
```


