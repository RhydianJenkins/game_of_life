<template>
  <div class="game-of-life">
    <div v-for="(row, rowIndex) in grid" :key="rowIndex">
      <div v-for="(col, colIndex) in row" :key="colIndex">
        <Cell
          :position="{ x: rowIndex, y: colIndex }"
          :neighbours="getCellNeighbours()"
        />
      </div>
    </div>
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
      return this.grid[x][y] || {};
    },
    getCellNeighbours(x, y) {
      return {
        NW: this.getCell(),
        N: {},
        NE: {},
        E: {},
        SE: {},
        S: {},
        SW: {},
        W: {},
      };
    }
  },
  created() {
    for (let x = 0; x < this.size; x++) {
      this.grid.push([])
      for (let y = 0; y < this.size; y++) {
        this.grid[i].push({ x: i })
      }
    }
    // console.log(this.getCell(4, 4));
  }
}
</script>

<style scoped>
.game-of-life {
  display: flex;
}
</style>
