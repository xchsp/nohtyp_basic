# 碰撞发生了什么？


![](http://upload-images.jianshu.io/upload_images/468490-64db0337961ffd09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Pygame中的默认碰撞类型是使用`collide_rect()`函数，该函数使用两个精灵的`rect`属性来计算它们是否重叠。这称为AABB碰撞，它非常快速和可靠。但是，如果精灵图像不是矩形，那么就会出现如图所示的情况。矩形重叠，那么`collide_rect()`就是`True`，但是玩家会感到沮丧，因为他们觉得自己应该已经成功地躲过流星（还差点才碰到一起）。

如果发现处于这种情况，可以尝试以下几种方法：

![image](http://upload-images.jianshu.io/upload_images/468490-2278370e497b91e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过使用`collide_rect_ratio()`函数，您可以使用较小的矩形（上图矩形的0.7倍），减少可以计为重叠的“空”空间量。根据精灵的形状，这可以很好地工作。这意味着在某些情况下，流星似乎会通过机翼而不会被视为撞击。这实际上是一个很好的情况！当游戏中的事物发生变化时，玩家不会直接注意到这一点，只会觉得他们“躲开”了一个非常接近的闪避。他们不会感到沮丧，而是会觉得自己做得很好。

![](http://upload-images.jianshu.io/upload_images/468490-676dee6f0efd3463.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另一种选择是使用圆形边界框。在流星的情况下，这是非常合适的。它不太适合飞船---在碰到飞船之外的机翼也不会算数。

## 设置精灵的半径

根据上面的选项，我们将为流星与玩家碰撞设置碰撞圈。Pygame让这很容易实现 - 我们只需要在每个sprite上设置一个新属性：`self.radius`。

让我们从玩家开始。碰撞圈应该有多大？可以通过一些实验来获得正确的值。以下是如何在玩家精灵中执行此操作`__init()__`：

```
	self.rect = self.image.get_rect()
	self.radius = 25
	pygame.draw.circle(self.image, RED, self.rect.center, self.radius)
```

我们在玩家图像上绘制一个红色圆圈，以便我们可以看到它的外观。让我们为流星做同样的事情：

```
	self.rect = self.image.get_rect()
	self.radius = int(self.rect.width / 2)
	pygame.draw.circle(self.image, RED, self.rect.center, self.radius)
```

以下是我们最终的结果：

![image](http://upload-images.jianshu.io/upload_images/468490-c8efe72bd7d1a646.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你可以看到我们玩家精灵的半径可能太大了 - 它在y轴上实际上比船的大小更大。为了更接近，让我们设置`self.radius = 20`。

对于流星，我们想要一些角落伸出来，所以让我们将圆圈缩放到85％的大小：

```
	self.radius = int(self.rect.width * .85 / 2)
```

## 更改碰撞类型

为了让游戏开始后使用这些圆圈进行碰撞测试，我们只需要更改`spritecollide`命令以使用圆形函数而不是AABB：

```
	# check to see if a mob hit the player
    hits = pygame.sprite.spritecollide(player, mobs, False, pygame.sprite.collide_circle)
    if hits:
        running = False
```

![](http://upload-images.jianshu.io/upload_images/468490-2f7ed9912088e99d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一旦你尝试了它并且你对碰撞的工作原理感到满意，你可以删除红色圆圈。建议只是将命令注释掉而不是删除它们，以防以后再次使用它们。

##  整合到一起

确定正确的碰撞风格可以对游戏的感觉产生重大影响。我们现在有一个更好的流星与玩家碰撞，但请注意我们没有改变子弹与流星碰撞的风格。圆圈对于子弹的形状来说是一个糟糕的选择，所以最好将它们留作矩形。
```
# Shmup game - part 5
# Video link: https://www.youtube.com/watch?v=_y5U8tB36Vk
# Improved collisions
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
        self.image = meteor_img
        self.image.set_colorkey(BLACK)
        self.rect = self.image.get_rect()
        self.radius = int(self.rect.width * .85 / 2)
        # pygame.draw.circle(self.image, RED, self.rect.center, self.radius)
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
在下一部分中，我们将通过学习如何为我们的精灵添加动画来实现目标。
