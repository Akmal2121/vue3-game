<template>
  <div :class="'board-item ' + (selected ? 'selected' : '') + (closed ? 'closed' : '')">
    <i v-if="cell" :class="'icon icon-' + cell"></i>
  </div>
</template>

<script>
export default {
  name: "BoardItem",
  props: {
    cell: {
      type: Number,
      required: true,
    },
    selected: {
      type: Boolean,
      required: false,
      default: false,
    },
    closed: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
};
</script>

<style scoped>
.board-item {
  border: 1px solid #2c3e50;
  width: 48px;
  height: 48px;
  position: relative;
}

.icon {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
}

.icon-1,
.icon-2,
.icon-3,
.icon-4,
.icon-5,
.icon-6 {
  background-image: url("../assets/icons.png");
  background-size: 150px 150px;
}

.icon-1 {
  background-position: 0 0;
}
.icon-2 {
  background-position: 50px 0;
}

.icon-3 {
  background-position: 100px 0;
}

.icon-4 {
  background-position: 0 50px;
}
.icon-5{
  background-position: 0 100px;
}
.icon-6{
  background-position: 50px 50px;
}

.closed {
  background-color: rgb(89, 191, 157);
}

.selected {
  background-color: rgb(13, 181, 125);
}
</style>
