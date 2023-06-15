<template>
  <div class="app">
    <p>Соедини парные ячейки!</p>
    <Board></Board>
  </div>
</template>

<script>
import Board from "./components/Board.vue";
export default {
  name: "App",
  components: {
    Board,
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  padding-top: 60px;
  user-select: none;
}
</style>
