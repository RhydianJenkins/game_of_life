<template>
  <div class="game-of-life">
    <h1>Game of Life</h1>
    <div class="game-of-life__grid" v-for="(row, rowIndex) in grid" :key="rowIndex">
      <div v-for="(cell, colIndex) in row" :key="colIndex">
        <Cell
          :grid="grid"
          :data="cell"
        />
      </div>
    </div>
    <button class="game-of-life__button" @click="iterate()">Next</button>
  </div>
</template>

<script>
import { bus } from '../main.js'
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
      bus.$emit('calculateNextState');
      bus.$emit('update');
    },
    buildGrid(size) {
      for (let x = 0; x < size; x++) {
        this.grid.push([])
        for (let y = 0; y < size; y++) {
          const cell = { x: x, y: y };
          // cell['initialState'] = y % 2 === 0 ? this.STATE.ALIVE : this.STATE.DEAD;
          cell['initialState'] = this.STATE.DEAD;
          cell['state'] = cell['initialState'];
          this.grid[x].push(cell)
        }
      }

      // add alive cells
      this.grid[4][2].state = this.STATE.ALIVE;
      this.grid[4][2].initialState = this.STATE.ALIVE;
      this.grid[4][3].state = this.STATE.ALIVE;
      this.grid[4][3].initialState = this.STATE.ALIVE;
      this.grid[4][4].state = this.STATE.ALIVE;
      this.grid[4][4].initialState = this.STATE.ALIVE;
    }
  },
  created() {
    this.buildGrid(this.size);
  }
}
</script>

<style scoped>
.game-of-life {
  width: 80vw;
  margin: 0 auto;
}

.game-of-life__grid {
  justify-content: center;
  display: flex;
}

.game-of-life__button {
  margin-top: 10px;
  width: 64px;
}
</style>
