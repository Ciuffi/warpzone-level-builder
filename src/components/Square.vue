<template>
<div @contextmenu.prevent="" @mouseover.prevent="dragged"
    @mousedown.prevent="click" :class="`square ${classTypes}`">

</div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    blockType: {
      type: Number,
      required: true,
    },
    clicked: {
      type: Function,
      required: true,
    },
    drag: {
      type: Function,
      required: true,
    },
    row: {
      type: Number,
      required: true,
    },
    col: {
      type: Number,
      required: true,
    },
  },
  methods: {
    click(event: MouseEvent) {
      this.clicked(event, this.row, this.col);
    },
    dragged(event: MouseEvent) {
      this.drag(event, this.row, this.col);
    },
  },
  computed: {
    classTypes() {
      return this.blockType === 0 ? '' : 'selected';
    },
  },
});
</script>

<style scoped>
.square {
    width: 7vh;
    height: 7vh;
    background-color: white;
    border: 1px solid black;
}
.selected {
    background-color: grey;
}
</style>
