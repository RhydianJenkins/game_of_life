<template>
  <div class="game-of-life">
    <div class="game-of-life__grid" v-for="(row, rowIndex) in grid" :key="rowIndex">
      <div v-for="(cell, colIndex) in row" :key="colIndex">
        <Cell
          :data="cell"
          :neighbours="getCellNeighbours(rowIndex)"
        />
      </div>
    </div>
    <button class="game-of-life__button" @click="iterate()">Next</button>
  </div>
</template>

<script>
import Cell from './Cell.vue'

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
      grid: []
    };
  },
  methods: {
    getCell(x, y) {
      const xWrapped = Math.abs(x % this.size);
      const yWrapped = Math.abs(y % this.size);
      return this.grid[xWrapped][yWrapped];
    },
    getCellNeighbours(x, y) {
      return {
        NW: this.getCell(--x, --y),
        N: this.getCell(x, --y),
        NE: this.getCell(++x, --y),
        E: this.getCell(++x, y),
        SE: this.getCell(++x, ++y),
        S: this.getCell(x, ++y),
        SW: this.getCell(--x, ++y),
        W: this.getCell(--x, y)
      };
    },
    iterate() {
      console.log('iterate!');
    }
  },
  created() {
    for (let x = 0; x < this.size; x++) {
      this.grid.push([])
      for (let y = 0; y < this.size; y++) {
        this.grid[x].push({ x: x, y: y })
      }
    }
  }
}
</script>

<style scoped>
.game-of-life__grid {
  display: flex;
}

.game-of-life__button {
  margin-top: 10px;
}
</style>
