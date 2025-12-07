from pygame import *
from random import randint
from time import time as timer
mixer.init()
font.init()
wind = display.set_mode((700, 500))
background = transform.scale(image.load("background3.png"), (700, 500))
mixer.music.load('space.ogg')
mixer.music.play(-1)
mixer.music.set_volume(0.1)
fire_music = mixer.Sound("shoot_sound.mp3")
clock = time.Clock()
FPS = 60
HP = 100
game = True
finish = False
lost = 0
points = 0
num_fire = 0
rel_time = False
health = 3
font1 = font.Font(None, 25)
font2 = font.Font(None, 70)
font3 = font.Font(None, 45)
ind_width = 90
ind_color = 0,255,0
class GameSprites(sprite.Sprite):
    def __init__(self, player_image, x, y, W, H, speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (W, H))
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed = speed

    def draw_sp(self):
        wind.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprites):
    def steps(self):
        key_press = key.get_pressed()
        if key_press[K_LEFT] and self.rect.x > 60:
            self.rect.x -= self.speed
        if key_press[K_RIGHT] and self.rect.x < 580:
            self.rect.x += self.speed

    def fire(self):
        bullet = Bullet("bulletxxx.png", self.rect.centerx, self.rect.top, 5, 20, -15)
        Bullets.add(bullet)

class Enemy(GameSprites):
    def update(self):
        self.rect.y += self.speed
        global lost
        if self.rect.y > 470:
            self.rect.x = randint(50, 550)
            self.rect.y = -30
            lost += 1

class Meteor(GameSprites):
    def update(self):
        self.rect.y += self.speed
        if self.rect.y > 480:
            self.rect.x = randint(50, 550)
            self.rect.y = -30

class Bullet(GameSprites):
    def update(self):
        self.rect.y += self.speed
        if self.rect.y < 0:
            self.kill()

class Indicators(sprite.Sprite): 
    def __init__(self, color, x, y, w, h):
        super().__init__()
        self.color = color
        self.width = w
        self.height = h
        self.image = Surface((self.width, self.height))
        self.image.fill((color))
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

    def draw_indicator(self):
        wind.blit(self.image, (self.rect.x, self.rect.y))

player = Player("player.png", 320, 435, 60, 60, 8)
Bullets = sprite.Group()
monsters = sprite.Group()
asteroids = sprite.Group()
for _ in range(5):
    monster = Enemy("enemy2_1.png", randint(50, 550), -30, 60, 60, randint(1, 2))
    monsters.add(monster)
for _ in range(3):
    asteroid = Enemy("meteor_2.png", randint(50, 550), -30, 60, 60, randint(1, 3))
    asteroids.add(asteroid)
while game:
    ind1 = Indicators(ind_color, 45, 60, ind_width, 10)
    for e in event.get():
        if e.type == QUIT:
            game = False
        if e.type == KEYDOWN:
            if e.key == K_SPACE:
                if num_fire < 5 and rel_time == False:
                    player.fire()
                    fire_music.play()
                    num_fire += 1
                if num_fire >= 5 and rel_time == False:
                    rel_time = True
                    fix_time = timer()
                    
    if finish != True:            
        wind.blit(background, (0,0))
        text_lose = font1.render("Пропущено: " + str(lost), True, (255,255,255))
        wind.blit(text_lose, (10, 10))
        point = font1.render("Счет: " + str(points), True, (255,255,255))
        wind.blit(point, (10, 35))
        point = font1.render("HP", True, (255,255,255))
        wind.blit(point, (10, 55))
        lose = font2.render("YOU LOSE", True, (0,0,255))
        win = font2.render("YOU WIN", True, (255,0,0))
        reloading_text = font3.render("Wait, reload...", True, (255,0,10))
        player.draw_sp()
        player.steps()
        asteroid.draw_sp()
        asteroid.update()
        ind1.draw_indicator()
        monsters.draw(wind)
        monsters.update()
        Bullets.draw(wind)
        Bullets.update()
        
        if health == 2:
            ind_width = 60
            ind_color = 255,255,0
        if health == 1:
            ind_width = 30
            ind_color = 255,0,0
        if rel_time == True:
            fix_time2 = timer()
            if fix_time2-fix_time != 3:
                wind.blit(reloading_text, (220, 400))
            else:
                num_fire = 0
                rel_time = False
        collide = sprite.groupcollide(Bullets, monsters, True, True)
        for _ in collide:
            points += 1
            monster = Enemy("enemy2_1.png", randint(50, 550), -30, 60, 60, randint(1, 3))
            monsters.add(monster)
        if sprite.spritecollide(player, asteroids, True) or sprite.spritecollide(player, monsters, True):
            health -= 1
        if health < 1 or lost > 9:
            ind_width = 1
            wind.blit(lose, (220, 220))
            finish = True
        if points >= 10:
            wind.blit(win, (220, 220))
            finish = True
    display.update()
    clock.tick(FPS)
