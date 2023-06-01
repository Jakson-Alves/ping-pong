# Ping Pong com Pygames

### - Projeto simples do famoso jogo de ping-pong em Python usando a biblioteca Pygame.
![image](./imagem/Ping-Pong.gif)

### - Criar um executável
```python
# Instalar o pacote pyInstaller
pyinstaller --onefile ping-pong.py
```

### - Subir no Github.


# Set das variáveis
```python
import pygame
from pygame import mixer
import sys

#Inicializa o Pygame e o mixer de áudio
pygame.init()
mixer.init()

#Define as dimensões da tela, da raquete e da bola.
SCREEN_WIDTH =800
SCREEN_HEIGHT = 600
PADDLE_WIDTH = 10
PADDLE_HEIGHT = 60
BALL_SIZE = 10
PADDLE_SPEED = 4
BALL_SPEED = 5

WHITE = (255,255,255)
BLACK = (0,0,0)

font_file = "add path"
font = pygame.font.Font(font_file, 36)
score_a = 0
score_b = 0

mixer.music.load("audios/music_game.mp3")
mixer.music.set_volume(0.3)
collision_sound_A = mixer.Sound("audios/Sound_A.wav")
collision_sound_B = mixer.Sound("audios/Sound_B.wav")
point_sound = mixer.Sound("audios/hoohooo.wav")

mixer.music.play(-1) # Reproduz a música de fundo continuamente
screen = pygame.display.set_mode((SCREEN_WIDTH,SCREEN_HEIGHT)) # Cria uma janela para o jogo com as dimensões definidas
pygame.display.set_caption("Pong") # Define o título da janela do jogo como "Pong"

#pygame.Rect(x,y,width,height)
# Cria as raquetes dos jogadores e a bola usando a classe Rect do Pygame
paddle_a = pygame.Rect(20, SCREEN_HEIGHT // 2 - PADDLE_HEIGHT//2, PADDLE_WIDTH, PADDLE_HEIGHT)
paddle_b = pygame.Rect(SCREEN_WIDTH - 20 - PADDLE_WIDTH, SCREEN_HEIGHT // 2 - PADDLE_HEIGHT//2, PADDLE_WIDTH, PADDLE_HEIGHT)
ball =  pygame.Rect(SCREEN_WIDTH //2 - BALL_SIZE // 2, SCREEN_HEIGHT // 2 - BALL_SIZE//2, BALL_SIZE, BALL_SIZE)
ball_dx, ball_dy = BALL_SPEED, BALL_SPEED

```

# Renderização do Menu



```python
def main_menu():
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    game_loop()
                elif event.key == pygame.K_ESCAPE:
                    pygame.quit()
                    sys.exit()

        # Renderização do menu principal
        screen.fill(BLACK)
        title_font = pygame.font.Font(font_file, 36)  # Configuração da Fonte
        title_text = title_font.render("Pong", True, WHITE)
        title_rect = title_text.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 4))

        screen.blit(title_text, title_rect) # Desenha o texto do título na tela

        title_font = pygame.font.Font(font_file, 16)
        current_time = pygame.time.get_ticks() # Obtém o tempo atual do jogo em milissegundos

        if current_time % 2000 < 1000:
            title_text1 = title_font.render("Pressione espaço para iniciar", True, WHITE)
            title_rect1 = title_text1.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 4 + 60)) # Obtém o retângulo do texto adicional
            screen.blit(title_text1, title_rect1) # Escreve o texto adicional na tela

        
        pygame.display.flip()
```

#### Explicação da Estrutura a seguir:

```python
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    game_loop()
                elif event.key == pygame.K_ESCAPE:
                    pygame.quit()
                    sys.exit()
```
<blockquote style="background-color: #E6F0F7;">

Trata-se de uma estrutura práticada por desenvolvedores de jogos utilizando essa biblioteca.

A estrutura envolve a criação de um loop principal *(while True)* que continua executando indefinidamente até que uma condição de término seja encontrada. Dentro desse loop principal, os eventos são verificados em um loop *for* para capturar as interações do jogador, como o fechamento da janela ou pressionar teclas específicas.

</blockquote>






<blockquote style="background-color: #E6F0F7;">

Podemos ver que esse bloco funciona da seguinte forma:

```python
current_time = pygame.time.get_ticks()
```
A função é chamada para obter o tempo em milissegundos desde que o jogo começou.

```python
if current_time % 2000 < 1000:
```
O trecho de código verifica se o resto da divisão da variável *current_time* por 2000 é menor que 1000. Se essa condição for verdadeira, significa que *current_time* está dentro do intervalo de 0 a 999 milissegundos após cada múltiplo de 2000 milissegundos. Essa verificação é usada para criar um efeito piscante.

Por exemplo, se *current_time* for 2500, o resto da divisão por 2000 é 500. Como 500 é menor que 1000, a condição é verdadeira e o código dentro do bloco if será executado. Isso resultará na renderização do texto desejado. Esse efeito piscante é criado porque o texto só será mostrado durante a primeira metade do intervalo de 2000 milissegundos.

</blockquote>

# Game

```python
def game_loop():
    global ball_dx, ball_dy, score_a, score_b, ball # Define as variáveis globais 

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_ESCAPE:
                    return

        # Preenche a tela com a cor preta
        screen.fill(BLACK) 
        pygame.draw.rect(screen, WHITE, paddle_a)
        pygame.draw.rect(screen, WHITE, paddle_b)
        pygame.draw.ellipse(screen, WHITE, ball)
        pygame.draw.aaline(screen, WHITE, (SCREEN_WIDTH // 2, 0), (SCREEN_WIDTH // 2, SCREEN_HEIGHT))

        # Obtém o estado das teclas pressionadas
        keys = pygame.key.get_pressed()

        #Movimento Vertical Raquete A
        if keys[pygame.K_w] and paddle_a.top > 0:
            paddle_a.y -= PADDLE_SPEED
        if keys[pygame.K_s] and paddle_a.bottom < SCREEN_HEIGHT:
            paddle_a.y += PADDLE_SPEED

        #Movimento Vertical Raquete B
        if keys[pygame.K_UP] and paddle_b.top > 0:
            paddle_b.y -= PADDLE_SPEED
        if keys[pygame.K_DOWN] and paddle_b.bottom < SCREEN_HEIGHT:
            paddle_b.y += PADDLE_SPEED

        #Movimento Horizontal Raquete A
        if keys[pygame.K_a] and paddle_a.left > 0:
            paddle_a.x -= PADDLE_SPEED
        if keys[pygame.K_d] and paddle_a.right < SCREEN_WIDTH // 2 - 70:
            paddle_a.x += PADDLE_SPEED

        #Movimento Horizontal Raquete B
        if keys[pygame.K_LEFT] and paddle_b.left > SCREEN_WIDTH // 2 + 70:
            paddle_b.x -= PADDLE_SPEED
        if keys[pygame.K_RIGHT] and paddle_b.right < SCREEN_WIDTH:
            paddle_b.x += PADDLE_SPEED
      
        # Atualização da posição da bola
        ball.x += ball_dx
        ball.y += ball_dy

        if ball.colliderect(paddle_a):
            ball.left = paddle_a.right
            ball_dx = -ball_dx
            collision_sound_A.play()

        elif ball.colliderect(paddle_b):
            ball.right = paddle_b.left
            ball_dx = -ball_dx
            collision_sound_B.play()

        elif ball.colliderect(paddle_a):
            ball.left = paddle_b.topright
            ball_dx = -ball_dx
            collision_sound_B.play()

        elif ball.colliderect(paddle_b):
            ball.right = paddle_b.topleft
            ball_dx = -ball_dx
            collision_sound_B.play()

        #Bola quando bate na extremidade da tela
        if ball.top <= 0 or ball.bottom >= SCREEN_HEIGHT:
            ball_dy = -ball_dy

        #Ponto para o Time B
        if ball.left <= 0:
            score_b += 1
            ball.x = SCREEN_WIDTH // 2 - BALL_SIZE // 2
            ball.y = SCREEN_HEIGHT // 2 - BALL_SIZE // 2
            ball_dx = -ball_dx
            point_sound.play()
            # print(score_b)
            if score_b == 10: # Quantidade máxima de pontos para B
                end_game(False)

        #Ponto para o Time A
        elif ball.right >= SCREEN_WIDTH:
            score_a += 1
            ball.x = SCREEN_WIDTH // 2 - BALL_SIZE // 2
            ball.y = SCREEN_HEIGHT // 2 - BALL_SIZE // 2
            ball_dx = -ball_dx
            point_sound.play()
            # print(score_a)
            if score_a == 10: # Quantidade máxima de pontos para A
                end_game(True)

        # Placar na Tela
        score_text = font.render(f"{score_a}  {score_b}", True, WHITE)
        score_rect = score_text.get_rect(center=(SCREEN_WIDTH // 2, 30))
        screen.blit(score_text, score_rect)

        # Atualizar a tela
        pygame.display.flip()

        # Controlar FPS
        clock = pygame.time.Clock()
        clock.tick(60)
```

#### Explicação da Estrutura a seguir - Movimento Vertical:

```python
        #Movimento Vertical Raquete A

        # Verifica se a tecla W está pressionada e se a raquete A não está no topo da tela 
        if keys[pygame.K_w] and paddle_a.top > 0: 
            paddle_a.y -= PADDLE_SPEED  # Move a raquete A para cima
        if keys[pygame.K_s] and paddle_a.bottom < SCREEN_HEIGHT:
            paddle_a.y += PADDLE_SPEED

        #Movimento Vertical Raquete B

        # Verifica se a tecla de seta para cima está pressionada e se a raquete B não está no topo da tela
        if keys[pygame.K_UP] and paddle_b.top > 0:
            paddle_b.y -= PADDLE_SPEED # Move a raquete B para cima
        if keys[pygame.K_DOWN] and paddle_b.bottom < SCREEN_HEIGHT:
            paddle_b.y += PADDLE_SPEED
```
<blockquote style="background-color: #E6F0F7;">

Essa estrutura de código controla o movimento vertical das raquetes A e B, verificando se as teclas W (cima) e S (baixo) estão pressionadas para mover a raquete A, e se as teclas de seta para cima e para baixo estão pressionadas para mover a raquete B. As verificações adicionais garantem que as raquetes não ultrapassem as bordas superior e inferior da tela.

</blockquote>

#### Explicação da Estrutura a seguir - Movimento Horizontal:

```python
        #Movimento Horizontal Raquete A

        # Verifica se a tecla A está pressionada e se a raquete A não está no limite esquerdo da tela
        if keys[pygame.K_a] and paddle_a.left > 0:
            paddle_a.x -= PADDLE_SPEED # Move a raquete A para a esquerda
        if keys[pygame.K_d] and paddle_a.right < SCREEN_WIDTH // 2 - 70:
            paddle_a.x += PADDLE_SPEED

        #Movimento Horizontal Raquete B

         # Verifica se a tecla de seta para a esquerda está pressionada e se a raquete B não está no limite esquerdo da tela
        if keys[pygame.K_LEFT] and paddle_b.left > SCREEN_WIDTH // 2 + 70: # Move a raquete B para a esquerda
            paddle_b.x -= PADDLE_SPEED
        if keys[pygame.K_RIGHT] and paddle_b.right < SCREEN_WIDTH:
            paddle_b.x += PADDLE_SPEED
```
<blockquote style="background-color: #E6F0F7;">

Essa estrutura de código controla o movimento horizontal das raquetes A e B, verificando se as teclas A (esquerda) e D (direita) estão pressionadas para mover a raquete A, e se as teclas de seta para direita e para esquerda estão pressionadas para mover a raquete B. As verificações adicionais garantem que as raquetes não ultrapassem as bordas esquerda e direita da tela.

</blockquote>

#### Explicação Sistema de Colisão a seguir:

```python
        # Atualização da posição da bola
        ball.x += ball_dx
        ball.y += ball_dy
        
        # Verifica se houve colisão entre a raquete A e a bola, se sim, emite o som
        if ball.colliderect(paddle_a):
            ball.left = paddle_a.right
            ball_dx = -ball_dx
            collision_sound_A.play()

        # Verifica se houve colisão entre a raquete B e a bola, se sim, emite o som
        elif ball.colliderect(paddle_b):
            ball.right = paddle_b.left
            ball_dx = -ball_dx
            collision_sound_B.play()

        # Verifica se houve colisão entre a raquete A e a bola no topo da raquete, se sim, emite o som
        elif ball.colliderect(paddle_a):
            ball.left = paddle_b.topright
            ball_dx = -ball_dx
            collision_sound_B.play()

        # Verifica se houve colisão entre a raquete B e a bola no topo da raquete, se sim, emite o som
        elif ball.colliderect(paddle_b):
            ball.right = paddle_b.topleft
            ball_dx = -ball_dx
            collision_sound_B.play()
```
<blockquote style="background-color: #E6F0F7;">

Esse código atualiza a posição da bola e lida com colisões entre a bola e as raquetes A e B, reproduzindo um som quando colidem. Após uma colisão, a posição da bola é ajustada e sua direção horizontal é invertida. 

</blockquote>

<blockquote style="background-color: #EAB8AD;">

Explique o Sistema de Pontuação colocando o código e comentando detalhadamente.

</blockquote>

#### Explicação Pontuação a seguir:

```python
        #Bola quando bate na extremidade da tela
        if ball.top <= 0 or ball.bottom >= SCREEN_HEIGHT:
            ball_dy = -ball_dy

        #Verifica se a bola ultrapassou a extremidade esquerda da tela, o que significa um ponto para o Time B.
        if ball.left <= 0:
            score_b += 1
            ball.x = SCREEN_WIDTH // 2 - BALL_SIZE // 2
            ball.y = SCREEN_HEIGHT // 2 - BALL_SIZE // 2
            ball_dx = -ball_dx
            point_sound.play()
            # print(score_b)
            if score_b == 10: # Quantidade máxima de pontos para B
                end_game(False)

        #Verifica se a bola ultrapassou a extremidade direita da tela, o que significa um ponto para o Time A.
        elif ball.right >= SCREEN_WIDTH:
            score_a += 1
            ball.x = SCREEN_WIDTH // 2 - BALL_SIZE // 2
            ball.y = SCREEN_HEIGHT // 2 - BALL_SIZE // 2
            ball_dx = -ball_dx
            point_sound.play()
            # print(score_a)
            if score_a == 10: # Quantidade máxima de pontos para A
                end_game(True)

        # Placar na Tela
        score_text = font.render(f"{score_a}  {score_b}", True, WHITE)
        score_rect = score_text.get_rect(center=(SCREEN_WIDTH // 2, 30))
        screen.blit(score_text, score_rect)
       
```
<blockquote style="background-color: #E6F0F7;">

Esse código verifica se a bola ultrapassou a extremidade esquerda da tela, indicando um ponto para o Time B. Incrementa o placar do Time B, reposiciona a bola no centro da tela, inverte sua direção horizontal, reproduz um som de ponto e verifica se o Time B alcançou a quantidade máxima de pontos permitida para encerrar o jogo. O mesmo ocorre para o Time A.

</blockquote>

#Fim de Jogo

```python
def end_game(winner): 
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    reset_game()
                    return
                elif event.key == pygame.K_ESCAPE:
                    pygame.quit()
                    sys.exit()

       
        mixer.music.stop()
        screen.fill(BLACK)
        #Comente
        if winner:
            winner_text = "Player 2 Wins!"
        else:
            winner_text = "Player 1 Wins!"

        # Renderização da tela de fim de jogo
        winner_font = pygame.font.Font(font_file, 36)
        winner_render = winner_font.render(winner_text, True, WHITE)
        winner_rect = winner_render.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 4))
        screen.blit(winner_render, winner_rect)
        pygame.display.flip()
```
<blockquote style="background-color: #E6F0F7;">

Esse código implementa a tela de fim de jogo, apresentando quem foi o vencedor, permitindo reiniciar ou encerrar o jogo de acordo com as teclas pressionadas.

</blockquote>

# Reiniciando o Jogo
```python
def reset_game():
    global paddle_a, paddle_b, ball, ball_dx, ball_dy, score_a, score_b

    paddle_a = pygame.Rect(20, SCREEN_HEIGHT // 2 - PADDLE_HEIGHT // 2, PADDLE_WIDTH, PADDLE_HEIGHT)
    paddle_b = pygame.Rect(SCREEN_WIDTH - 20 - PADDLE_WIDTH, SCREEN_HEIGHT // 2 - PADDLE_HEIGHT // 2, PADDLE_WIDTH, PADDLE_HEIGHT)
    ball = pygame.Rect(SCREEN_WIDTH // 2 - BALL_SIZE // 2, SCREEN_HEIGHT // 2 - BALL_SIZE // 2, BALL_SIZE, BALL_SIZE)
    ball_dx, ball_dy = BALL_SPEED, BALL_SPEED
    score_a, score_b = 0, 0
```

<blockquote style="background-color: #E6F0F7;">

Restaura as váriaveis para seus valores iniciais, permitindo que o jogo seja reiniciado.

</blockquote># ping-pong
