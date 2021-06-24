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
        <Cell
          :grid="grid"
          :data="cell"
        />
      </div>
    </div>
    <p><span style="color: red">RED</span> = alive, <span style="color: green">GREEN</span> = dead.</p>
    <p>Click on cells to toggle their states, and click 'Next' to iterate to the next generation.</p>
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
    }
  },
  created() {
    this.buildGrid(this.size);
  }
}
</script>

<style lang="scss" scoped>
.game-of-life {
  width: 80vw;
  margin: 0 auto;

  &__grid {
    justify-content: center;
    display: flex;
  }

  &__button {
    margin-top: 10px;
    width: 64px;
  }
}
</style>
