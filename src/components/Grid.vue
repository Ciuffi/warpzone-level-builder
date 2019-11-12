<template>
  <div @mousedown.prevent="mouseDown = true" @mouseup.prevent="mouseUp" class="grid">
      <div class="is-desktop is-mobile columns" v-for="(row, index) in grid" :key="index">
          <div v-for="(cell, cellIndex) in row" :key="cellIndex">
              <Square :drag="drag" :clicked="clicked"
              :row="index"
              :col="cellIndex"
              :blockType="cell"/>
          </div>
      </div>
  </div>
</template>

<script lang="ts">
/* eslint no-restricted-globals:0 */
import Vue from 'vue';
import Square from './Square.vue';

const GRID_HEIGHT = 13;
const GRID_WIDTH = 64;
const GRID_MIDDLE = 6;
const unTouchable = (x: Number): Boolean => x === GRID_MIDDLE || x === 0 || x === GRID_HEIGHT - 1;

export default Vue.extend({
  components: { Square },
  methods: {
    createGrid() {
      const arr = [];
      for (let i = 0; i < GRID_HEIGHT; i += 1) {
        if (unTouchable(i)) {
          arr.push(new Array(GRID_WIDTH).fill(1));
        } else {
          arr.push(new Array(GRID_WIDTH).fill(0));
        }
      }
      this.grid = arr;
    },
    mouseUp() {
      this.mouseDown = false;
      localStorage.setItem('grid', JSON.stringify(this.grid));
    },
    clicked(event: MouseEvent, x: number, y: number) {
      if (unTouchable(x)) return;
      const newGrid = [...this.grid[x]];
      newGrid[y] = event.button === 0 ? 1 : 0;
      this.grid.splice(x, 1, newGrid);
    },
    drag(event: MouseEvent, x: number, y: number) {
      if (!this.mouseDown) return;
      this.clicked(event, x, y);
    },
  },
  data() {
    const grid: Array<Array<Number>> = [];
    return {
      grid,
      mouseDown: false,
    };
  },
  mounted() {
    const grid = localStorage.getItem('grid');
    if (grid !== null) { this.grid = JSON.parse(grid); } else { this.createGrid(); }
  },
});
</script>

<style>
.grid {
  overflow-x: scroll;
  height: 90vh;
  overflow-y: hidden;
  margin-bottom: 0%;
  padding-bottom: 0%;
}
</style>
