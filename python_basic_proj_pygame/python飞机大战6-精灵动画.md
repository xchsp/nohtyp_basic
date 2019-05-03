# 动画流星

我们所有的流星都看起来完全一样，看起来不是好：

![image](http://upload-images.jianshu.io/upload_images/468490-4e28f9fd8ce69415.gif?imageMogr2/auto-orient/strip)

我们怎样才能为流星添加更多变化和视觉吸引力呢？一种方法是增加一点旋转，使它们看起来更像是在空间中翻滚的岩石。旋转相对容易 - 就像我们使用`pygame.transform.scale()`函数来改变Player精灵的大小一样，我们可以用`pygame.transform.rotate()`来执行旋转。但是，为了使其正常工作，我们需要学习一些东西。

首先，让我们为`Mob`精灵添加一些新属性：

```
class Mob(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = meteor_img
		self.image.set_colorkey(BLACK)
        self.rect = self.image.get_rect()
        self.radius = int(self.rect.width * .85 / 2)
        self.rect.x = random.randrange(WIDTH - self.rect.width)
        self.rect.y = random.randrange(-150, -100)
        self.speedy = random.randrange(1, 8)
        self.speedx = random.randrange(-3, 3)
        self.rot = 0
        self.rot_speed = random.randrange(-8, 8)
        self.last_update = pygame.time.get_ticks()
```

第一个属性`rot`（'旋转'rotate的缩写）将测量精灵旋转的度数。它从0开始，随着时间的推移而变化。 `rot_speed`测量精灵每次应旋转多少度 --- 更大的数字意味着更快的旋转。我们选择一个随机值，负值为逆时针，正值为顺时针。

最后一个属性是控制动画速度的重要属性。我们不想每帧都改变精灵的图像，否则它会显得太快。每当你为精灵的图像制作动画时，必须控制图像改变的频率。

我们有一个名为`pygame.time.Clock()`的对象`clock`，它帮助我们控制FPS。通过调用`pygame.time.get_ticks()`，我们可以看出自时钟启动以来已经过了多少毫秒。通过这种方式，可以控制图像改变的频率，具体办法如下：

### 旋转图像

更新`update()`方法，调用rotate()：

```
	def update(self):
		self.rotate()
```

rotate()的实现：

```
	def rotate(self):
		now = pygame.time.get_ticks()
		if now - self.last_update > 50:
			self.last_update = now
			# do rotation here
```

首先，我们检查当前的时间，然后我们减去上次更新的时间。如果超过50毫秒，我们将更新图像。我们把值`now`放入`last_update`，然后执行旋转的代码。现在，您可能会认为将旋转应用于精灵很简单：

```
self.image = pygame.transform.rotate(self.image, self.rot_speed)
```

但是，会遇到问题：

![image](http://upload-images.jianshu.io/upload_images/468490-31c9a48b9e606ccf.gif?imageMogr2/auto-orient/strip)

### 旋转是破坏性的！

发生这种情况是因为图像由像素网格组成。当您尝试将这些像素旋转到新位置时，其中一些像素将不再排列，因此某些信息将丢失。如果你只旋转一次就没问题，但是反复旋转图像会导致图像混乱。

解决方案是使用我们的`rot`变量来跟踪总旋转量（添加`rot_speed`每个更新）并将*原始*图像旋转至该量。这样我们总是从最初的图像开始，只旋转一次。

首先保留原始图像的副本：`self.image = self.image_orig.copy()`

```
class Mob(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image_orig = random.choice(meteor_images)
        self.image_orig.set_colorkey(BLACK)
        self.image = self.image_orig.copy()
        self.rect = self.image.get_rect()
        self.radius = int(self.rect.width * .85 / 2)
        self.rect.x = random.randrange(WIDTH - self.rect.width)
        self.rect.y = random.randrange(-150, -100)
        self.speedy = random.randrange(1, 8)
        self.speedx = random.randrange(-3, 3)
        self.rot = 0
        self.rot_speed = random.randrange(-8, 8)
        self.last_update = pygame.time.get_ticks()
```

然后在`rotate`方法中，更新旋转值`rot`，并将该旋转值应用于原始图像：

```
    def rotate(self):
        now = pygame.time.get_ticks()
        if now - self.last_update > 50:
            self.last_update = now
            self.rot = (self.rot + self.rot_speed) % 360
            self.image = pygame.transform.rotate(self.image_orig, self.rot)
```

请注意，我们使用余数运算符 - `%`来防止`rot`值大于360。

几乎完美了，但仍然有一个小问题：

![image](http://upload-images.jianshu.io/upload_images/468490-d96ea1d307d54ee6.gif?imageMogr2/auto-orient/strip)

流星看起来像是弹跳而不是顺畅地旋转。

### 更新rect

旋转图像后，图像的大小`rect`可能不再正确。看一个宇宙飞船图片旋转的例子：

![image](http://upload-images.jianshu.io/upload_images/468490-b9cb4924cc5d979a.gif?imageMogr2/auto-orient/strip)

在这里我们可以看到，当我们旋转图像时，矩形保持不变，这是不正确的。我们需要在每次图像更改时计算新的矩形：

![image](http://upload-images.jianshu.io/upload_images/468490-7901bcbff6faaf88.gif?imageMogr2/auto-orient/strip)

根据图像的旋转方式，很容易看出矩形的大小会有多大变化。现在，为了修复“弹跳”效果，我们需要确保将新的rect保持在与旧的矩形相同的位置，而不是固定在左上角：

![image](http://upload-images.jianshu.io/upload_images/468490-99f135cedbf21e70.gif?imageMogr2/auto-orient/strip)

回到我们的旋转代码，我们只记录rect中心的位置，计算新的rect，并将其中心更新：

```
	def rotate(self):
        now = pygame.time.get_ticks()
        if now - self.last_update > 50:
            self.last_update = now
            self.rot = (self.rot + self.rot_speed) % 360
            new_image = pygame.transform.rotate(self.image_orig, self.rot)
            old_center = self.rect.center
            self.image = new_image
            self.rect = self.image.get_rect()
            self.rect.center = old_center
```

## 随机流星图像

我们为使流星更有趣而做的最后一件事是使用不同尺寸的流星。

首先，我们将加载所有流星图像并将它们放入列表中：

```
meteor_images = []
meteor_list =['meteorBrown_big1.png','meteorBrown_med1.png',
              'meteorBrown_med1.png','meteorBrown_med3.png',
              'meteorBrown_small1.png','meteorBrown_small2.png',
              'meteorBrown_tiny1.png']
for img in meteor_list:
    meteor_images.append(pygame.image.load(path.join(img_dir, img)).convert())
```

然后，当我们的流星产生时，我们所要做的就是随机选择一个图像：

```
class Mob(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image_orig = random.choice(meteor_images)
        self.image_orig.set_colorkey(BLACK)
        self.image = self.image_orig.copy()
```

好多了！

![image](http://upload-images.jianshu.io/upload_images/468490-0343fe1a8012da86.gif?imageMogr2/auto-orient/strip)

## 整合到一起

```
# Shmup game - part 6
# Video link: https://www.youtube.com/watch?v=_y5U8tB36Vk
# Sprite animation - rotating meteors
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
        self.radius = 20
        # pygame.draw.circle(self.image, RED, self.rect.center, self.radius)
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
        self.image_orig = random.choice(meteor_images)
        self.image_orig.set_colorkey(BLACK)
        self.image = self.image_orig.copy()
        self.rect = self.image.get_rect()
        self.radius = int(self.rect.width * .85 / 2)
        # pygame.draw.circle(self.image, RED, self.rect.center, self.radius)
        self.rect.x = random.randrange(WIDTH - self.rect.width)
        self.rect.y = random.randrange(-150, -100)
        self.speedy = random.randrange(1, 8)
        self.speedx = random.randrange(-3, 3)
        self.rot = 0
        self.rot_speed = random.randrange(-8, 8)
        self.last_update = pygame.time.get_ticks()

    def rotate(self):
        now = pygame.time.get_ticks()
        if now - self.last_update > 50:
            self.last_update = now
            self.rot = (self.rot + self.rot_speed) % 360
            new_image = pygame.transform.rotate(self.image_orig, self.rot)
            old_center = self.rect.center
            self.image = new_image
            self.rect = self.image.get_rect()
            self.rect.center = old_center

    def update(self):
        self.rotate()
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
bullet_img = pygame.image.load(path.join(img_dir, "laserRed16.png")).convert()
meteor_images = []
meteor_list = ['meteorBrown_big1.png', 'meteorBrown_med1.png', 'meteorBrown_med1.png',
               'meteorBrown_med3.png', 'meteorBrown_small1.png', 'meteorBrown_small2.png',
               'meteorBrown_tiny1.png']
for img in meteor_list:
    meteor_images.append(pygame.image.load(path.join(img_dir, img)).convert())

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
    hits = pygame.sprite.spritecollide(player, mobs, False, pygame.sprite.collide_circle)
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

动画精灵为游戏增添了许多视觉吸引力，无论是旋转的岩石还是跑步/跳跃/蹲伏的英雄。但是，您拥有的动画越多，您需要跟踪的图像就越多。诀窍是让它们井井有条，并利用`pygame.transform`命令等工具- 只要你小心它们的局限性。

在下一部分中，我们将开始保持分数并深入了解如何在屏幕上绘制文本。
