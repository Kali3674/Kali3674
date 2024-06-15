- 👋 Hi, I’m @Kali3674
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Kali3674/Kali3674 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random

# Inicjalizacja Pygame
pygame.init()

# Kolory
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Wymiary okna gry
WIDTH = 800
HEIGHT = 600

# Ustawienia okna gry
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Motor Race")

# Ustawienia motocykla
motor_width = 50
motor_height = 100
motor_img = pygame.Surface((motor_width, motor_height))
motor_img.fill(RED)

# Ustawienia przeszkód
obstacle_width = 50
obstacle_height = 100
obstacle_speed = 7

# Funkcja wyświetlania tekstu
font = pygame.font.SysFont(None, 55)
def show_text(text, x, y):
    screen_text = font.render(text, True, WHITE)
    screen.blit(screen_text, [x, y])

# Funkcja główna
def game_loop():
    clock = pygame.time.Clock()
    running = True

    # Pozycja początkowa motocykla
    motor_x = WIDTH // 2
    motor_y = HEIGHT - motor_height - 10
    motor_speed_x = 0

    # Pozycja początkowa przeszkód
    obstacle_x = random.randint(0, WIDTH - obstacle_width)
    obstacle_y = -obstacle_height

    # Wynik
    score = 0

    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    motor_speed_x = -5
                if event.key == pygame.K_RIGHT:
                    motor_speed_x = 5
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    motor_speed_x = 0

        # Ruch motocykla
        motor_x += motor_speed_x
        if motor_x < 0:
            motor_x = 0
        if motor_x > WIDTH - motor_width:
            motor_x = WIDTH - motor_width

        # Ruch przeszkód
        obstacle_y += obstacle_speed
        if obstacle_y > HEIGHT:
            obstacle_y = -obstacle_height
            obstacle_x = random.randint(0, WIDTH - obstacle_width)
            score += 1

        # Kolizje
        if motor_y < obstacle_y + obstacle_height and motor_y + motor_height > obstacle_y:
            if motor_x > obstacle_x and motor_x < obstacle_x + obstacle_width or motor_x + motor_width > obstacle_x and motor_x + motor_width < obstacle_x + obstacle_width:
                running = False

        # Rysowanie elementów
        screen.fill(BLACK)
        screen.blit(motor_img, (motor_x, motor_y))
        pygame.draw.rect(screen, WHITE, [obstacle_x, obstacle_y, obstacle_width, obstacle_height])
        show_text(f"Score: {score}", 10, 10)

        # Aktualizacja ekranu
        pygame.display.flip()
        clock.tick(60)

    # Koniec gry
    show_text("Game Over!", WIDTH // 2 - 100, HEIGHT // 2)
    pygame.display.flip()
    pygame.time.wait(2000)

# Uruchomienie gry
game_loop()
pygame.quit()
