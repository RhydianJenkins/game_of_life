<template>
  <div id="app">
    <GameOfLife
      :size="50"
    />
  </div>
</template>

<script>
import GameOfLife from './components/GameOfLife.vue'

export default {
  name: 'App',
  components: {
    GameOfLife
  }
}
</script>

<style>
#app {

}
</style>
