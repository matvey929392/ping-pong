# ping-pong
from pygame import *

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed, wight, height):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (wight, height)) #вместе 55,55 - параметры
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y


    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))


class Player(GameSprite):
    def update1(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed
    def update2(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed

font.init()
font = font.Font(None, 70)

lose = font.render(
    'YOU LOSE', True, (242, 11, 11)
)

back = (14, 240, 146)
win_width = 600
win_height = 500
window = display.set_mode((win_width, win_height))
window.fill(back)

game = True
finish = False
clock = time.Clock()
FPS = 60

rocket1 = Player('t.png', 0, 200, 7, 25, 150) 
rocket2 = Player('t.png', 575, 200, 7, 25, 150)
ball = GameSprite('w.png', 300, 250, 10, 50, 50)

speed_x = 4
speed_y = 4



while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
  
    if finish != True:
        window.fill(back)
        ball.rect.x += speed_x
        ball.rect.y += speed_y
        ball.update()
        rocket1.update2()
        rocket2.update1()
        rocket1.reset()
        rocket2.reset()
        ball.reset()
        if sprite.collide_rect(rocket1, ball) or sprite.collide_rect(rocket2, ball):
            speed_x *= -1
            speed_y *= 1
        if ball.rect.y > win_height-50 or ball.rect.y < 0:
            speed_y *= -1
        if ball.rect.x > win_width:
            window.blit(lose, (180, 200))
            finish = True
        


    display.update()
    clock.tick(FPS)
