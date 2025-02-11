# Whack-a-mole-game
Creator-Arapuspa-Mallick
import pygame
import random
import time

# Initialize Pygame
pygame.init()

# Game Constants
WIDTH, HEIGHT = 600, 400
MOLE_SIZE = 80
WHITE = (255, 255, 255)
BROWN = (139, 69, 19)
GREEN = (34, 139, 34)

# Setup Display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Whack-A-Mole 🐾")

# Load mole image
mole_img = pygame.Surface((MOLE_SIZE, MOLE_SIZE))
mole_img.fill(BROWN)

# Game Variables
score = 0
mole_position = None
mole_time = 0
game_duration = 30  # 30 seconds
game_over = False

# Font for score display
font = pygame.font.SysFont(None, 36)

# Function to display the score
def show_score():
    score_text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(score_text, (10, 10))

# Function to display game over
def game_over_message():
    game_over_text = font.render(f"Game Over! Final Score: {score}", True, WHITE)
    screen.blit(game_over_text, (WIDTH//2 - 150, HEIGHT//2))

# Game Loop
start_time = time.time()
while True:
    screen.fill(GREEN)  # Background color

    if game_over:
        game_over_message()
        pygame.display.update()
        continue

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()

        # Detect mouse click on mole
        if event.type == pygame.MOUSEBUTTONDOWN and mole_position:
            mouse_x, mouse_y = pygame.mouse.get_pos()
            mole_rect = pygame.Rect(mole_position[0], mole_position[1], MOLE_SIZE, MOLE_SIZE)
            if mole_rect.collidepoint(mouse_x, mouse_y):
                 score += 1  # Increase score for hitting the mole
                mole_position = None  # Hide the mole

    # Randomly pop up a mole every 1-3 seconds
    if mole_position is None or time.time() - mole_time > random.uniform(1, 3):
        mole_position = (random.randint(0, WIDTH - MOLE_SIZE), random.randint(0, HEIGHT - MOLE_SIZE))
        mole_time = time.time()

    # Draw the mole if it's visible
    if mole_position:
        screen.blit(mole_img, mole_position)

    # Update the score
    show_score()

    # Check if game time has ended
    if time.time() - start_time > game_duration:
        game_over = True

    # Update the screen
    pygame.display.update()

    # Limit the frame rate
    pygame.time.Clock().tick(30)
