### 转向图形精灵

彩色矩形很好 - 它们是一个好的开始，并确保你的游戏基本工作，但迟早你会想要为你的精灵使用一个很酷的宇宙飞船图像或角色。这引出了第一个问题：在哪里获得游戏资源。

### 获得图片资源

当你需要为你的游戏添加图片资源时，你有3个选择：

1.  自己制作
2.  找一位美工为你制作
3.  使用互联网上已有的图片资源

在本课中，将使用图像“p1_jump.png”：

[![](http://upload-images.jianshu.io/upload_images/468490-48f18aec2809497a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://kidscancode.org/blog/img/p1_jump.png) 

### 管理游戏资源

首先，需要一个文件夹img来保存游戏资源，然后将图像放入其中。

要在游戏中使用此图像，需要让Pygame加载图片文件，这意味着需要程序知道文件的位置。根据使用的计算机类型，这可能会有所不同，希望能够在任何计算机上运行程序，因此需要导入一个名为`os`的Python库。

```
 import pygame
 import random
 import os

 # set up asset folders
 game_folder = os.path.dirname(__file__)

```

特殊的Python变量`__file__`指的是当前代码文件所在的文件夹，函数os.path.dirname会获得该文件夹的*路径*。例如

`/Users/chris/Documents/gamedev/tutorials/1-3 sprite example.py`

如果使用的是Windows，路径可能如下所示：

`C:\Users\chris\Documents\python\game.py`

不同的操作系统使用不同的方式来描述计算机上的位置。通过使用`os.path`命令，可以让计算机找出正确的路径。

```
 import pygame
 import random
 import os

 # set up asset folders
 game_folder = os.path.dirname(__file__)
 img_folder = os.path.join(game_folder, 'img')
 player_img = pygame.image.load(os.path.join(img_folder, 'p1_jump.png')).convert()

```

现在已经加载了图像，`pygame.image.load()`并且已经确保使用`convert()`，这将通过将图像转换为在屏幕上绘制更快的格式来加速Pygame的绘制。现在准备用精美的玩家图片形象替换精灵中的普通绿色方块：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = player_img
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)

```

请注意，已经删除了命令`self.image.fill(GREEN)`- 不再需要填充纯色。

现在，如果你运行该程序，你应该看到一个漂亮的小卡通外星人在屏幕上运行。但是遇到了一个问题： 
目前无法看到，因为背景目前是黑色的。
用` screen.fill(BLUE)`将背景改为蓝色。现在可以看到问题：

![image](http://upload-images.jianshu.io/upload_images/468490-c1c2ecff8d822174.gif?imageMogr2/auto-orient/strip)

当您在计算机上有图像文件时，该文件始终是矩形像素网格。无论你绘制什么样的形状，仍然会有像素边框填充图像的“背景”。需要做的是告诉Pygame忽略不关心的图像中的像素。在此图像中，这些像素恰好是黑色，因此可以添加以下内容：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = player_img
	self.image.set_colorkey(BLACK)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)

```

`set_colorkey()`只是告诉Pygame，当绘制图像时，要忽略指定颜色的任何像素。现在图像看起来好多了：

![image](http://upload-images.jianshu.io/upload_images/468490-e44e24665f60e31f.gif?imageMogr2/auto-orient/strip)

### 整合在一起
```
# Pygame sprite Example
import pygame
import random
import os

WIDTH = 800
HEIGHT = 600
FPS = 30

# define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# set up assets folders
# Windows: "C:\Users\chris\Documents\img"
# Mac: "/Users/chris/Documents/img"
game_folder = os.path.dirname(__file__)
img_folder = os.path.join(game_folder, "img")

class Player(pygame.sprite.Sprite):
    # sprite for the Player
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(os.path.join(img_folder, "p1_jump.png")).convert()
        self.image.set_colorkey(BLACK)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)
        self.y_speed = 5

    def update(self):
        self.rect.x += 5
        self.rect.y += self.y_speed
        if self.rect.bottom > HEIGHT - 200:
            self.y_speed = -5
        if self.rect.top < 200:
            self.y_speed = 5
        if self.rect.left > WIDTH:
            self.rect.right = 0

# initialize pygame and create window
pygame.init()
pygame.mixer.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("My Game")
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
    screen.fill(BLUE)
    all_sprites.draw(screen)
    # *after* drawing everything, flip the display
    pygame.display.flip()

pygame.quit()
```
