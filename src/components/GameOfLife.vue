<template>
  <div class="game-of-life">
    <h1>Game of Life</h1>
    <h2>Rules</h2>
    <ul>
      <li>Any live cell with fewer than two live neighbours dies, as if by underpopulation.</li>
      <li>Any live cell with two or three live neighbours lives on to the next generation.</li>
      <li>Any live cell with more than three live neighbours dies, as if by overpopulation.</li>
      <li>Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.</li>
    </ul>
    <div class="game-of-life__grid" v-for="(row, rowIndex) in grid" :key="rowIndex">
      <div v-for="(cell, colIndex) in row" :key="colIndex">
        <div class="game-of-life__cell" @click="toggleState(cell)">
          <Cell :data="cell" />
        </div>
      </div>
    </div>
    <p><span style="color: red">RED</span> = alive, <span style="color: green">GREEN</span> = dead.</p>
    <p>Click on cells to toggle their states, and click 'Next' to iterate to the next generation.</p>
    <button class="game-of-life__button" @click="iterate()">Next</button>
  </div>
</template>

<script>
import Vue from 'vue'
import Cell from './Cell.vue'
import STATE from '../assets/states.js';

export default {
  name: 'GameOfLife',
  components: {
    Cell
  },
  props: {
    size: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      grid: [],
      STATE
    };
  },
  methods: {
    iterate() {
      this.calculateNextStates();
      this.update();
    },
    buildGrid(size) {
      for (let x = 0; x < size; x++) {
        this.grid.push([])
        for (let y = 0; y < size; y++) {
          const cell = { x: x, y: y };
          cell['initialState'] = this.STATE.DEAD;
          cell['state'] = cell['initialState'];
          this.grid[x].push(cell)
        }
      }

      // add neighbours
      for (const row of this.grid) {
        for (const cell of row) {
          cell.neighbours = this.getNeighbours(cell);
        }
      }
    },
    getNeighbours(cell = {}) {
      return {
        NW: this.getCell(cell.x - 1, cell.y - 1),
        N: this.getCell(cell.x, cell.y - 1),
        NE: this.getCell(cell.x + 1, cell.y - 1),
        E: this.getCell(cell.x + 1, cell.y),
        SE: this.getCell(cell.x + 1, cell.y + 1),
        S: this.getCell(cell.x, cell.y + 1),
        SW: this.getCell(cell.x - 1, cell.y + 1),
        W: this.getCell(cell.x - 1, cell.y)
      };
    },
    calculateNextStates() {
      for (const row of this.grid) {
        for (const cell of row) {
          this.calculateNextState(cell);
        }
      }
    },
    calculateNextState(cell = {}) {
      const n = Object.values(cell.neighbours).filter(neighbour => neighbour.state === STATE.ALIVE).length;

      if (cell.state === STATE.ALIVE && n < 2) {
        Vue.set(cell, 'nextState', STATE.DEAD);
        return;
      }

      if (cell.state === STATE.ALIVE && (n === 2 || n === 3)) {
        Vue.set(cell, 'nextState', STATE.ALIVE);
        return;
      }

      if (cell.state === STATE.ALIVE && n > 3) {
        Vue.set(cell, 'nextState', STATE.DEAD);
        return;
      }

      if (cell.state === STATE.DEAD && n === 3) {
        Vue.set(cell, 'nextState', STATE.ALIVE);
        return;
      }

      Vue.set(cell, 'nextState', cell.state);
    },
    update() {
      for (const row of this.grid) {
        for (const cell of row) {
          Vue.set(cell, 'state', cell.nextState);
        }
      }
    },
    getCell(x, y) {
      const xWrapped = (x % this.size + this.size) % this.size
      const yWrapped = (y % this.size + this.size) % this.size
      return this.grid[xWrapped][yWrapped];
    },
    toggleState(cell = {}) {
      const oppositeState = cell.state === STATE.DEAD ? STATE.ALIVE : STATE.DEAD;
      Vue.set(cell, 'state', oppositeState);
    }
  },
  created() {
    this.buildGrid(this.size);
  }
}
</script>

<style lang="scss">
.game-of-life {
  width: 80vw;
  margin: 0 auto;

  &__grid {
    justify-content: center;
    display: flex;
  }

  &__button {
    cursor: pointer;
    margin-top: 10px;
    width: 64px;
  }

  &__cell {
    cursor: pointer;
    border: 1px black;
    border-width: thin;
    width: 8px;
    height: 8px;
    text-align: center;

    &--alive {
      width: 100%;
      height: 100%;
      background-color: red;
    }

    &--dead {
      width: 100%;
      height: 100%;
      background-color: green;
    }
  }

}
</style>
