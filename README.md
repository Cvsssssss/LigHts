# 🌟 Lights Game  

## 🎮 About the Game  

"Lights Game" is a puzzle game built with Python and Pygame. The objective is to turn on all the lights by clicking on them. However, each light you click will also toggle the state of its adjacent lights, making the puzzle more challenging! The game offers various difficulty levels, making it engaging for both beginners and advanced players.  

✨ **Key Highlights:**  
- Smooth animations and glowing effects  
- Sound effects and background music  
- A reward system with emeralds  
- Different shapes for lights (circle, square, triangle)  

## 📜 Code Example  

Here is a snippet of the core logic that toggles the lights and their adjacent neighbors:  

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
