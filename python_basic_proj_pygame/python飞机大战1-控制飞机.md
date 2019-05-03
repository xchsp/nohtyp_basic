## 入门


在这个系列中，将制作一个星际争霸游戏。

`shmup.py`

首先，让将游戏设置修改一下：

```
WIDTH = 480
HEIGHT = 600
FPS = 60
```

## 玩家精灵

要添加的第一件事是代表玩家的精灵。最终，这将是一艘宇宙飞船。但是当你第一次开始时，忽略图形会更简单，只需对所有精灵使用普通矩形。

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((50, 40))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.centerx = WIDTH / 2
        self.rect.bottom = HEIGHT - 10
        self.speedx = 0
```

为Player选择了50x40像素的大小，将其定位矩形`rect`放在屏幕的底部中心。还定义了一个`speedx`属性，可以跟踪玩家在x方向上移动的速度（从一侧到另一侧）。

对于`update()`方法，这是将在游戏的每一次循环调用的函数，x坐标向右移动speedx个像素：

```
    def update(self):
        self.rect.x += self.speedx
```

创建精灵，加入精灵组：

```
all_sprites = pygame.sprite.Group()
player = Player()
all_sprites.add(player)
```


## 运动/控制

这是一个键盘控制的游戏，所以希望玩家在按下`Left`或`Right`箭头键时移动精灵。


```
def update(self):
        self.speedx = 0
        keystate = pygame.key.get_pressed()
        if keystate[pygame.K_LEFT]:
            self.speedx = -8
        if keystate[pygame.K_RIGHT]:
            self.speedx = 8
        self.rect.x += self.speedx
```

上面代码会将`speedx`每帧设置为0，然后检查键是否已被按下。在`pygame.key.get_pressed()`返回一个字典keystate，如果某个键被按下，那么字典中该键对应的值是True。

## 边界检查

最后，需要确保精灵不会离开屏幕。将向Player类的`update()`方法添加以下内容：

```
        if self.rect.right > WIDTH:
            self.rect.right = WIDTH
        if self.rect.left < 0:
            self.rect.left = 0
```

现在，如果Player试图移动到超过屏幕左边缘或右边缘的位置，它将停止。

## 整合在一起

以下是此步骤的完整代码：

```
# Shmup game - part 1
# Video link: https://www.youtube.com/watch?v=nGufy7weyGY
# Player sprite and movement
import pygame
import random

WIDTH = 480
HEIGHT = 600
FPS = 60

# define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
YELLOW = (255, 255, 0)

# initialize pygame and create window
pygame.init()
pygame.mixer.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Shmup!")
clock = pygame.time.Clock()

class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((50, 40))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.centerx = WIDTH / 2
        self.rect.bottom = HEIGHT - 10
        self.speedx = 0

    def update(self):
        self.speedx = 0
        keystate = pygame.key.get_pressed()
        if keystate[pygame.K_LEFT]:
            self.speedx = -8
        if keystate[pygame.K_RIGHT]:
            self.speedx = 8
        self.rect.x += self.speedx
        if self.rect.right > WIDTH:
            self.rect.right = WIDTH
        if self.rect.left < 0:
            self.rect.left = 0

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

