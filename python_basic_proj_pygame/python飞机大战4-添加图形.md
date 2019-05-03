# 游戏用到的图片资源

[![](http://upload-images.jianshu.io/upload_images/468490-c3d847dd910cbcc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://kidscancode.org/blog/img/playerShip1_orange.png) [![](http://upload-images.jianshu.io/upload_images/468490-5f466d6a769fdb3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://kidscancode.org/blog/img/meteorBrown_med1.png) [![](http://upload-images.jianshu.io/upload_images/468490-fcccc65063c16132.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://kidscancode.org/blog/img/laserRed16.png) [![](http://upload-images.jianshu.io/upload_images/468490-5242cc26945e4f22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://kidscancode.org/blog/img/starfield.png) 

将这些图像放到“img”文件夹。

## 加载图片

```
from os import path

img_dir = path.join(path.dirname(__file__), 'img')
```

## 绘制背景

从加载背景图像开始。在现有的游戏循环和初始化代码之前完成所有资源加载：

```
# Load all game graphics
background = pygame.image.load(path.join(img_dir, 'starfield.png')).convert()
background_rect = background.get_rect()
```

*在*绘制任何精灵*之前，在*游戏循环的绘图部分绘制我们的背景：

```
# Draw / render
screen.fill(BLACK)
screen.blit(background, background_rect)
all_sprites.draw(screen)
```

`blit`将一个图像的像素绘制到另一个图像的像素上 --- 背景图片绘制到屏幕上。现在我们的背景看起来更好了：

![image](http://upload-images.jianshu.io/upload_images/468490-439fb3c095ecbe3c.gif?imageMogr2/auto-orient/strip)

## 精灵图片

为我们的sprite加载图像：

```
# Load all game graphics
background = pygame.image.load(path.join(img_dir, 'starfield.png')).convert()
background_rect = background.get_rect()
player_img = pygame.image.load(path.join(img_dir, "playerShip1_orange.png")).convert()
meteor_img = pygame.image.load(path.join(img_dir, "meteorBrown_med1.png")).convert()
bullet_img = pygame.image.load(path.join(img_dir, "laserRed16.png")).convert()
```

从Player开始 - 我们想要替换那个绿色矩形，所以我们改变了`self.image`，不要忘记删除`image.fill(GREEN)`：

```
class Player(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = player_img
        self.rect = self.image.get_rect()
```

![](http://upload-images.jianshu.io/upload_images/468490-fc2ec32c429be347.gif?imageMogr2/auto-orient/strip)

但是，现在我们看到了一些问题。首先，图像比我们想象的要大很多。我们有两个选择：1）在图形编辑器（Photoshop，GIMP等）中打开图像并调整其大小; 或者2）利用代码调整图像大小。我们将选择选项2，使用Pygame的`transform.scale()`命令使图像大小缩小到50x30像素。

另一个问题是我们的船周围有一个黑色矩形，因为我们没有设置透明颜色`set_colorkey`：

```
self.image = pygame.transform.scale(player_img, (50, 38))
self.image.set_colorkey(BLACK)
```

`Bullet`和`Mob` 类似的做法，最终：

![image](http://upload-images.jianshu.io/upload_images/468490-1663db4afc6250c4.gif?imageMogr2/auto-orient/strip)

## 整合在一起
```
# Shmup game - part 4
# Video link: https://www.youtube.com/watch?v=mOckdKp3V38
# Adding graphics
import pygame
import random
from os import path

img_dir = path.join(path.dirname(__file__), 'img')

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
        self.image = pygame.transform.scale(player_img, (50, 38))
        self.image.set_colorkey(BLACK)
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

    def shoot(self):
        bullet = Bullet(self.rect.centerx, self.rect.top)
        all_sprites.add(bullet)
        bullets.add(bullet)

class Mob(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = meteor_img
        self.image.set_colorkey(BLACK)
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(WIDTH - self.rect.width)
        self.rect.y = random.randrange(-100, -40)
        self.speedy = random.randrange(1, 8)
        self.speedx = random.randrange(-3, 3)

    def update(self):
        self.rect.x += self.speedx
        self.rect.y += self.speedy
        if self.rect.top > HEIGHT + 10 or self.rect.left < -25 or self.rect.right > WIDTH + 20:
            self.rect.x = random.randrange(WIDTH - self.rect.width)
            self.rect.y = random.randrange(-100, -40)
            self.speedy = random.randrange(1, 8)

class Bullet(pygame.sprite.Sprite):
    def __init__(self, x, y):
        pygame.sprite.Sprite.__init__(self)
        self.image = bullet_img
        self.image.set_colorkey(BLACK)
        self.rect = self.image.get_rect()
        self.rect.bottom = y
        self.rect.centerx = x
        self.speedy = -10

    def update(self):
        self.rect.y += self.speedy
        # kill if it moves off the top of the screen
        if self.rect.bottom < 0:
            self.kill()

# Load all game graphics
background = pygame.image.load(path.join(img_dir, "starfield.png")).convert()
background_rect = background.get_rect()
player_img = pygame.image.load(path.join(img_dir, "playerShip1_orange.png")).convert()
meteor_img = pygame.image.load(path.join(img_dir, "meteorBrown_med1.png")).convert()
bullet_img = pygame.image.load(path.join(img_dir, "laserRed16.png")).convert()

all_sprites = pygame.sprite.Group()
mobs = pygame.sprite.Group()
bullets = pygame.sprite.Group()
player = Player()
all_sprites.add(player)
for i in range(8):
    m = Mob()
    all_sprites.add(m)
    mobs.add(m)

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
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                player.shoot()

    # Update
    all_sprites.update()

    # check to see if a bullet hit a mob
    hits = pygame.sprite.groupcollide(mobs, bullets, True, True)
    for hit in hits:
        m = Mob()
        all_sprites.add(m)
        mobs.add(m)

    # check to see if a mob hit the player
    hits = pygame.sprite.spritecollide(player, mobs, False)
    if hits:
        running = False

    # Draw / render
    screen.fill(BLACK)
    screen.blit(background, background_rect)
    all_sprites.draw(screen)
    # *after* drawing everything, flip the display
    pygame.display.flip()

pygame.quit()
```

