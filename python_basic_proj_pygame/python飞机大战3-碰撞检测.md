# 碰撞

碰撞是游戏开发的基本部分。 *碰撞检测*意味着您要检测游戏世界中的一个对象是否正在触碰另一个对象。 *碰撞反应*决定了碰撞发生时你想要做什么 - 马里奥拿起硬币，子弹伤害敌人等等。


## 边框

请记住，Pygame中的每个sprite都有一个`rect`属性定义其坐标及其大小。`rect`在Pygame的对象格式为`[x, y, width, height]`，其中`x`和`y`表示矩形的左上角。

为了检测碰撞，我们需要查看`rect`玩家的角色并将其与`rect`每个怪物进行比较。现在我们可以通过循环遍历怪物并为执行此比较的每个人执行此操作：

![image](http://upload-images.jianshu.io/upload_images/468490-accfe3a5619404ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在此图片中，您可以看到只有矩形＃3与大黑色矩形相撞。＃1在x轴上重叠，但不在y; ＃2在y中重叠，但在x中不重叠。为了使两个矩形重叠，它们的*边界*必须在*每个*轴上重叠。在代码中写这个：

```
if mob.rect.right > player.rect.left and \
   mob.rect.left < player.rect.right and \
   mob.rect.bottom > player.rect.top and \
   mob.rect.top < player.rect.bottom:
       collide = True
```

幸运的是，Pygame通过使用一个内置函数`spritecollide()`来完成上述操作。

# 与玩家碰撞的怪物

我们将把这个命令添加到游戏循环的“更新”部分：

```
#Update
all_sprites.update()

#check to see if a mob hit the player
hits = pygame.sprite.spritecollide(player, mobs, False)
if hits:
    running = False
```

该`spritecollide()`函数有3个参数：要检查的玩家精灵，要比较的*敌人组*的名称，以及`dokill`参数（True / False）。`dokill`参数设置是否应该在命中对象时删除该对象。例如，如果我们试图看看玩家是否拿起了硬币，我们就想把它设置`True`，硬币就会消失。

`spritecollide()`命令的返回值是被击中的精灵列表（记住，玩家可能一次与多个暴徒相撞）。我们将该列表分配给变量`hits`。

如果`hits`列表不为空，设置`running`为`False`，使得游戏结束。

# 回击

## 子弹精灵

添加一个新精灵：子弹。这将是我们按键时产生的精灵，出现在玩家精灵的顶部，并以相当高的速度向上移动。
`Bullet`类：

```
class Bullet(pygame.sprite.Sprite):
    def __init__(self, x, y):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((10, 20))
        self.image.fill(YELLOW)
        self.rect = self.image.get_rect()
        self.rect.bottom = y
        self.rect.centerx = x
        self.speedy = -10

    def update(self):
        self.rect.y += self.speedy
        # kill if it moves off the top of the screen
        if self.rect.bottom < 0:
            self.kill()
```

在`__init__()`子弹精灵的方法中，我们传递`x`和`y`值，以便我们可以告诉精灵在哪里出现。由于玩家精灵可以移动，设置为玩家射击时的位置。我们设置`speedy`为负值，以便它会向上移动。

最后，我们检查子弹是否已经脱离屏幕顶部，如果是，我们将其删除。

## Keypress事件

为了让事情变得简单，我们要做到这一点，每次玩家按下空格键时，都会发射子弹。我们需要将其添加到事件检查中：

```
for event in pygame.event.get():
    # check for closing window
    if event.type == pygame.QUIT:
        running = False
    elif event.type == pygame.KEYDOWN:
        if event.key == pygame.K_SPACE:
            player.shoot()
```

我们的新代码检查`KEYDOWN`事件，如果有事件，则检查它是否是`K_SPACE`键。如果是的话，我们将运行玩家精灵的`shoot()`方法。

## 大量子弹

首先，我们需要添加一个新精灵组来保存所有子弹对象：

```
bullets = pygame.sprite.Group()
```

现在，我们可以在`Player`类中添加以下方法：

```
def shoot(self):
    bullet = Bullet(self.rect.centerx, self.rect.top)
    all_sprites.add(bullet)
    bullets.add(bullet)
```

`shoot()`方法会产生一颗子弹，使用玩家的顶部中心作为生成点。然后将子弹添加到`all_sprites精灵组`（绘制和更新）。以及`bullets精灵组`（碰撞检查）。

#### 子弹碰撞

现在我们需要检查子弹是否击中怪物。这里的区别是我们有多个子弹（在`bullets`组中）和多个小怪（在`mobs`组中），所以我们不能用`spritecollide()`，因为它只比较*一个*精灵与一个组。将使用`groupcollide()`：

```
# Update
all_sprites.update()

# check to see if a bullet hit a mob
hits = pygame.sprite.groupcollide(mobs, bullets, True, True)
for hit in hits:
    m = Mob()
    all_sprites.add(m)
    mobs.add(m)
```

该`groupcollide()`功能类似于`spritecollide()`，除了命名两个组进行比较，您将获得的是被击中的怪物列表。有两个`dokill`选项，每个选项对应一个组。

如果我们只是删除小怪，我们就会遇到一个问题：怪物用完了！所以我们做的是循环`hits`，对于我们销毁的每个怪物，将产生另一个新的怪物。

现在它实际上开始感觉像一个游戏：![image](http://upload-images.jianshu.io/upload_images/468490-3b01e181db6bf366.gif?imageMogr2/auto-orient/strip)

### 整合在一起
```
# Shmup game - part 3
# Video link: https://www.youtube.com/watch?v=33g62PpFwsE
# Collisions and bullets
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

    def shoot(self):
        bullet = Bullet(self.rect.centerx, self.rect.top)
        all_sprites.add(bullet)
        bullets.add(bullet)

class Mob(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((30, 40))
        self.image.fill(RED)
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
        self.image = pygame.Surface((10, 20))
        self.image.fill(YELLOW)
        self.rect = self.image.get_rect()
        self.rect.bottom = y
        self.rect.centerx = x
        self.speedy = -10

    def update(self):
        self.rect.y += self.speedy
        # kill if it moves off the top of the screen
        if self.rect.bottom < 0:
            self.kill()

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
    all_sprites.draw(screen)
    # *after* drawing everything, flip the display
    pygame.display.flip()

pygame.quit()
```
在下一课中，我们将学习如何在游戏中添加图形，而不是使用那些纯色矩形。
