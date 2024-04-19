<template>
  <div id="app">
    <Board/>
  </div>
</template>

<script>
import Board from '@/components/board'

export default {
  name: 'App',
  components: {
    Board,
  }
}
</script>

<style>
::-webkit-scrollbar {
  width: 2px;
  height: 2px;
}
</style>