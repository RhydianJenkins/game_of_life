<template>
  <div>
    <h1 class="display-1 text-center">Game of Life</h1>
    <RulesPopover/>
    <GameOfLifeGrid class="game-of-life" :grid="grid"/>
    <Controls @iterate-invoked="iterate()"/>
  </div>
</template>

<script>
import Vue from 'vue'
import RulesPopover from './RulesPopover.vue'
import GameOfLifeGrid from './GameOfLifeGrid.vue'
import Controls from './Controls.vue'
import STATE from '../assets/states.js';

export default {
  name: 'GameOfLife',
  components: {
    RulesPopover,
    GameOfLifeGrid,
    Controls
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
    }
  },
  created() {
    this.buildGrid(this.size);
  }
}
</script>

<style lang="scss">
.game-of-life {
  margin: 0 auto;

  &__grid {
    justify-content: center;
    display: flex;
  }

  &__cell {
    cursor: pointer;
    width: 20px;
    height: 20px;
    text-align: center;
    border: 1px solid;

    &--alive {
      width: 100%;
      height: 100%;
      background-color: black;
    }

    &--dead {
      width: 100%;
      height: 100%;
      background-color: green;
    }
  }
}
</style>
