<template>
    <div :class="cellStateClass"/>
</template>

<script>
import STATE from '../assets/states.js';

export default {
    name: 'Cell',
    props: {
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
    },
    computed: {
        cellStateClass() {
            return this.data.state === STATE.ALIVE ? 'game-of-life__cell--alive' : 'game-of-life__cell--dead';
        }
    }
}
</script>

<style lang="scss" scoped>
</style>