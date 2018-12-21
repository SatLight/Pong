# Pong
The classic arcade game Pong coded by me in Python!


import pygame
import sys
import random
def ran():
    return random.randint(-4,4)

BGC=(0,0,0) #background colour

pygame.init() #initializes Pygame

pygame.mouse.set_visible(0) #makes the cursor invisible over the game screen
WIDTH=1400 #width of the game screen
HEIGHT=650 #height of the game screen
PC=(0,0,255) #boundary colour
RED=(255,0,0) #RGB code for red
BC=(255,255,255) #ball colour
LC=(255,255,255) #line colour
player_pos1=[700-100,HEIGHT-75] #bottom handle
player_pos2=[700-100,75-20] #top handle
xin1=0 #initial position of P1 handle
xin2=0 #initial position of P2 handle
n=(WIDTH//2,HEIGHT//2,75) #middle circle
nc=(57,255,20)  #colour for middle circle
bx=WIDTH//2 #initial position of ball in x axis
by=HEIGHT//2 #initial position of ball in y axis
bxin=0 
byin=1
r=10 #radius of the ball
p1w=0
p2w=0
#width = 800 , height = 600

screen = pygame.display.set_mode((WIDTH,HEIGHT))
pygame.display.set_caption('PONG!!')
font = pygame.font.Font(None, 36)
gameIcon = pygame.image.load('Pong_Icon.png')
pygame.display.set_icon(gameIcon)

pause=False

def text_objects(text, font):
    textSurface = font.render(text, True, (194,14,213))
    return textSurface, textSurface.get_rect()

def gameover(a,b):
    if a==1:
        largeText = pygame.font.SysFont("comicsansms",115)
        TextSurf, TextRect = text_objects("PLAYER 1 WON!", largeText)
        TextRect.center = ((WIDTH/2),(HEIGHT/2))
        screen.blit(TextSurf, TextRect)  
    if b==1:
        largeText = pygame.font.SysFont("comicsansms",115)
        TextSurf, TextRect = text_objects("PLAYER 2 WON!", largeText)
        TextRect.center = ((WIDTH/2),(HEIGHT/2))
        screen.blit(TextSurf, TextRect)
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
        pygame.display.update()

def paused():
    largeText = pygame.font.SysFont("comicsansms",115)
    TextSurf, TextRect = text_objects("Paused", largeText)
    TextRect.center = ((WIDTH/2),(HEIGHT/2))
    screen.blit(TextSurf, TextRect)

# Main Game Code

game_over=False
while not game_over:
    for event in pygame.event.get():          #for tracking event same as kbhit() function in c++
        if event.type == pygame.QUIT:
                pygame.quit()
        if event.type==pygame.KEYDOWN:
            if event.key == pygame.K_LEFT and xin1 >=0:
                xin1-=3
            elif event.key==pygame.K_RIGHT and xin1 <=0:
                xin1+=3
            elif event.key==pygame.K_a and xin2>=0:
                xin2-=3
            elif event.key==pygame.K_d and xin2<=0:
                xin2+=3
            elif event.key == pygame.K_p and pause==False:
                pause =True
                pbxin=bxin
                pbyin=byin
                pxin1=xin1
                pxin2=xin2
                xin1=0
                xin2=0
                bxin=0
                byin=0
                paused()
            elif event.key == pygame.K_p and pause==True:
                xin1=pxin1
                xin2=pxin2
                bxin=pbxin
                byin=pbyin
                pause=False
        if pause==True:
            xin1=0
            xin2=0

    '''    if event.type==pygame.KEYUP:
            if event.key == pygame.K_LEFT and xin1 >=0:
                xin1-=4
            elif event.key==pygame.K_RIGHT and xin1 <=0:
                xin1+=4
            elif event.key==pygame.K_a and xin2>=0:
                xin2-=4
            elif event.key==pygame.K_d and xin2<=0:
                xin2+=4
            elif event.key == pygame.K_p:
                pause=False
                paused()
'''

    if player_pos1[0] >= WIDTH and xin1 > 0:
        player_pos1[0] = -200
    if player_pos1[0] <= -200 and xin1 <0:
        player_pos1[0]=WIDTH
    if player_pos2[0] >= WIDTH and xin2 >0:
        player_pos2[0] = -200
    if player_pos2[0] <= -200 and xin2 <0:
        player_pos2[0]=WIDTH

    if  ( by<=HEIGHT-75 and by>=HEIGHT-75-15 ) and bx>=player_pos1[0]-20 and (bx<=player_pos1[0]+220):
        byin=-1
        bxin=ran()
    if ( by>=75 and by<=75+15) and bx>=player_pos2[0]-20 and (bx<=player_pos2[0]+220):
        byin=1
        bxin=ran()
    if by<=0:
        p1w=1
    if by>=HEIGHT:
        p2w=1

    if bx<=0 or bx>=WIDTH:
        bxin=-bxin


    if p1w==1 or p2w==1:
        bxin=0
        byin=0
        gameover(p1w,p2w)


    player_pos1[0]+=xin1
    player_pos2[0]+=xin2
    bx+=bxin
    by+=byin



        
    screen.fill(BGC)
    pygame.draw.line(screen,LC,(0,HEIGHT//2),(WIDTH,HEIGHT//2))
    pygame.draw.rect(screen,PC,(player_pos1[0],player_pos1[1],200,20))
    pygame.draw.rect(screen,PC,(player_pos2[0],player_pos2[1],200,20))

    '''    largeText = pygame.font.SysFont("comicsansms",25)                              #
    TextSurf, TextRect = text_objects("P1", largeText)                               #
    TextRect.center = (700,HEIGHT-55+20)                   #
    screen.blit(TextSurf, TextRect)                                                         #
    
    largeText = pygame.font.SysFont("comicsansms",25)                              #
    TextSurf, TextRect = text_objects("P2", largeText)                               #
    TextRect.center = (700,75-20-20)                   #
    screen.blit(TextSurf, TextRect)                                                         #'''
    
    pygame.draw.circle(screen,nc,(n[0],n[1]),n[2])
    pygame.draw.circle(screen,(0,0,0),(n[0],n[1]),n[2]-3)
    pygame.draw.circle(screen,BC,(bx,by),r)
    pygame.draw.rect(screen,RED,(0,0,20,HEIGHT))
    pygame.draw.rect(screen,RED,(WIDTH-20,0,20,HEIGHT))

    if pause==True:
            largeText = pygame.font.SysFont("comicsansms",115)
            TextSurf, TextRect = text_objects("Paused", largeText)
            TextRect.center = ((WIDTH/2),(HEIGHT/2))
            screen.blit(TextSurf, TextRect)
        
    pygame.display.update()
    


