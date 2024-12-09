import pygame
import random

# Initialize the game
pygame.init()

# Set the screen dimensions
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Snake Game")

# Define colors
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)
green = (0, 255, 0)

# Define the snake's initial position and size
snake_block_size = 20
snake_speed = 15
snake_list = []

# Define the initial position and size of the food
food_x = round(random.randrange(0, screen_width - snake_block_size) / 20.0) * 20.0
food_y = round(random.randrange(0, screen_height - snake_block_size) / 20.0) * 20.0

# Define the score
score = 0

# Define the font for displaying the score
font_style = pygame.font.SysFont(None, 50)

# Function to display the score on the screen
def display_score(score):
    score_text = font_style.render("Score: " + str(score), True, white)
    screen.blit(score_text, [10, 10])

# Function to display the snake on the screen
def draw_snake(snake_block_size, snake_list):
    for x in snake_list:
        pygame.draw.rect(screen, green, [x[0], x[1], snake_block_size, snake_block_size])

# Main game loop
game_over = False
game_close = False
snake_x = screen_width / 2
snake_y = screen_height / 2
snake_x_change = 0
snake_y_change = 0
snake_list.append([snake_x, snake_y])

clock = pygame.time.Clock()

while not game_over:

    while game_close:
        screen.fill(black)
        game_over_text = font_style.render("Game Over!", True, red)
        screen.blit(game_over_text, [screen_width / 2 - 100, screen_height / 2 - 50])
        display_score(score)
        pygame.display.update()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
                game_close = False

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                snake_x_change = -snake_block_size
                snake_y_change = 0
            elif event.key == pygame.K_RIGHT:
                snake_x_change = snake_block_size
                snake_y_change = 0
            elif event.key == pygame.K_UP:
                snake_y_change = -snake_block_size
                snake_x_change = 0
            elif event.key == pygame.K_DOWN:
                snake_y_change = snake_block_size
                snake_x_change = 0

    if snake_x >= screen_width or snake_x < 0 or snake_y >= screen_height or snake_y < 0:
        game_close = True

    snake_x += snake_x_change
    snake_y += snake_y_change
    screen.fill(black)
    pygame.draw.rect(screen, red, [food_x, food_y, snake_block_size, snake_block_size])
    snake_head = []
    snake_head.append(snake_x)
    snake_head.append(snake_y)
    snake_list.append(snake_head)
    if len(snake_list) > snake_speed:
        del snake_list[0]

    for x in snake_list[:-1]:
        if x == snake_head:
            game_close = True

    draw_snake(snake_block_size, snake_list)
    display_score(score)

    pygame.display.update()

    if snake_x == food_x and snake_y == food_y:
        food_x = round(random.randrange(0, screen_width - snake_block_size) / 20.0) * 20.0
        food_y = round(random.randrange(0, screen_height - snake_block_size) / 20.0) * 20.0
        score += 1

    clock.tick(snake_speed)

pygame.quit()


<!---
Jfydty/Jfydty is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
