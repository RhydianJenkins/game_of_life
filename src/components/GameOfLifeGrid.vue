<template>
    <div class="game-of-life__grid">
        <div class="game-of-life__row" v-for="(row, rowIndex) in grid" :key="rowIndex">
            <div class="game-of-life__cell" v-for="(cell, colIndex) in row" :key="colIndex" @click="toggleState(cell)">
                <Cell :data="cell" />
            </div>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
import Cell from './Cell.vue'
import STATE from '../assets/states.js';

export default {
    name: 'GameOfLifeGrid',
    components: {
        Cell
    },
    props: {
        grid: {
            type: Array,
            required: true
        }
    },
    computed: {
        //
    },
    methods: {
        toggleState(cell = {}) {
            const oppositeState = cell.state === STATE.DEAD ? STATE.ALIVE : STATE.DEAD;
            Vue.set(cell, 'state', oppositeState);
        }
    }
}
</script>

<style lang="scss" scoped>
</style>