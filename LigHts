import pygame
import random
import math

# Inițializare Pygame și mixer pentru sunet
pygame.init()
pygame.mixer.init()

# Încarcă imaginile cufărului
chest_open_image = pygame.image.load(r"C:\Users\DELL Vostro User\Desktop\My_Games\1355900.png")
chest_closed_image = pygame.image.load(r"C:\Users\DELL Vostro User\Desktop\My_Games\1355876.png")

# Redimensionăm imaginile pentru a se potrivi (opțional)
chest_width, chest_height = 100, 100  # Dimensiunile dorite
chest_open_image = pygame.transform.scale(chest_open_image, (chest_width, chest_height))
chest_closed_image = pygame.transform.scale(chest_closed_image, (chest_width, chest_height))


try:
    emoji_font = pygame.font.SysFont("Segoe UI Emoji", 28)  # Font that supports emojis on most systems
except:
    emoji_font = pygame.font.Font(None, 28)  # Fallback to default font if "Segoe UI Emoji" is unavailable


# Încărcăm sunetul de victorie
win_sound = pygame.mixer.Sound("588234__mehraniiii__win.wav")
spin_click_sound = pygame.mixer.Sound("wheel-spin-click-slow-down-fast-101154.mp3")
coin_sound = pygame.mixer.Sound("coins-sound-effect-1-241818.mp3")



# Dimensiuni fereastră
screen_width = 400
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Luminile Interzise")

# Paletă de culori actualizată
PRIMARY_COLOR = (45, 45, 45)  # Gri închis
SECONDARY_COLOR = (200, 200, 200)  # Gri deschis
ACCENT_COLOR = (124, 252, 0)  # Verde teal vibrant
HIGHLIGHT_COLOR = (255, 87, 34)  # Portocaliu ars
BACKGROUND_COLOR = (20, 20, 30)  # Gri-albastru întunecat


# Culori pentru butoane, text, și altele
BUTTON_COLOR = PRIMARY_COLOR
BUTTON_HOVER_COLOR = SECONDARY_COLOR
BUTTON_BORDER_COLOR = HIGHLIGHT_COLOR
TEXT_COLOR = ACCENT_COLOR
GRID_LINE_COLOR = HIGHLIGHT_COLOR
WIN_BG_COLOR = PRIMARY_COLOR
WIN_TEXT_COLOR = BACKGROUND_COLOR
GLOW_COLOR = SECONDARY_COLOR

# Niveluri de dificultate (dimensiuni grid)
levels = [{"grid_size": i} for i in range(3, 13)]

# Inițializare variabile
current_level = 0
score = 0
moves = 0
solution_steps = []
current_step_index = 0
game_state = "home"  # starea inițială a jocului este "home" (ecranul principal)
# Variabilă pentru a controla starea sunetului
is_music_muted = False
font = pygame.font.SysFont('Segoe UI Emoji', 30)
emeralds = 100  # Total emeralde colectate
# Add state variables to track main and sub-level selections

# Încarcă și redă muzica de fundal
pygame.mixer.music.load("drunk-on-funk-273910.mp3")
pygame.mixer.music.play(-1)  # -1 pentru a reda în buclă

# Daily Rewards Initialization
from datetime import datetime, timedelta

# Variables to track rewards progress
daily_streak = 1  # Current streak
last_claimed_date = None  # Last date the reward was claimed


def draw_button(rect, text, mouse_pos, font):
    """
    Desenează un buton și ajustează culorile în funcție de hover.
    :param rect: Rect-ul butonului.
    :param text: Textul afișat pe buton.
    :param mouse_pos: Poziția mouse-ului.
    :param font: Font-ul utilizat pentru text.
    """
    is_hovered = rect.collidepoint(mouse_pos)
    button_color = SECONDARY_COLOR if is_hovered else PRIMARY_COLOR
    text_color = BACKGROUND_COLOR if is_hovered else ACCENT_COLOR  # Contrast pentru text

    # Desenează butonul
    pygame.draw.rect(screen, button_color, rect, border_radius=10)
    pygame.draw.rect(screen, HIGHLIGHT_COLOR, rect, 2, border_radius=10)  # Marginea butonului

    # Desenează textul
    text_surface = font.render(text, True, text_color)
    text_rect = text_surface.get_rect(center=rect.center)
    screen.blit(text_surface, text_rect)

def draw_rewards_button(mouse_pos):
    font = pygame.font.SysFont('Segoe UI Emoji', 30)
    button_radius = 40
    button_x = screen_width // 2
    button_y = button_radius + 10
    button_center = (button_x, button_y)
    if pygame.Rect(button_x - button_radius, button_y - button_radius, button_radius * 2, button_radius * 2).collidepoint(mouse_pos):
        glow_surface = pygame.Surface((button_radius * 2 + 20, button_radius * 2 + 20), pygame.SRCALPHA)
        pygame.draw.circle(
            glow_surface,
            (*GLOW_COLOR, 80),
            (glow_surface.get_width() // 2, glow_surface.get_height() // 2),
            glow_surface.get_width() // 2,
        )
        screen.blit(glow_surface, (button_x - button_radius - 10, button_y - button_radius - 10))
    button_color = BUTTON_HOVER_COLOR if pygame.Rect(button_x - button_radius, button_y - button_radius, button_radius * 2, button_radius * 2).collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.circle(screen, button_color, button_center, button_radius)
    pygame.draw.circle(screen, BUTTON_BORDER_COLOR, button_center, button_radius, 2)
    text_surface = font.render("🎁", True, TEXT_COLOR)
    text_rect = text_surface.get_rect(center=button_center)
    screen.blit(text_surface, text_rect)
    return pygame.Rect(button_x - button_radius, button_y - button_radius, button_radius * 2, button_radius * 2)

def draw_rewards_page(mouse_pos):
    """Draw the Daily Rewards page with 30 buttons, symmetrically centered."""
    screen.fill(BACKGROUND_COLOR)  # Înlocuire BLACK
    font = pygame.font.Font(None, 36)

    # Title
    title_font = pygame.font.Font(None, 50)
    title_surface = title_font.render("Daily Rewards", True, TEXT_COLOR)  # Înlocuire TEXT
    title_rect = title_surface.get_rect(center=(screen_width // 2, 50))
    screen.blit(title_surface, title_rect)

    # Calculate starting X position for centering the buttons
    button_width = 50
    button_spacing = 60  # The space between buttons
    total_width = button_spacing * 6 - 10  # 6 buttons per row with the desired spacing
    start_x = (screen_width - total_width) // 2  # Center the grid horizontally

    # Grid of buttons
    buttons = []
    for i in range(30):
        row = i // 6
        col = i % 6
        x = start_x + col * button_spacing
        y = 120 + row * button_spacing
        button_rect = pygame.Rect(x, y, button_width, button_width)
        button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
        pygame.draw.rect(screen, button_color, button_rect, border_radius=10)
        pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=10)

        # Button text (1 to 30)
        button_text = font.render(str(i + 1), True, TEXT_COLOR)  # Înlocuire WHITE
        text_rect = button_text.get_rect(center=button_rect.center)
        screen.blit(button_text, text_rect)

        # Highlight current streak button
        if i + 1 == daily_streak:
            pygame.draw.rect(screen, GLOW_COLOR, button_rect, 4, border_radius=10)

        buttons.append(button_rect)

    # Buton "Back"
    back_button_rect = pygame.Rect(10, screen_height - 60, 100, 40)
    back_button_color = BUTTON_HOVER_COLOR if back_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, back_button_color, back_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, back_button_rect, 2, border_radius=10)
    back_text = emoji_font.render("Back", True, TEXT_COLOR)  # Înlocuire WHITE
    screen.blit(back_text, (back_button_rect.x + 20, back_button_rect.y + 10))

    return buttons, back_button_rect




def claim_reward(streak, last_date):
    """Handle the logic for claiming daily rewards."""
    global emeralds
    today = datetime.now().date()

    if last_date == today:
        print("Reward already claimed today!")
        return streak, last_date

    # If not consecutive day, reset streak
    if last_date is None or (today - last_date).days > 1:
        streak = 1
    else:
        streak += 1

    # Calculate reward
    reward = 5 * (2 ** (streak - 1))
    emeralds += reward
    print(f"Claimed {reward} emeralds! Total emeralds: {emeralds}")

    # Reset streak if reached 30
    if streak > 30:
        streak = 1

    return streak, today

# Funcție pentru a crea rețeaua de lumini
def create_lights(grid_size):
    return [[0 for _ in range(grid_size)] for _ in range(grid_size)]


def draw_mute_button(mouse_pos):
    global is_music_muted
    font = pygame.font.SysFont('Segoe UI Emoji', 30)
    button_rect = pygame.Rect(10, 10, 40, 40)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR

    # Desenează cercul butonului
    pygame.draw.circle(screen, button_color, button_rect.center, button_rect.width // 2)
    pygame.draw.circle(screen, BUTTON_BORDER_COLOR, button_rect.center, button_rect.width // 2, 2)

    # Simbol pentru mute / unmute
    icon = "🔊" if not is_music_muted else "🔇"
    text_surface = font.render(icon, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    text_rect = text_surface.get_rect(center=button_rect.center)
    screen.blit(text_surface, text_rect)

    return button_rect


def collect_emeralds(level):
    global emeralds
    emeralds += level * 5
    print(f"Felicitări! Ai adunat {level * 5} emeralde. Total emeralde: {emeralds}")

def draw_emerald_button(mouse_pos):
    # Utilizăm un font care suportă emoji-uri
    font = pygame.font.SysFont('Segoe UI Emoji', 30)
    button_rect = pygame.Rect(screen_width // 2 - 60, screen_height // 2 + 140, 120, 60)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=10)

    # Adăugăm simbolurile de smarald și cristal
    emerald_icon = "💎"

    # Textul afișat pe buton
    text = font.render(f"{emerald_icon} {emeralds}", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR

    # Afișăm textul pe buton
    screen.blit(text, (button_rect.x + 10, button_rect.y + 20))
    return button_rect


def toggle_music():
    global is_music_muted
    if is_music_muted:
        pygame.mixer.music.unpause()
    else:
        pygame.mixer.music.pause()
    is_music_muted = not is_music_muted


def draw_background():
    for i in range(screen_height):
        r, g, b = [
            int(BACKGROUND_COLOR[j] + (PRIMARY_COLOR[j] - BACKGROUND_COLOR[j]) * i / screen_height)
            for j in range(3)
        ]
        pygame.draw.line(screen, (r, g, b), (0, i), (screen_width, i))


# Definiții suplimentare necesare
colors = [
    [(255, 255, 100), (50, 50, 50)],  # Nivel 1
    [(200, 50, 50), (30, 30, 30)],    # Nivel 2
    [(50, 200, 50), (30, 50, 30)]     # Nivel 3
]
current_level = 0
current_light_shape = "circle"
GRID_LINE_COLOR = HIGHLIGHT_COLOR

def draw_lights(lights, grid_size):
    light_size = screen_width // grid_size
    for row in range(grid_size):
        for col in range(grid_size):
            # Culoarea luminilor, în funcție de nivel
            light_color = colors[current_level % len(colors)]

            # Pulsare subtilă a luminilor aprinse
            pulse = int(20 * math.sin(pygame.time.get_ticks() * 0.005 + (row + col) * 0.1))
            color = tuple(min(255, max(0, x + pulse)) for x in light_color[0]) if lights[row][col] == 1 else light_color[1]

            rect = pygame.Rect(col * light_size, row * light_size, light_size, light_size)

            # Desenează lumina în funcție de forma selectată
            if current_light_shape == "circle":
                pygame.draw.ellipse(screen, color, rect)  # Cerc
            elif current_light_shape == "triangle":
                points = [
                    (rect.centerx, rect.top),  # Vârful de sus
                    rect.bottomleft,  # Colțul stânga-jos
                    rect.bottomright  # Colțul dreapta-jos
                ]
                pygame.draw.polygon(screen, color, points)  # Triunghi
            else:  # Implicit, pătrat cu colțuri rotunjite
                pygame.draw.rect(screen, color, rect, border_radius=10)  # Pătrat

            # Contur pentru fiecare lumină
            pygame.draw.rect(screen, GRID_LINE_COLOR, rect, 2, border_radius=10)


def draw_lights_button(mouse_pos):
    # Coordonatele și dimensiunile butonului
    button_width = 200
    button_height = 80
    button_x = (screen_width - button_width) // 2
    button_y = screen_height // 2 - button_height - 30

    button_rect = pygame.Rect(button_x, button_y, button_width, button_height)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR

    # Desenăm butonul cu colțuri rotunjite
    pygame.draw.rect(screen, button_color, button_rect, border_radius=20)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 3, border_radius=20)

    # Adăugăm un efect de glow animat
    if button_rect.collidepoint(mouse_pos):
        glow_surface = pygame.Surface((button_width + 40, button_height + 40), pygame.SRCALPHA)
        pygame.draw.ellipse(glow_surface, (*GLOW_COLOR, 100), glow_surface.get_rect(), border_radius=20)
        screen.blit(glow_surface, (button_x - 20, button_y - 20))

    # Desenăm textul "LigHts" pe buton
    title_font = pygame.font.Font(None, 50)
    title_text = "LigHts"
    text_color = (255, 255, 255) if not button_rect.collidepoint(mouse_pos) else GLOW_COLOR
    title_surface = title_font.render(title_text, True, text_color)
    title_rect = title_surface.get_rect(center=button_rect.center)

    # Adăugăm un font animat
    scale_factor = 1.05 + (0.05 * math.sin(pygame.time.get_ticks() * 0.005))
    scaled_surface = pygame.transform.scale(title_surface,
                                            (int(title_rect.width * scale_factor), int(title_rect.height * scale_factor)))
    scaled_rect = scaled_surface.get_rect(center=button_rect.center)
    screen.blit(scaled_surface, scaled_rect)

    return button_rect



def draw_confetti():
    confetti = [{"pos": [random.randint(0, screen_width), -10], "color": random.choice(colors)} for _ in range(30)]
    for particle in confetti:
        particle["pos"][1] += random.randint(1, 5)
        pygame.draw.circle(screen, particle["color"], particle["pos"], 3)

def draw_parallax_background(offset):
    for i in range(screen_height):
        r = int(15 + math.sin(i * 0.02 + offset * 0.001) * 50)
        g = int(15 + math.cos(i * 0.03 + offset * 0.001) * 50)
        b = 80 + int(math.sin(i * 0.04 + offset * 0.001) * 50)
        color = (r, g, b)
        pygame.draw.line(screen, color, (0, i), (screen_width, i))




# Funcție pentru a comuta lumina și luminile adiacente
def toggle_light(lights, row, col, grid_size):
    lights[row][col] = 1 - lights[row][col]
    if row > 0:
        lights[row - 1][col] = 1 - lights[row - 1][col]
    if row < grid_size - 1:
        lights[row + 1][col] = 1 - lights[row + 1][col]
    if col > 0:
        lights[row][col - 1] = 1 - lights[row][col - 1]
    if col < grid_size - 1:
        lights[row][col + 1] = 1 - lights[row][col + 1]

# Funcție pentru a verifica dacă toate luminile sunt aprinse
def check_win(lights, grid_size):
    return all(lights[row][col] == 1 for row in range(grid_size) for col in range(grid_size))

def draw_glow_effect(screen, pos, radius, color):
    glow_surface = pygame.Surface((radius * 2, radius * 2), pygame.SRCALPHA)
    pygame.draw.circle(glow_surface, (*color, 50), (radius, radius), radius)
    screen.blit(glow_surface, (pos[0] - radius, pos[1] - radius))


def draw_help_button(mouse_pos):
    global hints_left
    font = pygame.font.Font(None, 24)
    button_width, button_height = 80, 40
    button_x = screen_width - 90
    button_y = screen_height - 50
    button_rect = pygame.Rect(button_x, button_y, button_width, button_height)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=10)
    text = font.render(f"Hints: {hints_left}", True, TEXT_COLOR)
    text_rect = text.get_rect(center=button_rect.center)
    screen.blit(text, text_rect)
    return button_rect


def draw_reset_button(mouse_pos):
    font = pygame.font.Font(None, 30)
    button_rect = pygame.Rect(screen_width - 100, screen_height - 50, 80, 40)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=10)
    text = font.render("Reset", True, TEXT_COLOR)
    screen.blit(text, (button_rect.x + 10, button_rect.y + 10))
    return button_rect



def fade_out():
    fade_surface = pygame.Surface((screen_width, screen_height))
    for alpha in range(0, 255, 5):
        fade_surface.set_alpha(alpha)
        screen.blit(fade_surface, (0, 0))
        pygame.display.update()
        pygame.time.delay(10)


# Funcție pentru a rezolva puzzle-ul
def solve_puzzle(lights, grid_size):
    global solution_steps
    solution_steps = []
    temp_lights = [row[:] for row in lights]
    for row in range(grid_size):
        for col in range(grid_size):
            if temp_lights[row][col] == 0:
                solution_steps.append((row, col))
                toggle_light(temp_lights, row, col, grid_size)
    global current_step_index
    current_step_index = 0

def purchase_item(item_name):
    global emeralds, hints_left

    if item_name == "Golden Hint":
        price = 10  # Costul în smaralde
        if emeralds >= price:
            emeralds -= price
            hints_left += 10  # Adaugă 10 hint-uri în loc de 1
            print(f"Ai cumpărat un Golden Hint! Acum ai {hints_left} hint-uri.")
        else:
            print("Nu ai suficiente smaralde pentru a cumpăra acest articol.")


def give_hint(lights, grid_size):
    global current_step_index, solution_steps

    # Dacă nu există pași calculați, rezolvă puzzle-ul pentru a genera soluția
    if not solution_steps or current_step_index >= len(solution_steps):
        solve_puzzle(lights, grid_size)

    # Aplică pașii din soluție
    if current_step_index < len(solution_steps):
        row, col = solution_steps[current_step_index]
        toggle_light(lights, row, col, grid_size)
        current_step_index += 1


def reset_level():
    global lights, moves, solution_steps, current_step_index
    grid_size = levels[current_level]["grid_size"] if current_level < len(levels) else 12
    lights = create_lights(grid_size)
    moves = 0
    solution_steps = []
    current_step_index = 0

def draw_reset_button(mouse_pos):
    font = pygame.font.Font(None, 24)  # Font mic
    button_width, button_height = 80, 40
    button_x = (screen_width // 2) - 70  # Centrat pe mijlocul ecranului
    button_y = screen_height - 50  # Aliniat cu celelalte butoane

    button_rect = pygame.Rect(button_x, button_y, button_width, button_height)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR

    # Desenează butonul
    pygame.draw.rect(screen, button_color, button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=10)

    # Textul butonului
    text = font.render("Reset", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    text_rect = text.get_rect(center=button_rect.center)
    screen.blit(text, text_rect)

    return button_rect


# Particule pentru fundal
particles = [{"pos": [random.randint(0, screen_width), random.randint(0, screen_height)],
              "speed": [random.uniform(-0.5, 0.5), random.uniform(-0.5, 0.5)]} for _ in range(50)]


def draw_home_screen(mouse_pos):
    screen.fill(BACKGROUND_COLOR)  # Înlocuire BLACK cu BACKGROUND_COLOR
    mute_button_rect = draw_mute_button(mouse_pos)

    # Particule de fundal
    for particle in particles:
        pygame.draw.circle(screen, TEXT_COLOR, particle["pos"], 2)  # Înlocuire (255, 255, 255) cu TEXT_COLOR
        particle["pos"][0] += particle["speed"][0]
        particle["pos"][1] += particle["speed"][1]
        if particle["pos"][0] < 0 or particle["pos"][0] > screen_width:
            particle["speed"][0] *= -1
        if particle["pos"][1] < 0 or particle["pos"][1] > screen_height:
            particle["speed"][1] *= -1

    # Titlu cu efect de sclipire personalizat pentru a se potrivi cu paleta jocului
    title_font = pygame.font.Font(None, 80)
    title_text = "LigHts"
    tick = pygame.time.get_ticks() * 0.005
    glow_intensity = abs(int(80 * math.sin(tick)) + 175)
    glow_color = (glow_intensity, 100, 200)  # Culoare variabilă care se combină cu paleta jocului
    title_surface = title_font.render(title_text, True, glow_color)
    title_rect = title_surface.get_rect(center=(screen_width // 2, screen_height // 4))
    screen.blit(title_surface, title_rect)

    # Butonul "Shop" cu simbol
    button_font = pygame.font.SysFont('Segoe UI Emoji', 40)  # Font pentru simboluri emoji
    shop_button_rect = pygame.Rect(screen_width // 2 - 60, screen_height // 2 + 60, 120, 60)
    draw_circular_button(shop_button_rect, "🛒", mouse_pos, button_font)  # Emoji-ul de cărucior în loc de text

    # Butonul "Buy Hints" în colțul dreapta sus
    buy_hints_button_rect = pygame.Rect(screen_width - 100, 10, 100, 35)

    # Redare buton "Buy Hints" cu simbol emoji
    small_font = pygame.font.SysFont('Segoe UI Emoji', 24)
    button_color = BUTTON_HOVER_COLOR if buy_hints_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, buy_hints_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, buy_hints_button_rect, 2, border_radius=10)

    # Text pentru butonul "Buy Hints" cu icon
    buy_icon = "💰"
    text_surface = small_font.render(f"{buy_icon} Buy", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    text_rect = text_surface.get_rect(center=buy_hints_button_rect.center)
    screen.blit(text_surface, text_rect)

    # Returnează doar cele trei butoane așteptate
    return shop_button_rect, mute_button_rect, buy_hints_button_rect


def draw_shop_screen(mouse_pos):
    """Shop modernizat cu titlu și smaralde într-un chenar centralizat, fără scroll."""
    # Gradient de fundal pentru Shop
    for i in range(screen_height):
        r = int(40 + math.sin(i * 0.03) * 30)
        g = int(60 + math.cos(i * 0.03) * 30)
        b = int(100 + math.sin(i * 0.04) * 40)
        color = (r, g, b)
        pygame.draw.line(screen, color, (0, i), (screen_width, i))

    # Titlu și Emeralds într-un chenar elegant
    title_width, title_height = 350, 120
    title_rect = pygame.Rect(
        (screen_width - title_width) // 2,
        40,  # Fixat la o poziție fără scroll
        title_width,
        title_height,
    )

    # Fundal pentru chenar cu o culoare nouă (violet închis)
    pygame.draw.rect(screen, (50, 40, 80), title_rect, border_radius=20)

    # Marginea chenarului cu o culoare vie (gradient auriu)
    border_color = (255, 223, 0)  # Aurie
    pygame.draw.rect(screen, border_color, title_rect, 4, border_radius=20)

    # Efect de glow animat (albastru strălucitor)
    glow_brightness = int(150 + 100 * math.sin(pygame.time.get_ticks() * 0.005))
    glow_color = (50, glow_brightness, 255)  # Albastru strălucitor
    glow_surface = pygame.Surface((title_width + 20, title_height + 20), pygame.SRCALPHA)
    pygame.draw.rect(glow_surface, (*glow_color, 80), glow_surface.get_rect(), border_radius=20)
    screen.blit(glow_surface, title_rect.topleft - pygame.Vector2(10, 10))

    # Textul pentru "Welcome to the Shop"
    title_font = pygame.font.Font(None, 40)
    title_text = "Welcome to the Shop"
    title_surface = title_font.render(title_text, True, TEXT_COLOR)
    title_text_rect = title_surface.get_rect(midtop=(title_rect.centerx, title_rect.y + 15))
    screen.blit(title_surface, title_text_rect)

    # Text pentru Emeralds
    emerald_text = f"Emeralds: {emeralds}"
    emerald_surface = emoji_font.render(emerald_text, True, TEXT_COLOR)
    emerald_text_rect = emerald_surface.get_rect(midtop=(title_rect.centerx, title_rect.y + 60))
    screen.blit(emerald_surface, emerald_text_rect)

    # Elemente de vânzare (carduri atractive)
    items_for_sale = [
        {"name": "Golden Hint", "price": 10, "emoji": "✨", "description": ""},
        {"name": "Mystery Box", "price": 30, "emoji": "🎁", "description": ""},
    ]

    # Configurație pentru carduri
    card_width = screen_width - 80
    card_height = 100
    card_x = 40
    card_spacing = 20
    item_start_y = title_rect.bottom + 20  # Începem sub titlu

    # Afișăm fiecare card
    item_rects = []
    for i, item in enumerate(items_for_sale):
        card_y = item_start_y + i * (card_height + card_spacing)
        item_rect = pygame.Rect(card_x, card_y, card_width, card_height)

        # Fundal și margine pentru card
        shadow_rect = item_rect.move(5, 5)
        pygame.draw.rect(screen, (50, 50, 50), shadow_rect, border_radius=12)  # Umbra
        button_color = BUTTON_HOVER_COLOR if item_rect.collidepoint(mouse_pos) else BUTTON_COLOR
        pygame.draw.rect(screen, button_color, item_rect, border_radius=15)
        pygame.draw.rect(screen, BUTTON_BORDER_COLOR, item_rect, 2, border_radius=15)

        # Emoji-ul obiectului
        emoji_surface = emoji_font.render(item["emoji"], True, TEXT_COLOR)
        screen.blit(emoji_surface, (item_rect.x + 15, item_rect.y + 25))

        # Numele obiectului
        name_font = pygame.font.Font(None, 36)
        name_surface = name_font.render(item["name"], True, TEXT_COLOR)
        screen.blit(name_surface, (item_rect.x + 80, item_rect.y + 20))

        # Descrierea obiectului
        desc_font = pygame.font.Font(None, 24)
        desc_surface = desc_font.render(item["description"], True, TEXT_COLOR)
        screen.blit(desc_surface, (item_rect.x + 80, item_rect.y + 60))

        # Prețul obiectului
        price_surface = emoji_font.render(f"{item['price']}", True, TEXT_COLOR)
        screen.blit(price_surface, (item_rect.right - 80, item_rect.y + 35))

        # Salvează rect-ul pentru interacțiuni
        item_rects.append((item_rect, item))

    # Buton "Back"
    back_button_rect = pygame.Rect(10, screen_height - 60, 100, 40)
    back_button_color = BUTTON_HOVER_COLOR if back_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, back_button_color, back_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, back_button_rect, 2, border_radius=10)
    back_text = emoji_font.render("Back", True, TEXT_COLOR)
    screen.blit(back_text, (back_button_rect.x + 20, back_button_rect.y + 10))

    return item_rects, back_button_rect


def draw_emerald_balance():
    emerald_text = f"💎 Emeralds: {emeralds}"
    emerald_surface = emoji_font.render(emerald_text, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    screen.blit(emerald_surface, (10, 10))  # Adjustează poziția dacă este necesar

def draw_levels_button(mouse_pos):
    button_width = 200
    button_height = 60
    button_x = (screen_width - button_width) // 2
    button_y = screen_height - button_height - 60
    button_rect = pygame.Rect(button_x, button_y, button_width, button_height)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, button_rect, border_radius=15)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=15)
    if button_rect.collidepoint(mouse_pos):
        glow_surface = pygame.Surface((button_width + 20, button_height + 20), pygame.SRCALPHA)
        pygame.draw.ellipse(glow_surface, (*GLOW_COLOR, 50), glow_surface.get_rect())
        screen.blit(glow_surface, (button_x - 10, button_y - 10))
    font = pygame.font.Font(None, 40)
    text_surface = font.render("Select Levels", True, TEXT_COLOR)
    text_rect = text_surface.get_rect(center=button_rect.center)
    screen.blit(text_surface, text_rect)
    return button_rect

def draw_levels_page(mouse_pos):
    """Draws the main level selection page with 10 level buttons arranged in 2 columns."""
    screen.fill(BACKGROUND_COLOR)  # Fundal
    font = pygame.font.Font(None, 36)
    buttons = []

    # Dimensiuni și spațiere pentru grilă
    button_width = 100
    button_height = 40
    button_spacing_x = 20
    button_spacing_y = 20

    # Calcularea poziției inițiale pentru centrare
    total_columns = 2
    total_rows = 5
    grid_width = (button_width * total_columns) + (button_spacing_x * (total_columns - 1))
    grid_height = (button_height * total_rows) + (button_spacing_y * (total_rows - 1))
    grid_start_x = (screen_width - grid_width) // 2
    grid_start_y = (screen_height - grid_height) // 2

    # Desenează butoanele pentru fiecare nivel
    for i in range(10):
        col = i % total_columns  # Coloana (0 sau 1)
        row = i // total_columns  # Rândul (0-4)
        x = grid_start_x + col * (button_width + button_spacing_x)
        y = grid_start_y + row * (button_height + button_spacing_y)

        # Creează un efect de umbră
        shadow_rect = pygame.Rect(x + 5, y + 5, button_width, button_height)
        pygame.draw.rect(screen, (50, 50, 50), shadow_rect, border_radius=5)  # Umbra butonului

        # Desenează butonul principal
        level_rect = pygame.Rect(x, y, button_width, button_height)
        button_color = BUTTON_HOVER_COLOR if level_rect.collidepoint(mouse_pos) else BUTTON_COLOR
        pygame.draw.rect(screen, button_color, level_rect, border_radius=5)

        # Adaugă margini mai întunecate pentru un efect de adâncime
        pygame.draw.rect(screen, (30, 30, 30), level_rect, 2, border_radius=5)

        # Textul butonului
        level_text = font.render(f"Level {i + 1}", True, TEXT_COLOR)
        text_rect = level_text.get_rect(center=level_rect.center)
        screen.blit(level_text, text_rect)

        buttons.append(level_rect)

    # Buton "Back" cu umbră
    back_button_rect = pygame.Rect(10, 10, 100, 40)
    back_shadow_rect = pygame.Rect(back_button_rect.x + 5, back_button_rect.y + 5, back_button_rect.width, back_button_rect.height)
    pygame.draw.rect(screen, (50, 50, 50), back_shadow_rect, border_radius=10)  # Umbra pentru butonul "Back"
    back_button_color = BUTTON_HOVER_COLOR if back_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, back_button_color, back_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, back_button_rect, 2, border_radius=10)

    # Text pentru butonul "Back"
    back_text = font.render("Back", True, TEXT_COLOR)
    screen.blit(back_text, (back_button_rect.x + 20, back_button_rect.y + 10))

    return buttons, back_button_rect


def draw_daily_wheel_button(mouse_pos, spins_left):
    """Draws a circular Daily Wheel button with the 🎡 icon and displays spins left if available."""
    font = pygame.font.SysFont('Segoe UI Emoji', 36)  # Font for the icon
    small_font = pygame.font.Font(None, 24)  # Smaller font for the spin count
    button_radius = 40  # Radius for the circular button
    button_x = screen_width - button_radius - 20  # Positioned on the right side with padding
    button_y = screen_height // 2  # Vertically centered

    # Create the button as a circle
    button_center = (button_x, button_y)
    button_rect = pygame.Rect(button_x - button_radius, button_y - button_radius, button_radius * 2, button_radius * 2)

    # Hover effect
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR

    # Draw the circular button
    pygame.draw.circle(screen, button_color, button_center, button_radius)
    pygame.draw.circle(screen, BUTTON_BORDER_COLOR, button_center, button_radius, 2)  # Border

    # Add the 🎡 icon in the center
    icon = "🎡"
    icon_surface = font.render(icon, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    icon_rect = icon_surface.get_rect(center=button_center)
    screen.blit(icon_surface, icon_rect)

    # Display spins left as a red circle with text if spins are available
    if spins_left > 0:
        badge_radius = 15  # Radius of the red circle
        badge_center = (button_x + button_radius - 10, button_y - button_radius + 10)  # Position near top-right corner
        pygame.draw.circle(screen, (255, 0, 0), badge_center, badge_radius)  # Red circle

        # Draw the number of spins left
        spins_text = small_font.render(str(spins_left), True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
        spins_text_rect = spins_text.get_rect(center=badge_center)
        screen.blit(spins_text, spins_text_rect)

    return button_rect


def draw_wheel_screen(angle, spins_left):
    """
    Desenează roata norocoasă cu triunghiuri cu contur portocaliu mai gros.
    :param angle: Unghiul de rotire al roții.
    :param spins_left: Numărul de spinuri rămase.
    """
    center = (screen_width // 2, screen_height // 2 - 50)  # Ridicăm roata mai sus pentru a face loc mesei
    radius = 150
    segment_colors = [BUTTON_COLOR, TEXT_COLOR, (0, 0, 255), BACKGROUND_COLOR,
                      (0, 255, 0), (128, 0, 128), BUTTON_HOVER_COLOR, (255, 20, 147)]
    rewards = ["5 💎", "5 Hints", "Mystery Box", "10 Hints",
               "20 💎", "3 Spins", "10 💎", "15 Hints"]

    num_segments = len(segment_colors)
    segment_angle = 360 / num_segments

    # Desenează interiorul roții
    pygame.draw.circle(screen, BACKGROUND_COLOR, center, radius)  # Fundalul roții

    # Desenează segmentele roții
    for i, color in enumerate(segment_colors):
        start_angle = math.radians(i * segment_angle) + angle
        end_angle = math.radians((i + 1) * segment_angle) + angle
        points = [center]
        for theta in (start_angle, end_angle):
            x = center[0] + radius * math.cos(theta)
            y = center[1] + radius * math.sin(theta)
            points.append((x, y))
        pygame.draw.polygon(screen, color, points)  # Triunghiul colorat
        pygame.draw.polygon(screen, (255, 165, 0), points, 4)  # Contur portocaliu mai gros

    # Desenează săgeata
    arrow_points = [
        (center[0], center[1] - radius - 10),
        (center[0] - 15, center[1] - radius - 30),
        (center[0] + 15, center[1] - radius - 30)
    ]
    pygame.draw.polygon(screen, BUTTON_BORDER_COLOR, arrow_points)  # Săgeata galbenă

    # Contor de spinuri
    font = pygame.font.Font(None, 40)
    spins_text = f"Spins Left: {spins_left}"
    spins_surface = font.render(spins_text, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    screen.blit(spins_surface, (10, 10))  # Poziționează contorul în colțul stânga sus

    # Butonul "Spin"
    spin_button_rect = pygame.Rect(screen_width // 2 - 60, screen_height - 100, 120, 60)
    if final_reward:  # Dacă există o recompensă nerevendicată, dezactivează butonul
        button_color = (100, 100, 100)  # Gri închis pentru buton dezactivat
        spin_text_color = (150, 150, 150)  # Gri deschis pentru text
    else:
        button_color = BUTTON_HOVER_COLOR if spin_button_rect.collidepoint(pygame.mouse.get_pos()) else BUTTON_COLOR
        spin_text_color = TEXT_COLOR

    pygame.draw.rect(screen, button_color, spin_button_rect, border_radius=15)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, spin_button_rect, 2, border_radius=15)
    spin_text = font.render("Spin", True, spin_text_color)
    spin_text_rect = spin_text.get_rect(center=spin_button_rect.center)
    screen.blit(spin_text, spin_text_rect)

    return spin_button_rect, rewards

def draw_back_button(mouse_pos):
    font = pygame.font.Font(None, 24)
    button_width, button_height = 80, 40
    button_x = 10
    button_y = screen_height - 50
    button_rect = pygame.Rect(button_x, button_y, button_width, button_height)
    button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=10)
    text = font.render("Back", True, TEXT_COLOR)
    text_rect = text.get_rect(center=button_rect.center)
    screen.blit(text, text_rect)
    return button_rect


def spin_wheel_logic(rewards):
    """Alege un câștigător din lista de premii bazat pe segmentul selectat."""
    selected_index = random.randint(0, len(rewards) - 1)
    return rewards[selected_index]

def draw_reward_message(reward):
    """Desenează chenarul cu premiul câștigat."""
    reward_rect = pygame.Rect(50, screen_height // 2 - 60, screen_width - 100, 120)
    pygame.draw.rect(screen, BUTTON_COLOR, reward_rect, border_radius=20)  # Fundal chenar
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, reward_rect, 2, border_radius=20)  # Margine

    # Utilizează un font compatibil cu emoji-uri
    try:
        emoji_font = pygame.font.SysFont("Segoe UI Emoji", 40)  # Font care suportă emoji-uri
    except:
        emoji_font = pygame.font.Font(None, 40)  # Fallback la font implicit

    reward_text = f"You won: {reward}!"
    reward_surface = emoji_font.render(reward_text, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR

    # Ajustează dimensiunea textului pentru a încăpea în chenar
    font_size = 40  # Dimensiunea inițială a fontului
    while reward_surface.get_width() > reward_rect.width - 20:  # Margine de 10px pe fiecare parte
        font_size -= 2
        emoji_font = pygame.font.SysFont("Segoe UI Emoji", font_size)
        reward_surface = emoji_font.render(reward_text, True, TEXT_COLOR)

    reward_rect_text = reward_surface.get_rect(center=reward_rect.center)
    screen.blit(reward_surface, reward_rect_text)

    # Buton Claim
    claim_button = pygame.Rect(screen_width // 2 - 50, reward_rect.bottom + 10, 100, 40)
    pygame.draw.rect(screen, BUTTON_COLOR, claim_button, border_radius=10)  # Fundal buton
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, claim_button, 2, border_radius=10)  # Margine
    claim_text = emoji_font.render("Claim", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    claim_text_rect = claim_text.get_rect(center=claim_button.center)
    screen.blit(claim_text, claim_text_rect)

    return claim_button

def draw_sub_levels_page(level, mouse_pos):
    """Desenează pagina de sub-nivele pentru un nivel specific, aranjate în grilă cu efecte de umbră."""
    screen.fill(BACKGROUND_COLOR)  # Fundal

    # Font compatibil cu emoji-uri, cu dimensiune mai mică
    try:
        font = pygame.font.SysFont('Segoe UI Emoji', 30, bold=True)  # Font bold și dimensiune mai mică
    except:
        font = pygame.font.Font(None, 30)  # Fallback la font standard

    buttons = []

    # Numerotarea subnivelurilor (ex: pentru level=4, va afișa subnivelurile 41-50)
    start_sub_level = (level - 1) * 10 + 1
    end_sub_level = start_sub_level + 10

    # Dimensiuni și spațiere pentru grilă
    button_width = 120
    button_height = 40
    button_spacing_x = 20
    button_spacing_y = 20

    # Calcularea poziției inițiale pentru centrare
    grid_start_x = (screen_width - (button_width * 2 + button_spacing_x)) // 2
    grid_start_y = 100

    # Desenează butoanele pentru fiecare sub-nivel
    for i, sub_level_num in enumerate(range(start_sub_level, end_sub_level)):
        col = i % 2  # Coloana (0 sau 1)
        row = i // 2  # Rândul (0-4)
        x = grid_start_x + col * (button_width + button_spacing_x)
        y = grid_start_y + row * (button_height + button_spacing_y)

        # Creează un efect de umbră
        shadow_rect = pygame.Rect(x + 5, y + 5, button_width, button_height)  # Umbra este ușor deplasată
        pygame.draw.rect(screen, (50, 50, 50), shadow_rect, border_radius=5)  # Culoarea umbrei

        # Desenează butonul principal
        sub_level_rect = pygame.Rect(x, y, button_width, button_height)
        button_color = BUTTON_HOVER_COLOR if sub_level_rect.collidepoint(mouse_pos) else BUTTON_COLOR
        pygame.draw.rect(screen, button_color, sub_level_rect, border_radius=5)

        # Adaugă margini mai întunecate pentru un efect de adâncime
        pygame.draw.rect(screen, (30, 30, 30), sub_level_rect, 2, border_radius=5)

        # Textul butonului (mai mic, dar îngroșat)
        sub_level_text = font.render(f"Level {sub_level_num}", True, TEXT_COLOR)
        text_rect = sub_level_text.get_rect(center=sub_level_rect.center)
        screen.blit(sub_level_text, text_rect)

        buttons.append(sub_level_rect)

    # Buton "Back" în colțul din stânga sus
    back_button_rect = pygame.Rect(10, 10, 100, 40)
    back_shadow_rect = pygame.Rect(back_button_rect.x + 5, back_button_rect.y + 5, back_button_rect.width, back_button_rect.height)
    pygame.draw.rect(screen, (50, 50, 50), back_shadow_rect, border_radius=10)  # Umbra pentru butonul "Back"
    back_button_color = BUTTON_HOVER_COLOR if back_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, back_button_color, back_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, back_button_rect, 2, border_radius=10)

    # Text pentru butonul "Back" (mai mic, dar îngroșat)
    back_text = font.render("Back", True, TEXT_COLOR)
    back_text_rect = back_text.get_rect(center=back_button_rect.center)
    screen.blit(back_text, back_text_rect)

    return buttons, back_button_rect


def draw_game_screen(mouse_pos):
    # Fundal și grila de lumini
    draw_background()
    grid_size = 12 if current_level == 10 else 2 + current_level
    draw_lights(lights, grid_size)
    draw_score_and_level(moves, current_sub_level, total_sub_levels)

    # Desenează butoanele
    back_button_rect = draw_back_button(mouse_pos)  # Butonul Back
    reset_button_rect = draw_reset_button(mouse_pos)  # Butonul Reset
    help_button_rect = draw_help_button(mouse_pos)  # Butonul Hints

    return help_button_rect, reset_button_rect, back_button_rect


def draw_purchase_screen(mouse_pos):
    # Enhanced gradient background
    for i in range(screen_height):
        r = int(80 + math.sin(i * 0.03) * 60)
        g = int(120 + math.cos(i * 0.03) * 60)
        b = int(160 + math.sin(i * 0.04) * 60)
        color = (r, g, b)
        pygame.draw.line(screen, color, (0, i), (screen_width, i))

    # Title with larger font and glow effect
    title_font = pygame.font.Font(None, 50)
    title_surface = title_font.render("Hint Shop", True, TEXT_COLOR)  # Înlocuire (255, 255, 255) cu TEXT_COLOR
    title_glow = pygame.font.Font(None, 52).render("Hint Shop", True, BUTTON_HOVER_COLOR)  # Glow galben
    title_rect = title_surface.get_rect(center=(screen_width // 2, 100))
    screen.blit(title_glow, title_rect.move(1, 1))
    screen.blit(title_surface, title_rect)

    # Icon for hints or coins
    hint_icon = pygame.font.SysFont('Segoe UI Emoji', 30).render("💡", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR

    # Display packages as cards with shadow and icon
    packages = [
        ("100 Hints - €0.99", 100),
        ("300 Hints - €1.99", 300),
        ("500 Hints - €2.99", 500),
        ("700 Hints - €3.99", 700),
        ("900 Hints - €4.99", 900)
    ]

    button_rects = []
    for i, (text, hints) in enumerate(packages):
        button_rect = pygame.Rect(screen_width // 2 - 120, 180 + i * 70, 240, 60)

        # Draw shadow for card
        shadow_rect = button_rect.move(4, 4)
        pygame.draw.rect(screen, BACKGROUND_COLOR, shadow_rect, border_radius=15)  # Umbra

        # Draw card with gradient and rounded borders
        button_color = BUTTON_HOVER_COLOR if button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
        pygame.draw.rect(screen, button_color, button_rect, border_radius=15)
        pygame.draw.rect(screen, BUTTON_BORDER_COLOR, button_rect, 2, border_radius=15)

        # Icon placement on card
        screen.blit(hint_icon, (button_rect.x + 10, button_rect.y + 10))

        # Text rendering for hints and price
        font = pygame.font.Font(None, 32)
        text_surface = font.render(text, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
        text_rect = text_surface.get_rect(midleft=(button_rect.x + 50, button_rect.centery))
        screen.blit(text_surface, text_rect)

        button_rects.append(button_rect)

    # Button for watching an ad for 10 free hints
    ad_button_rect = pygame.Rect(screen_width // 2 - 120, 180 + len(packages) * 70, 240, 60)
    ad_button_color = BUTTON_HOVER_COLOR if ad_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, ad_button_color, ad_button_rect, border_radius=15)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, ad_button_rect, 2, border_radius=15)
    ad_text_surface = font.render("Watch Ad for 10 Hints", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    ad_text_rect = ad_text_surface.get_rect(center=ad_button_rect.center)
    screen.blit(ad_text_surface, ad_text_rect)

    # Back button with arrow icon at top-left corner
    back_button_rect = pygame.Rect(10, 10, 100, 40)
    button_color = BUTTON_HOVER_COLOR if back_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, back_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, back_button_rect, 2, border_radius=10)
    back_text = font.render("  Back", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    screen.blit(back_text, (back_button_rect.x + 10, back_button_rect.y + 5))

    return button_rects, back_button_rect, ad_button_rect

def draw_circular_button(rect, text, mouse_pos, font):
    """Desenează un buton circular cu text și efect de glow."""
    button_color = BUTTON_HOVER_COLOR if rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.circle(screen, button_color, rect.center, rect.width // 2)
    pygame.draw.circle(screen, BUTTON_BORDER_COLOR, rect.center, rect.width // 2, 3)

    # Efect de glow
    text_color = GLOW_COLOR if rect.collidepoint(mouse_pos) else TEXT_COLOR
    glow_surface = font.render(text, True, text_color)

    text_rect = glow_surface.get_rect(center=rect.center)
    screen.blit(glow_surface, text_rect)

def draw_win_message(level, emeralds_won, mouse_pos):
    """
    Afișează mesajul de câștig cu bordură și textul "You Win!".
    :param level: Nivelul curent.
    :param emeralds_won: Numărul de smaralde câștigate.
    :param mouse_pos: Poziția mouse-ului.
    :return: Rect pentru butonul Claim.
    """
    # Fundal semi-transparent cu bordură
    overlay_rect = pygame.Rect(50, screen_height // 2 - 120, screen_width - 100, 240)
    pygame.draw.rect(screen, (135, 206, 250), overlay_rect, border_radius=20)  # Fundal albastru deschis
    pygame.draw.rect(screen, (0, 150, 136), overlay_rect, 8, border_radius=20)  # Bordură verde teal vibrant

    # Textul "You Win!"
    win_text = "You Win!"
    win_font = pygame.font.Font(None, 72)
    outline_color = (0, 0, 0)  # Negru pentru contur

    # Desenează conturul textului
    for dx, dy in [(-2, 0), (2, 0), (0, -2), (0, 2), (-2, -2), (-2, 2), (2, -2), (2, 2)]:
        outline_surface = win_font.render(win_text, True, outline_color)
        outline_rect = outline_surface.get_rect(center=(screen_width // 2 + dx, overlay_rect.y + 60 + dy))
        screen.blit(outline_surface, outline_rect)

    # Text principal
    win_surface = win_font.render(win_text, True, (255, 255, 255))  # Alb pentru textul principal
    win_rect = win_surface.get_rect(center=(screen_width // 2, overlay_rect.y + 60))
    screen.blit(win_surface, win_rect)

    # Text pentru premiul în smaralde
    emerald_font = pygame.font.SysFont('Segoe UI Emoji', 36)
    emerald_text = f"💎 +{emeralds_won} Emeralds"
    emerald_surface = emerald_font.render(emerald_text, True, (255, 255, 255))
    emerald_rect = emerald_surface.get_rect(center=(screen_width // 2, overlay_rect.y + 130))
    screen.blit(emerald_surface, emerald_rect)

    # Buton "Claim"
    claim_button_rect = pygame.Rect(screen_width // 2 - 70, overlay_rect.bottom - 70, 140, 50)
    button_color = (0, 200, 150) if claim_button_rect.collidepoint(mouse_pos) else (0, 150, 136)
    pygame.draw.rect(screen, button_color, claim_button_rect, border_radius=12)
    pygame.draw.rect(screen, (0, 100, 90), claim_button_rect, 2, border_radius=12)  # Bordură mai închisă

    claim_text = emerald_font.render("Claim", True, (255, 255, 255))
    claim_text_rect = claim_text.get_rect(center=claim_button_rect.center)
    screen.blit(claim_text, claim_text_rect)

    return claim_button_rect

def draw_game_screen(mouse_pos):
    """Desenează ecranul de joc, inclusiv butoanele Help, Back, și Reset, și afișarea mutărilor și nivelului curent."""

    # Fundal și grila de lumini
    draw_background()
    grid_size = 12 if current_level == 10 else 2 + current_level
    draw_lights(lights, grid_size)
    draw_score_and_level(moves, current_level, 10)

    # Coordonate pentru butoane
    help_button_rect = draw_help_button(mouse_pos)
    reset_button_rect = draw_reset_button(mouse_pos)

    # Poziționăm butonul Back în stânga butonului Help
    back_button_width = 80
    back_button_height = 40
    back_button_x = help_button_rect.x - back_button_width - 10  # Plasează butonul Back în stânga butonului Help
    back_button_y = help_button_rect.y

    # Desenăm butonul Back
    back_button_rect = pygame.Rect(back_button_x, back_button_y, back_button_width, back_button_height)
    button_color = BUTTON_HOVER_COLOR if back_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
    pygame.draw.rect(screen, button_color, back_button_rect, border_radius=10)
    pygame.draw.rect(screen, BUTTON_BORDER_COLOR, back_button_rect, 2, border_radius=10)

    # Desenăm textul "Back" pe buton
    font = pygame.font.Font(None, 30)
    back_text = font.render("Back", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    screen.blit(back_text, (back_button_rect.x + 15, back_button_rect.y + 10))

    return help_button_rect, reset_button_rect, back_button_rect

def initialize_game(level, sub_level):
    """Configures the game based on the main level and sublevel."""
    global current_level, current_sub_level, lights, moves

    # Setează nivelul și subnivelul curent
    current_level = level
    current_sub_level = sub_level

    # Fixăm dimensiunea gridului la 12x12 pentru nivelul 10
    grid_size = levels[current_level - 1]["grid_size"] if current_level <= len(levels) else 12

    # Inițializează luminile cu dimensiunea calculată a gridului
    lights = create_lights(grid_size)

    # Reset move count and solution steps
    moves = 0
    reset_level()


def draw_score_and_level(moves, current_sub_level, total_sub_levels):
    """Displays the current moves and sublevel progress on the screen."""
    font = pygame.font.Font(None, 30)
    moves_text = f"Moves: {moves}"
    level_text = f"Level: {current_sub_level}/{total_sub_levels}"  # Displays the sublevel out of 100
    moves_surface = font.render(moves_text, True, TEXT_COLOR)
    level_surface = font.render(level_text, True, TEXT_COLOR)
    screen.blit(level_surface, (10, screen_height - 80))
    screen.blit(moves_surface, (10, screen_height - 50))


def draw_mystery_box(mouse_pos, rewards_count, rewards=None, is_opened=False, reveal_time=1000):
    """
    Desenează Mystery Box-ul cu afișarea recompenselor una câte una.
    :param mouse_pos: Poziția mouse-ului.
    :param rewards_count: Numărul total de recompense.
    :param rewards: Lista recompenselor.
    :param is_opened: Dacă Mystery Box-ul este deschis.
    :param reveal_time: Timpul în milisecunde pentru a dezvălui fiecare cadou.
    """
    global chest_open_image, chest_closed_image, revealed_rewards, last_reveal_time

    # Dimensiuni și poziția Mystery Box
    box_width, box_height = 100, 100
    box_x = screen_width // 2 - box_width // 2
    box_y = screen_height // 2 - box_height // 2

    # Verificăm starea cufărului și alegem imaginea potrivită
    chest_image = chest_open_image if is_opened else chest_closed_image

    # Desenează cufărul (închis sau deschis)
    chest_rect = pygame.Rect(box_x, box_y, box_width, box_height)
    screen.blit(chest_image, (box_x, box_y))

    # Butonul "Open" (vizibil doar dacă cufărul este închis)
    open_button_rect = None
    if not is_opened:
        open_button_rect = pygame.Rect(screen_width // 2 - 50, screen_height // 2 + 60, 100, 40)
        button_color = BUTTON_HOVER_COLOR if open_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
        pygame.draw.rect(screen, button_color, open_button_rect, border_radius=10)
        pygame.draw.rect(screen, BUTTON_BORDER_COLOR, open_button_rect, 2, border_radius=10)
        open_font = pygame.font.Font(None, 30)
        open_text = open_font.render("Open", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
        open_text_rect = open_text.get_rect(center=open_button_rect.center)
        screen.blit(open_text, open_text_rect)

    # Recompensele afișate (dacă cufărul este deschis)
    claim_button_rect = None
    if is_opened and rewards:
        reward_font = pygame.font.SysFont('Segoe UI Emoji', 30)
        reward_width, reward_height = 80, 80  # Dimensiunea fiecărui pătrat pentru recompense
        reward_spacing = 20  # Spațiere între pătrate
        columns = 4  # Numărul de recompense pe rând
        start_x = (screen_width - (reward_width + reward_spacing) * columns + reward_spacing) // 2
        start_y = box_y + box_height + 20

        # Dezvăluim recompensele una câte una
        current_time = pygame.time.get_ticks()
        if len(revealed_rewards) < len(rewards) and current_time - last_reveal_time >= reveal_time:
            revealed_rewards.append(rewards[len(revealed_rewards)])
            last_reveal_time = current_time

        for i, reward in enumerate(revealed_rewards):
            row = i // columns
            col = i % columns
            x = start_x + col * (reward_width + reward_spacing)
            y = start_y + row * (reward_height + reward_spacing)

            reward_rect = pygame.Rect(x, y, reward_width, reward_height)
            pygame.draw.rect(screen, BUTTON_COLOR, reward_rect, border_radius=10)
            pygame.draw.rect(screen, BUTTON_BORDER_COLOR, reward_rect, 2, border_radius=10)

            reward_text = reward_font.render(reward, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
            reward_text_rect = reward_text.get_rect(center=reward_rect.center)
            screen.blit(reward_text, reward_text_rect)

        # Buton "Claim" (vizibil doar după afișarea tuturor recompenselor)
        if len(revealed_rewards) == len(rewards):
            claim_button_rect = pygame.Rect(screen_width // 2 - 50, screen_height - 100, 100, 40)
            button_color = BUTTON_HOVER_COLOR if claim_button_rect.collidepoint(mouse_pos) else BUTTON_COLOR
            pygame.draw.rect(screen, button_color, claim_button_rect, border_radius=10)
            pygame.draw.rect(screen, BUTTON_BORDER_COLOR, claim_button_rect, 2, border_radius=10)
            claim_text = reward_font.render("Claim", True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
            claim_text_rect = claim_text.get_rect(center=claim_button_rect.center)
            screen.blit(claim_text, claim_text_rect)

    return chest_rect, open_button_rect, claim_button_rect


def process_mystery_box():
    """Generează recompense din Mystery Box cu șanse mai mici pentru premii bune."""
    reward_types = [
        ("💎", 1, 15),  # Emeralds: minim 5, maxim 50
        ("🔄", 1, 2),   # Spins: minim 1, maxim 5
        ("💡", 3, 15),  # Hints: minim 3, maxim 15
    ]
    reward_list = []

    num_rewards = random.randint(1, 4)  # 1-4 recompense
    for _ in range(num_rewards):
        reward_type = random.choices(
            reward_types,
            weights=[60, 30, 10],  # Șanse pentru fiecare tip (mai mari pentru 💎, mai mici pentru 💡)
        )[0]
        symbol, min_value, max_value = reward_type
        reward_value = random.randint(min_value, max_value)
        reward_list.append(f"{symbol} {reward_value}")

    return reward_list

def draw_emerald_counter(emeralds):
    """Draws the emerald counter between the mute and daily rewards buttons."""
    font = pygame.font.SysFont('Segoe UI Emoji', 30)  # Font pentru smaralde
    emerald_icon = "💎"  # Simbol pentru smarald
    counter_text = f"{emerald_icon} {emeralds}"  # Textul afișat

    # Poziționare între butonul de mute și cel de Daily Rewards
    counter_x = 60  # În dreapta butonului de mute
    counter_y = 10  # Aliniat vertical cu butonul de mute

    # Afișăm textul
    text_surface = font.render(counter_text, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    screen.blit(text_surface, (counter_x, counter_y))

def draw_mystery_box_rewards(rewards):
    """Desenează lista de recompense obținute din Mystery Box."""
    font = pygame.font.Font(None, 30)
    y_start = screen_height // 2 - 80

    for i, reward in enumerate(rewards):
        reward_text = font.render(reward, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
        screen.blit(reward_text, (screen_width // 2 - 100, y_start + i * 30))

def draw_shape_selection_screen():
    """Ecran pentru a selecta forma luminilor."""
    screen.fill(BACKGROUND_COLOR)  # Înlocuire BLACK cu BACKGROUND_COLOR
    font = pygame.font.Font(None, 36)
    shapes = ["circle", "square", "triangle"]
    buttons = []

    for i, shape in enumerate(shapes):
        rect = pygame.Rect(50, 100 + i * 60, 300, 50)
        pygame.draw.rect(screen, BUTTON_COLOR, rect, border_radius=10)
        pygame.draw.rect(screen, BUTTON_BORDER_COLOR, rect, 2, border_radius=10)

        text = font.render(shape.capitalize(), True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
        text_rect = text.get_rect(center=rect.center)
        screen.blit(text, text_rect)

        buttons.append((rect, shape))

    return buttons

def draw_emerald_counter(emeralds):
    """Draws the emerald counter between the mute and daily rewards buttons."""
    font = pygame.font.SysFont('Segoe UI Emoji', 30)  # Font pentru smaralde
    emerald_icon = "💎"  # Simbol pentru smarald
    counter_text = f"{emerald_icon} {emeralds}"  # Textul afișat

    # Poziționare între butonul de mute și cel de Daily Rewards
    counter_x = 60  # În dreapta butonului de mute
    counter_y = 10  # Aliniat vertical cu butonul de mute

    # Afișăm textul
    text_surface = font.render(counter_text, True, TEXT_COLOR)  # Înlocuire WHITE cu TEXT_COLOR
    screen.blit(text_surface, (counter_x, counter_y))




# Constante pentru animația roții
DECELERATION = 0.005  # Decelerare pentru animația roții (valori mai mici = animație mai lungă)
MAX_SPIN_SPEED = 4.0  # Viteza maximă inițială a roții

# Main game loop
running = True
game_state = "home"  # Starea inițială a jocului
scroll_offset = 0  # Offset pentru scrolling în shop
lights = create_lights(3)  # Grila inițială pentru lumini
current_level = 1
current_sub_level = 1
total_levels = 10
total_sub_levels = total_levels * 10  # 100 sub-nivele în total
current_level_page = None  # Pagina curentă pentru nivele
moves = 0
spin_angle = 0  # Unghiul pentru animația de rotire
spin_speed = 0  # Viteza de rotire a roții
is_spinning = False  # Control pentru animația de rotire
final_reward = None  # Ultimul premiu câștigat
daily_streak = 1  # Serie zilnică
last_claimed_date = None  # Data ultimei revendicări
spins_left = 3  # Numărul de spinuri disponibile
mystery_box_opened = False  # Dacă Mystery Box este deschis
mystery_box_rewards = []  # Lista recompenselor din Mystery Box
hints_left = 0  # Hint-uri disponibile
revealed_rewards = []  # Lista recompenselor dezvăluite progresiv
last_reveal_time = pygame.time.get_ticks()  # Timpul ultimei dezvăluiri a unei recompense
reveal_time = 500  # Timp între dezvăluirea fiecărei recompense (în milisecunde)
current_light_shape = "square"  # Forma implicită a luminilor

# Main game loop
while running:
    mouse_pos = pygame.mouse.get_pos()  # Poziția curentă a mouse-ului
    screen.fill(BACKGROUND_COLOR)  # Șterge ecranul cu o culoare de fundal definită

    # Gestionarea stării jocului
    if game_state == "home":
        shop_button_rect, mute_button_rect, buy_hints_button_rect = draw_home_screen(mouse_pos)
        levels_button_rect = draw_levels_button(mouse_pos)
        daily_wheel_button_rect = draw_daily_wheel_button(mouse_pos, spins_left)
        rewards_button_rect = draw_rewards_button(mouse_pos)

        # Afișează numărul de smaralde între mute și daily rewards
        draw_emerald_counter(emeralds)

    elif game_state == "wheel":
        if not is_spinning:  # Când intri în ecranul „roata norocului”
            pygame.mixer.music.set_volume(0.2)  # Reduce volumul muzicii de fundal

        back_button_rect = draw_back_button(mouse_pos)
        spin_button_rect, rewards = draw_wheel_screen(spin_angle, spins_left)

        if is_spinning:
            spin_speed = max(0, spin_speed - DECELERATION)
            spin_angle += spin_speed

            if spin_speed == 0 and is_spinning:
                final_reward = spin_wheel_logic(rewards)
                is_spinning = False

        elif final_reward:
            claim_button_rect = draw_reward_message(final_reward)

    elif game_state == "shop":
        item_rects, back_button_rect = draw_shop_screen(mouse_pos)

    elif game_state == "shape_selection":
        shape_buttons = draw_shape_selection_screen()
        for rect, shape in shape_buttons:
            if rect.collidepoint(mouse_pos) and event.type == pygame.MOUSEBUTTONDOWN:
                current_light_shape = shape  # Schimbă forma
                game_state = "home"  # Revine la ecranul principal

    elif game_state == "levels":
        if current_level_page is None:
            level_buttons, back_button_rect = draw_levels_page(mouse_pos)
        else:
            sub_level_buttons, back_button_rect = draw_sub_levels_page(current_level_page, mouse_pos)

    elif game_state == "game":
        help_button_rect, reset_button_rect, back_button_rect = draw_game_screen(mouse_pos)

    elif game_state == "purchase":
        button_rects, back_button_rect, ad_button_rect = draw_purchase_screen(mouse_pos)

    elif game_state == "rewards":
        reward_buttons, back_button_rect = draw_rewards_page(mouse_pos)

    elif game_state == "mystery_box":
        if not mystery_box_opened:  # Când intri în ecranul Mystery Box
            pygame.mixer.music.set_volume(0.3)  # Reduce volumul muzicii de fundal

        chest_rect, open_button_rect, claim_button_rect = draw_mystery_box(
            mouse_pos,
            len(mystery_box_rewards),
            mystery_box_rewards if mystery_box_opened else None,
            is_opened=mystery_box_opened
        )

        if mystery_box_opened and len(revealed_rewards) < len(mystery_box_rewards):
            current_time = pygame.time.get_ticks()
            if current_time - last_reveal_time >= reveal_time:
                revealed_rewards.append(mystery_box_rewards[len(revealed_rewards)])
                last_reveal_time = current_time

                # Redă sunetul pentru fiecare câștig dezvăluit
                coin_sound = pygame.mixer.Sound("coins-sound-effect-1-241818.mp3")
                coin_sound.play()

    # Evenimente
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        if event.type == pygame.MOUSEBUTTONDOWN:
            if game_state == "home":
                if shop_button_rect.collidepoint(event.pos):
                    game_state = "shop"
                elif mute_button_rect.collidepoint(event.pos):
                    toggle_music()
                elif buy_hints_button_rect.collidepoint(event.pos):
                    game_state = "purchase"
                elif levels_button_rect.collidepoint(event.pos):
                    game_state = "levels"
                    current_level_page = None
                elif daily_wheel_button_rect.collidepoint(event.pos):
                    game_state = "wheel"
                elif rewards_button_rect.collidepoint(event.pos):
                    game_state = "rewards"

            elif game_state == "wheel":
                if back_button_rect.collidepoint(event.pos):
                    pygame.mixer.music.set_volume(0.5)  # Resetează volumul muzicii la normal
                    game_state = "home"
                elif spin_button_rect.collidepoint(event.pos) and spins_left > 0 and not is_spinning and not final_reward:
                    is_spinning = True
                    spin_speed = random.uniform(3.0, MAX_SPIN_SPEED)
                    spins_left -= 1
                    spin_click_sound.play()  # Redă sunetul pentru „spin”
                elif final_reward and claim_button_rect.collidepoint(event.pos):
                    if "💎" in final_reward:
                        emeralds += int(final_reward.split(" ")[0])
                    elif "Hints" in final_reward:
                        hints_left += int(final_reward.split(" ")[0])
                    elif "Spins" in final_reward:
                        spins_left += int(final_reward.split(" ")[0])
                    elif "Mystery Box" in final_reward:
                        mystery_box_rewards = process_mystery_box()
                        mystery_box_opened = False
                        revealed_rewards = []
                        game_state = "mystery_box"
                    final_reward = None

            elif game_state == "shop":
                if back_button_rect.collidepoint(event.pos):
                    game_state = "home"
                else:
                    for item_rect, item in item_rects:
                        if item_rect.collidepoint(event.pos):
                            if emeralds >= item["price"]:
                                emeralds -= item["price"]
                                if item["name"] == "Custom Light Shape":
                                    game_state = "shape_selection"  # Trece la ecranul de selecție a formei
                                elif item["name"] == "Mystery Box":
                                    mystery_box_rewards = process_mystery_box()
                                    mystery_box_opened = False
                                    revealed_rewards = []
                                    game_state = "mystery_box"
                                elif "Hint" in item["name"]:
                                    hints_left += 10  # Golden Hint oferă acum 10 hint-uri

            elif game_state == "levels":
                if back_button_rect.collidepoint(event.pos):
                    if current_level_page is None:
                        game_state = "home"
                    else:
                        current_level_page = None
                elif current_level_page is None:
                    for i, button in enumerate(level_buttons):
                        if button.collidepoint(event.pos):
                            current_level_page = i + 1
                            break
                else:
                    for i, button in enumerate(sub_level_buttons):
                        if button.collidepoint(event.pos):
                            initialize_game(current_level_page, i + 1)
                            game_state = "game"
                            break

            elif game_state == "game":
                if back_button_rect.collidepoint(event.pos):
                    game_state = "home"
                elif help_button_rect.collidepoint(event.pos):
                    if hints_left > 0:
                        give_hint(lights, 3 + (current_level - 1))
                        hints_left -= 1
                        moves += 1
                elif reset_button_rect.collidepoint(event.pos):
                    reset_level()
                else:
                    grid_size = 2 + current_level
                    light_size = screen_width // grid_size
                    for row in range(grid_size):
                        for col in range(grid_size):
                            rect = pygame.Rect(col * light_size, row * light_size, light_size, light_size)
                            if rect.collidepoint(event.pos):
                                toggle_light(lights, row, col, grid_size)
                                moves += 1

            elif game_state == "purchase":
                if back_button_rect.collidepoint(event.pos):
                    game_state = "home"
                for i, rect in enumerate(button_rects):
                    if rect.collidepoint(event.pos):
                        hints_purchased = [20, 50, 100, 200, 400][i]
                        hints_left += hints_purchased
                        game_state = "home"
                if ad_button_rect.collidepoint(event.pos):
                    hints_left += 10
                    game_state = "home"

            elif game_state == "rewards":
                if back_button_rect.collidepoint(event.pos):
                    game_state = "home"
                for i, button in enumerate(reward_buttons):
                    if button.collidepoint(event.pos):
                        if i + 1 == daily_streak:
                            daily_streak, last_claimed_date = claim_reward(daily_streak, last_claimed_date)

            elif game_state == "mystery_box":
                if open_button_rect and open_button_rect.collidepoint(event.pos) and not mystery_box_opened:
                    mystery_box_opened = True
                    revealed_rewards = []
                    last_reveal_time = pygame.time.get_ticks()
                if claim_button_rect and claim_button_rect.collidepoint(event.pos) and mystery_box_opened:
                    for reward in mystery_box_rewards:
                        if "🔄" in reward:
                            spins_left += int(reward.split(" ")[1])
                        elif "💎" in reward:
                            emeralds += int(reward.split(" ")[1])
                        elif "💡" in reward:
                            hints_left += int(reward.split(" ")[1])
                    mystery_box_rewards = []
                    revealed_rewards = []
                    game_state = "home"

        if event.type == pygame.MOUSEWHEEL:
            if game_state == "shop":
                scroll_offset += event.y * 30
                max_scroll = len(item_rects) * 120 - screen_height + 200
                scroll_offset = max(-max_scroll, min(0, scroll_offset))

    # Resetează seria zilnică
    today = datetime.now().date()
    if last_claimed_date and (today - last_claimed_date).days > 1:
        daily_streak = 1

    # Verifică câștigarea nivelului
    if game_state == "game" and check_win(lights, 2 + current_level):
        win_sound.play()
        emeralds_won = current_level * 5
        claim_pressed = False

        while not claim_pressed:
            mouse_pos = pygame.mouse.get_pos()
            screen.fill(BACKGROUND_COLOR)

            claim_button_rect = draw_win_message(current_level, emeralds_won, mouse_pos)

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    running = False
                    claim_pressed = True
                elif event.type == pygame.MOUSEBUTTONDOWN:
                    if claim_button_rect.collidepoint(event.pos):
                        emeralds += emeralds_won
                        claim_pressed = True

            pygame.display.update()

        if current_sub_level < total_sub_levels:
            if current_sub_level % 10 == 0:
                current_level += 1
            current_sub_level += 1
            initialize_game(current_level, current_sub_level % 10 if current_sub_level % 10 != 0 else 10)
        else:
            game_state = "home"

    pygame.display.update()
