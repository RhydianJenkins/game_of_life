# Game of Life (vue implementation)

![image](https://user-images.githubusercontent.com/9198690/133149496-b3b54c01-d72f-47f5-bd11-d3e1ffe80600.png)

## Rules
1. Any live cell with fewer than two live neighbours dies, as if by underpopulation.
2. Any live cell with two or three live neighbours lives on to the next generation.
3. Any live cell with more than three live neighbours dies, as if by overpopulation.
4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

## Installation
Clone, build, run. Simple.
```bash
git clone https://github.com/RhydianJenkins/game_of_life.git && cd game_of_life
npm i
npm run serve -- --open
```
