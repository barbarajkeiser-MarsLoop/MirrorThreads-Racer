i# MirrorThreads Racer 🏎️💜🪞✨

**A high-octane interdimensional racing battle royale where threads weave chaos, mirrors flip reality, and Clue weapons decide who survives.**

[![Hero Banner](images/hero-cosmic-racer.png)](https://github.com/barbarajkeiser-MarsLoop/MirrorThreads-Racer)  
*(Futuristic racer blasting through mirror portals in a neon cosmos – your game in full glory!)*

## Concept Art Gallery 🌌

Here’s the stunning visual inspiration behind MirrorThreads:

<div align="center">
  <img src="images/cosmic-warrior-racer.png" alt="Cosmic female racer with red panda companion" width="45%">
  <img src="images/thread-portal-racer.png" alt="Racer speeding through thread-filled mirror dimensions" width="45%">
</div>

<div align="center">
  <img src="images/dark-portal-warrior.png" alt="Mysterious dark warrior stepping through flaming portals" width="45%">
  <img src="images/characters-montage.png" alt="Epic character lineup: Foxes, soldiers, warriors & more" width="45%">
</div>

> *From cosmic foxes with glowing eyes to armored soldiers and interdimensional warriors – every thread tells a story.*

## 🌟 Features

- 🏁 **High-Speed Arcade Racing** — Drift, boost, and warp through procedurally twisted tracks
- 🔫 **Clue-Inspired Combat** — Wield revolver, candlestick, wrench & more (endless crafting combos!)
- 🧵 **Thread Crafting System** — Collect & combine mystical threads to forge deadly weapons & upgrades
- 🪞 **Mirror & Portal Mechanics** — Flip dimensions, ambush from reflections, escape via portals
- 👾 **Battle Royale Mode** — Press **B** → Survive shrinking zones against smart AI bots (foxes & soldiers)
- ⚡ **Dynamic Progression** — Coins, inventory, multipliers, permanent unlocks

## 🎮 Controls

| Action              | Keys                          |
|---------------------|-------------------------------|
| Accelerate          | W / ↑                         |
| Brake / Reverse     | S / ↓                         |
| Steer Left / Right  | A / ←    •   D / →            |
| Boost               | SPACE                         |
| Aim & Shoot         | Mouse move + Left Click       |
| Craft Weapon        | C (needs 2+ threads)          |
| Toggle BR Mode      | B                             |

## 🔧 Weapons Overview

| Weapon       | Emoji | Type       | Damage | Special                  |
|--------------|-------|------------|--------|--------------------------|
| Revolver     | 🔫    | Projectile | 50     | Fast accurate shots      |
| Candlestick  | 🕯️    | AOE        | 30     | Area blaze damage        |
| Wrench       | 🔧    | Boomerang  | 40     | Returns for extra hits   |

*Craft more by mixing thread types – endless possibilities!*

## 💾 Quick Install & Play

```bash
# Clone & run (recommended)
git clone https://github.com/barbarajkeiser-MarsLoop/MirrorThreads-Racer.git
cd MirrorThreads-Racer
pip install pygame
python mirrorthreads_v3.py   # or latest version

pip install pygame
# Then download & run the latest .py file from repo

---

import pygame
import math
import random
from typing import List, Tuple

# Init (same as v2)
pygame.init()
WIDTH, HEIGHT = 1200, 800
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("MirrorThreads v3 - Clue Battle Royale 💜🔫🪞")
clock = pygame.time.Clock()
font = pygame.font.SysFont('Arial', 24)
small_font = pygame.font.SysFont('Arial', 18)

# Colors + Clue theme
PURPLE = (138, 43, 226)
DARK_PURPLE = (75, 0, 130)
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GLOW = (200, 100, 255)
FOX_ORANGE = (255, 165, 0)
SOLDIER_RED = (200, 0, 0)
MIRROR_SILVER = (192, 192, 192)
CLUE_GOLD = (255, 215, 0)  # Mansion vibe

# Clue Weapons
WEAPONS = {
    'revolver': {'color': (100, 100, 255), 'emoji': '🔫', 'proj_speed': 10, 'damage': 50},
    'candlestick': {'color': GLOW, 'emoji': '🕯️', 'aoe': 50, 'damage': 30},
    'wrench': {'color': (150, 150, 150), 'emoji': '🔧', 'boomerang': True, 'speed': 8, 'damage': 40}
}

class Weapon:
    def __init__(self, wtype: str):
        self.type = wtype
        self.data = WEAPONS[wtype]
        self.cooldown = 0
        self.ammo = 10  # Infinite via threads?

    def can_shoot(self):
        return self.cooldown <= 0

    def shoot(self, px, py, angle, targets):
        self.cooldown = 20
        if self.type == 'revolver':
            return [Projectile(px, py, math.sin(angle)*self.data['proj_speed'], -math.cos(angle)*self.data['proj_speed'], self.data['damage'])]
        elif self.type == 'candlestick':
            return self.aoe_damage(px, py, self.data['aoe'], self.data['damage'], targets)
        elif self.type == 'wrench':
            return [Boomerang(px, py, angle, self.data['speed'], self.data['damage'])]
        return []

    def aoe_damage(self, x, y, radius, dmg, targets):
        for t in targets:
            if math.hypot(t.x - x, t.y - y) < radius:
                t.health -= dmg
        return []

class Projectile:
    def __init__(self, x, y, vx, vy, dmg):
        self.x, self.y = x, y
        self.vx, self.vy = vx, vy
        self.dmg = dmg
        self.size = 5
        self.life = 100

    def update(self):
        self.x += self.vx
        self.y += self.vy
        self.life -= 1
        return self.life > 0 and 0 <= self.x < WIDTH and 0 <= self.y < HEIGHT

    def draw(self, screen):
        pygame.draw.circle(screen, BLUE, (int(self.x), int(self.y)), self.size)

class Boomerang:
    def __init__(self, x, y, angle, speed, dmg):
        self.x, self.y = x, y
        self.angle = angle
        self.speed = speed
        self.dmg = dmg
        self.time = 0
        self.size = 10

    def update(self):
        self.time += 1
        r = self.speed * self.time
        if self.time > 50:  # Return
            r = self.speed * (100 - self.time)
        self.x += math.sin(self.angle) * r / 10
        self.y -= math.cos(self.angle) * r / 10
        return self.time < 100

    def draw(self, screen):
        pygame.draw.circle(screen, (150,150,150), (int(self.x), int(self.y)), self.size)

BLUE = (100, 150, 255)

class Bot:
    def __init__(self, x, y, bot_type='soldier'):
        self.x, self.y = x, y
        self.angle = 0
        self.speed = 3
        self.health = 100
        self.max_health = 100
        self.type = bot_type
        self.color = SOLDIER_RED if bot_type == 'soldier' else FOX_ORANGE
        self.weapon = Weapon(random.choice(list(WEAPONS.keys())))
        self.shoot_timer = random.randint(60, 120)

    def update(self, player, projectiles, zone_center):
        # Simple AI: chase player, shoot
        dx, dy = player.x - self.x, player.y - self.y
        dist = math.hypot(dx, dy)
        if dist > 0:
            self.angle = math.atan2(dx, -dy)
            if dist < 200:
                self.x += math.sin(self.angle) * self.speed
                self.y -= math.cos(self.angle) * self.speed

        # Zone damage
        zone_dist = math.hypot(self.x - zone_center[0], self.y - zone_center[1])
        if zone_dist > 300:
            self.health -= 1

        self.shoot_timer -= 1
        if self.shoot_timer <= 0 and dist < 250:
            projs = self.weapon.shoot(self.x, self.y, self.angle, [player])
            projectiles.extend(projs)
            self.shoot_timer = random.randint(90, 180)

        self.health = max(0, self.health)

    def draw(self, screen):
        points = [  # Simple triangle
            (self.x + math.sin(self.angle) * 15, self.y - math.cos(self.angle) * 15),
            (self.x + math.sin(self.angle + 2.5) * 8, self.y - math.cos(self.angle + 2.5) * 8),
            (self.x + math.sin(self.angle + 4.5) * 8, self.y - math.cos(self.angle + 4.5) * 8)
        ]
        pygame.draw.polygon(screen, self.color, points)
        # Health bar
        bar_w = 30
        pygame.draw.rect(screen, RED, (self.x - 15, self.y - 30, bar_w, 5))
        pygame.draw.rect(screen, GREEN, (self.x - 15, self.y - 30, bar_w * self.health / self.max_health, 5))
        # Weapon emoji
        text = small_font.render(self.weapon.data['emoji'], True, WHITE)
        screen.blit(text, (self.x - 10, self.y + 15))

RED = (255,0,0)
GREEN = (0,255,0)

class Player:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.angle = 0
        self.speed = 0
        self.max_speed = 5
        self.accel = 0.3  # Faster for BR
        self.friction = 0.1
        self.size = 20
        self.health = 100
        self.max_health = 100
        self.weapon: Weapon = None
        self.inventory = []
        self.coins = 0
        # ... (add_thread, craft from v2, but craft now makes weapons)

    def craft_weapon(self):
        if len(self.inventory) >= 2:
            # Assume last 2 for weapon
            del self.inventory[-2:]
            wtype = random.choice(list(WEAPONS.keys()))
            self.weapon = Weapon(wtype)
            return f"Equipped {wtype.upper()}!"
        return "Need 2 threads"

    def update(self, keys, mouse_pos, mouse_down, dt, projectiles, bots, zone_center, battle_mode):
        # Drive (enhanced)
        keys_pressed = pygame.key.get_pressed()
        if keys_pressed[pygame.K_w] or keys_pressed[pygame.K_UP]: self.speed = min(self.speed + self.accel, self.max_speed)
        if keys_pressed[pygame.K_s] or keys_pressed[pygame.K_DOWN]: self.speed = max(self.speed - self.accel, -self.max_speed / 2)
        if keys_pressed[pygame.K_a] or keys_pressed[pygame.K_LEFT]: self.angle -= 0.15
        if keys_pressed[pygame.K_d] or keys_pressed[pygame.K_RIGHT]: self.angle += 0.15
        if keys_pressed[pygame.K_SPACE]: self.speed *= 1.05  # Mini-boost

        self.speed *= (1 - self.friction)
        self.x += math.sin(self.angle) * self.speed
        self.y -= math.cos(self.angle) * self.speed
        self.x %= WIDTH
        self.y %= HEIGHT

        # Aim/shoot with mouse
        mx, my = mouse_pos
        shoot_angle = math.atan2(mx - self.x, -(my - self.y))
        if mouse_down[0] and self.weapon and self.weapon.can_shoot():
            projs = self.weapon.shoot(self.x, self.y, shoot_angle, bots)
            projectiles.extend(projs)

        if self.weapon: self.weapon.cooldown = max(0, self.weapon.cooldown - 1)

        # Zone damage
        if battle_mode:
            zone_dist = math.hypot(self.x - zone_center[0], self.y - zone_center[1])
            if zone_dist > 300:
                self.health -= 0.5

        # Proj hits
        for p in projectiles[:]:
            if math.hypot(p.x - self.x, p.y - self.y) < self.size + p.size:
                self.health -= p.dmg
                projectiles.remove(p)

    def draw(self, screen):
        color = GLOW if self.weapon else PURPLE
        points = [
            (self.x + math.sin(self.angle) * self.size, self.y - math.cos(self.angle) * self.size),
            (self.x + math.sin(self.angle + 2.5) * self.size / 2, self.y - math.cos(self.angle + 2.5) * self.size / 2),
            (self.x + math.sin(self.angle + 4.5) * self.size / 2, self.y - math.cos(self.angle + 4.5) * self.size / 2)
        ]
        pygame.draw.polygon(screen, color, points)
        # Health
        bar_w = 40
        pygame.draw.rect(screen, RED, (self.x - 20, self.y - 35, bar_w, 6))
        pygame.draw.rect(screen, GREEN, (self.x - 20, self.y - 35, bar_w * self.health / self.max_health, 6))
        if self.weapon:
            text = small_font.render(self.weapon.data['emoji'], True, WHITE)
            screen.blit(text, (self.x + 15, self.y - 10))

# (Thread, Portal classes same as v2)

# Game
player = Player(WIDTH//2, HEIGHT//2)
bots: List[Bot] = []
projectiles: List = []
threads = []
portals = []
score, coins = 0, 0
battle_mode = False
zone_center = (WIDTH//2, HEIGHT//2)
zone_size = 600
message = ""
alive_bots = 0

running = True
while running:
    dt = clock.tick(60) / 1000.0 * 60
    screen.fill(BLACK if not battle_mode else DARK_PURPLE)
    mouse_pos = pygame.mouse.get_pos()
    mouse_down = pygame.mouse.get_pressed()
    keys = pygame.key.get_pressed()
    for event in pygame.event.get():
        if event.type == pygame.QUIT: running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_b: 
                battle_mode = not battle_mode
                if battle_mode:
                    bots = [Bot(random.randint(100,WIDTH-100), random.randint(100,HEIGHT-100), random.choice(['soldier','fox'])) for _ in range(3)]
                    message = "CLUE BATTLE ROYALE! Last standing wins! 🔫🕯️🔧"
            if event.key == pygame.K_c: message = player.craft_weapon()

    # Updates
    player.update(keys, mouse_pos, mouse_down, dt, projectiles, bots, zone_center, battle_mode)

    # Bots update
    alive_bots = 0
    for bot in bots[:]:
        bot.update(player, projectiles, zone_center)
        if bot.health > 0:
            alive_bots += 1
        else:
            bots.remove(bot)
            coins += 100

    # Projectiles
    for proj in projectiles[:]:
        if hasattr(proj, 'update') and not proj.update():
            projectiles.remove(proj)
        else:
            proj.draw(screen) if not isinstance(proj, list) else None

    # Zone shrink
    if battle_mode:
        zone_size -= 0.5
        pygame.draw.circle(screen, CLUE_GOLD, zone_center, int(zone_size), 3)
        if zone_size < 50: zone_size = 50

    if battle_mode and alive_bots == 0:
        message = "YOU WIN! 💜🏆"
        coins += 1000

    if player.health <= 0:
        message = "Game Over! Restart?"
        player.health = 100

    # Spawns/collisions (simplified from v2 for brevity)
    if random.random() < 0.01: threads.append(Thread(random.choice(Thread.TYPES)))
    # (Add portal/thread logic here - omitted for space)

    # Draws (player, bots, inv, etc.)
    player.draw(screen)
    for bot in bots: bot.draw(screen)
    # (Inventory draw/craft rect)

    # UI
    screen.blit(font.render(f"Health: {int(player.health)} | Coins: {coins} | Bots: {alive_bots}", True, PURPLE), (10, 10))
    screen.blit(font.render("B: Clue BR | C: Craft Weapon | Mouse Shoot", True, WHITE), (10, HEIGHT-30))
    if message:
        screen.blit(font.render(message, True, GLOW), (10, 40))

    pygame.display.flip()

pygame.quit()

---

 Roadmap Online multiplayer lobbies
 Custom racer skins & fox/soldier variants
 Achievements & leaderboards
 Pulsing soundtrack that reacts to speed/portals
 New arenas: Clue mansions, cosmic voids, fox dens

Built with  by Barbara Keiser
Race. Thread. Survive.   Star this repo if you're ready to warp through mirrors!
 Issues/PRs welcome – let's build this empire together #MirrorThreads #ThreadTheory #xAI #Pygame

