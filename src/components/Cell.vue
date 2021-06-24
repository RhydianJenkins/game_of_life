<template>
    <div class="game-of-life__cell" @click="toggleState">
        <div :class="cellStateClass"/>
    </div>
</template>

<script>
import Vue from 'vue'
import STATE from '../assets/states.js';
import { bus } from '../main.js'

export default {
    name: 'Cell',
    props: {
        grid: {
            type: Array,
            required: true
        },
        data: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            STATE
        };
    },
    methods: {
        /**
         * 1. Any live cell with fewer than two live neighbours dies, as if by underpopulation.
         * 2. Any live cell with two or three live neighbours lives on to the next generation.
         * 3. Any live cell with more than three live neighbours dies, as if by overpopulation.
         * 4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
         */
        calculateNextState() {
            const n = this.numAliveNeighbours;

            if (this.data.state === STATE.ALIVE && n < 2) {
                Vue.set(this.data, 'nextState', STATE.DEAD);
                return;
            }

            if (this.data.state === STATE.ALIVE && (n === 2 || n === 3)) {
                Vue.set(this.data, 'nextState', STATE.ALIVE);
                return;
            }

            if (this.data.state === STATE.ALIVE && n > 3) {
                Vue.set(this.data, 'nextState', STATE.DEAD);
                return;
            }

            if (this.data.state === STATE.DEAD && n === 3) {
                Vue.set(this.data, 'nextState', STATE.ALIVE);
                return;
            }

            Vue.set(this.data, 'nextState', this.data.state);
        },
        update() {
            Vue.set(this.data, 'state', this.data.nextState);
        },
        getCell(x, y) {
            const xWrapped = (x % this.gridSize + this.gridSize) % this.gridSize
            const yWrapped = (y % this.gridSize + this.gridSize) % this.gridSize
            return this.grid[xWrapped][yWrapped];
        },
        toggleState() {
            const oppositeState = this.data.state === STATE.DEAD ? STATE.ALIVE : STATE.DEAD;
            Vue.set(this.data, 'state', oppositeState);
        }
    },
    computed: {
        neighbours() {
            return {
                NW: this.getCell(this.data.x - 1, this.data.y - 1),
                N: this.getCell(this.data.x, this.data.y - 1),
                NE: this.getCell(this.data.x + 1, this.data.y - 1),
                E: this.getCell(this.data.x + 1, this.data.y),
                SE: this.getCell(this.data.x + 1, this.data.y + 1),
                S: this.getCell(this.data.x, this.data.y + 1),
                SW: this.getCell(this.data.x - 1, this.data.y + 1),
                W: this.getCell(this.data.x - 1, this.data.y)
            };
        },
        numAliveNeighbours() {
            return Object.values(this.neighbours).filter(neighbour => neighbour.state === STATE.ALIVE).length;
        },
        gridSize() {
            return this.grid.length;
        },
        currentState() {
            return this.data.state;
        },
        cellStateClass() {
            return this.data.state === STATE.ALIVE ? 'game-of-life__cell--alive' : 'game-of-life__cell--dead';
        }
    },
    created() {
        bus.$on('calculateNextState', () => this.calculateNextState());
        bus.$on('update', () => this.update());
    }
}
</script>

<style scoped>
.game-of-life__cell {
    border: 1px black;
    border-width: thin;
    width: 8px;
    height: 8px;
    text-align: center;
}

.game-of-life__cell--alive {
    width: 100%;
    height: 100%;
    background-color: red;
}

.game-of-life__cell--dead {
    width: 100%;
    height: 100%;
    background-color: green;
}
</style>