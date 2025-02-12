#  Lights Game

## About the Game
"Lights Game" is a challenging and engaging puzzle game built with **Python** and **Pygame**. Your goal is to turn on all the lights by clicking on them. However, each light you click will also toggle the state of its adjacent lights, adding complexity to the puzzle!

The game features **multiple difficulty levels**, **animated glowing effects**, **interactive sound effects**, and **a dynamic background**. Earn **emeralds** as you progress, which can be used in the in-game shop for hints and rewards. Daily rewards and a spinning wheel add extra incentives to keep playing!

---

##  Features
✔ Multiple difficulty levels (**3x3 to 12x12 grids**)
✔ Animated **glowing effects** for lights
✔ **Sound effects** and **background music**
✔ **Daily rewards** and a **spinning wheel** for bonuses
✔ **Virtual shop** with in-game currency (**emeralds**)
✔ Various **light shapes** (circle, square, triangle)
✔ **Hint system** to assist players
✔ **Elegant UI** with smooth transitions

---

##  Code Example
Lights and their adjacent neighbors:

```python
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
```
This function ensures that clicking on one light affects its surrounding neighbors, making the puzzle more complex and interesting.

---

##  How to Play
1. Click on a light to toggle it and its adjacent lights.
2. The goal is to **turn all the lights ON**.
3. Earn **emeralds** by completing levels.
4. Use emeralds to **buy hints** or **spin the wheel** for rewards.
5. Challenge yourself with **larger grids**!

---

##  Installation

### Requirements
- **Python 3.x**
- **Pygame library**

### Setup
#### Clone the repository:
```sh
git clone https://github.com/yourusername/lights-game.git
```

#### Navigate to the folder:
```sh
cd lights-game
```

#### Install dependencies:
```sh
pip install pygame
```

#### Run the game:
```sh
python game.py
```

---

Enjoy the game and happy coding! 
