<template>
  <div v-if="gameStatus === 0" class="board__block">
    <h2>Уровень: {{ level }}</h2>
    <p>время игры {{ timer }} секунд</p>
    <div
      class="board"
      :style="{ width: 50 * size + 'px', height: 50 * size + 'px' }"
    >
      <BoardItem
        v-for="(cell, index) in cells"
        :cell="cell"
        :key="index"
        @mousedown="mousedown(index)"
        @mouseup="mouseup(index)"
        @mousemove="go(index)"
        :selected="checkRoad(index)"
        :closed="checkClosedRoad(index)"
      >
      </BoardItem>
    </div>
    <div @click="reloadGame()" class="reload">Restart</div>
  </div>
  <div v-if="gameStatus === 1" class="win__block">
    <h3>{{ player }}, Вы победили!</h3>
    <p>Время игры {{ timer }} секунд.</p>
  </div>
</template>

<script>
import { ref } from "vue";
import BoardItem from "./BoardItem.vue";

export default {
  name: "Board",
  components: {
    BoardItem,
  },
  setup() {
    let cells = ref([3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3]);
    let closedPath = ref([]); // закрашенные пути
    let size = ref(4);
    let timer = ref(0);
    let player = prompt("Введите имя");

    const path = ref([]); // путь в данный момент
    const level = ref(1);
    const gameStatus = ref(0);

    const reloadGame = () => {
      startGame(level.value);
    };

    const intervalId = setInterval(() => {
      timer.value += 1;
      if (gameStatus.value === 1) {
        console.log("Done");
        clearInterval(intervalId);
      }
    }, 1000);

    const startGame = (level) => {
      switch (level) {
        case 1:
          cells.value = [3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3];
          size.value = 4;
          break;
        case 2:
          cells.value = [
            0, 2, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 2, 0, 0, 3, 0, 0, 0, 0, 0, 0, 3,
            0, 0,
          ];
          size.value = 5;
          break;
        case 3:
          cells.value = [
            2, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 2, 0, 0, 3, 0, 0, 4, 0, 0, 0, 3,
            0, 4,
          ];
          break;
        case 4:
          cells.value = [
            2, 0, 0, 1, 5, 0, 0, 0, 1, 0, 0, 0, 2, 0, 5, 3, 0, 0, 4, 0, 0, 0, 3,
            0, 4,
          ];
          break;
        case 5:
          cells.value = [
            2, 6, 0, 1, 5, 0, 0, 6, 1, 0, 0, 0, 2, 5, 0, 3, 0, 0, 4, 0, 0, 0, 3,
            0, 4,
          ];
          break;
      }

      path.value = [];
      closedPath.value = [];
    };

    const mousedown = (index) => {
      path.value = [];
      if (cells.value[index] && !checkClosedRoad(index)) {
        path.value.push(index);
      }
    };

    const goToNextLevel = () => {
      level.value += 1;

      if (level.value > size.value) {
        level.value = 1;
        gameStatus.value = 1;
      }
      startGame(level.value);
    };

    const checkLevel = () => {
      let completed = true;

      cells.value.forEach((cell, index) => {
        if (cell && !checkClosedRoad(index)) {
          completed = false;
        }
      });
      if (completed) {
        goToNextLevel();
      }
    };

    const mouseup = (index) => {
      if (
        index !== path.value[0] &&
        cells.value[index] === cells.value[path.value[0]]
      ) {
        closedPath.value = [...closedPath.value, ...path.value];
      }
      path.value = [];
      checkLevel();
    };

    const checkRoad = (index) => {
      return path.value.findIndex((p) => p === index) > -1;
    };

    const checkClosedRoad = (index) => {
      return closedPath.value.findIndex((p) => p === index) > -1;
    };

    const go = (index) => {
      if (path.value.length) {
        const lastIndex = path.value[path.value.length - 1];

        if (
          (Math.abs(lastIndex - index) === 1 ||
            Math.abs(lastIndex - index) === size.value) &&
          !checkClosedRoad(index)
        ) {
          path.value.push(index);
        }
      }

      if (
        checkClosedRoad(index) ||
        (cells.value[index] &&
          cells.value[index] !== cells.value[path.value[0]])
      ) {
        path.value = [];
      }
    };

    return {
      cells,
      level,
      gameStatus,
      size,
      player,
      timer,
      mousedown,
      mouseup,
      checkRoad,
      checkClosedRoad,
      go,
      reloadGame,
    };
  },
};
</script>

<style scoped>
.board {
  width: 200px;
  height: 200px;
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
}

.win__block {
  text-align: center;
}

.reload {
  margin-top: 30px;
  cursor: pointer;
}

.board__block {
  text-align: center;
}
</style>
