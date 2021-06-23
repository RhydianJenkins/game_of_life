<template>
  <div class="game-of-life__cell">
    {{ `${data.x}, ${data.y}` }}
  </div>
</template>

<script>
export default {
  name: 'Cell',
  props: {
    data: {
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
    width: 32px;
    height: 32px;
    text-align: center;

    .alive {
        background-color: white;
    }

    .dead {
        background-color: black;
    }
}
</style>
