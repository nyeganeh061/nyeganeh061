pygame.init()

# Constants
WIDTH, HEIGHT = 640, 480
BALL_SPEED = [4, 4]
PADDLE_SPEED = 5
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Create the window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Ping Pong Game")

# Initialize game objects
ball = pygame.Rect(WIDTH // 2, HEIGHT // 2, 20, 20)
paddle = pygame.Rect(WIDTH // 2 - 60, HEIGHT - 20, 120, 10)

# Initialize game variables
ball_dx = random.choice((-1, 1))
ball_dy = -1

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and paddle.left > 0:
        paddle.move_ip(-PADDLE_SPEED, 0)
    if keys[pygame.K_RIGHT] and paddle.right < WIDTH:
        paddle.move_ip(PADDLE_SPEED, 0)

    # Update the ball position
    ball.move_ip(ball_speed[0] * ball_dx, ball_speed[1] * ball_dy)

    # Ball collision with walls
    if ball.left < 0 or ball.right > WIDTH:
        ball_dx *= -1
    if ball.top < 0:
        ball_dy = 1

    # Ball collision with the paddle
    if ball.colliderect(paddle) and ball_dy > 0:
        ball_dy *= -1

    # Ball out of bounds (missed)
    if ball.top > HEIGHT:
        # Reset the ball
        ball.topleft = (WIDTH // 2, HEIGHT // 2)
        ball_dx = random.choice((-1, 1))
        ball_dy = -1

    # Clear the screen
    screen.fill(BLACK)

    # Draw objects
    pygame.draw.rect(screen, WHITE, ball)
    pygame.draw.rect(screen, WHITE, paddle)

    # Update the display
    pygame.display.flip()

# Quit Pygame
pygame.quit()
