pip install pygame
import pygame
import random

# Initialize Pygame
pygame.init()

# Define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Card Game")

# Load card images
card_images = []
for i in range(1, 14):  # Ace to King
    card_image = pygame.image.load(f'cards/{i}.png')  # Card images from 1.png to 13.png
    card_images.append(card_image)

# Deck of cards (1 to 13 for simplicity)
deck = list(range(1, 14)) * 4  # 52 cards, 4 suits
random.shuffle(deck)

# Players
player1_cards = deck[:26]
player2_cards = deck[26:]

# Game variables
player1_score = 0
player2_score = 0
turn = 0  # 0 for player 1, 1 for player 2
game_over = False

# Function to draw text on screen
def draw_text(text, font, color, x, y):
    screen_text = font.render(text, True, color)
    screen.blit(screen_text, (x, y))

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        if event.type == pygame.KEYDOWN:
            if not game_over:
                if event.key == pygame.K_SPACE:  # Press space to draw a card
                    if turn == 0 and player1_cards:
                        p1_card = player1_cards.pop()
                        turn = 1
                    elif turn == 1 and player2_cards:
                        p2_card = player2_cards.pop()
                        turn = 0

                    if len(player1_cards) == 0 or len(player2_cards) == 0:
                        game_over = True
            else:
                if event.key == pygame.K_r:  # Press 'R' to restart the game
                    player1_cards = deck[:26]
                    player2_cards = deck[26:]
                    random.shuffle(deck)
                    game_over = False
                    turn = 0

    # Fill screen background
    screen.fill(WHITE)

    # Display game over message
    if game_over:
        draw_text("Game Over! Press 'R' to restart", pygame.font.Font(None, 36), BLACK, 200, 300)

    # Update the screen
    pygame.display.flip()

# Quit Pygame
pygame.quit()
