<template>
  <div class="game-of-life__cell">
    <p>{{ `${position.x}, ${position.y}` }}</p>
  </div>
</template>

<script>
export default {
  name: 'Cell',
  props: {
    position: {
      type: Object,
      required: true
    },
    neighbours: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.game-of-life__cell {
    border: 1px solid;
    width: 64px;
    height: 64px;
}
</style>
