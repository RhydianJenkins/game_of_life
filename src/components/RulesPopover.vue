<template>
    <div class="text-center my-3">
        <b-button id="popover-target-rules">Rules</b-button>
        <b-popover target="popover-target-rules" triggers="hover" placement="top">
            <ul>
                <li>Any live cell with fewer than two live neighbours dies, as if by underpopulation.</li>
                <li>Any live cell with two or three live neighbours lives on to the next generation.</li>
                <li>Any live cell with more than three live neighbours dies, as if by overpopulation.</li>
                <li>Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.</li>
            </ul>
        </b-popover>
    </div>
</template>

<script>
export default {
    name: 'RulesPopover',
    props: {
        //
    }
}
</script>

<style lang="scss" scoped>
</style>