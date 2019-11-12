<template>
  <div id="app">
    <div style="margin-bottom: 1%;" class="title"> Warpzone Level Builder</div>
        <div class="instructions">
      Left click to create blocks
      <br>
      Right click to delete blocks
      <br>
      left/right click and drag to be quick
    </div>
      <div class="credits">
      Made by Giulio Rossi
      <br>
      <a href="https://ciuffi.dev" target="_blank" rel="noopener"> Ciuffi.dev
      </a>
    </div>
    <div class="buttons">
      <button class="button is-info is-light" @click="upload">📁 Import</button>
      <button class="button is-success is-light" @click="save">💾 Save</button>
      <button class="button is-danger is-light" @click="clear">❌ Clear</button>
      <input ref="upload" style="display: none;" type="file" @change="onFileChanged"/>
    </div>
    <Grid ref="grid"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import VueCompositionApi from '@vue/composition-api';
import HelloWorld from './components/HelloWorld.vue';
import Grid from './components/Grid.vue';

enum Mode {
  add = 'add',
  delete = 'delete'
}

Vue.use(VueCompositionApi);
export default Vue.extend({
  name: 'app',
  components: {
    Grid,
  },
  methods: {
    save() {
      const gridComponent: any = this.$refs.grid;
      const data = gridComponent.grid;
      const json = JSON.stringify(data);
      const blob = new Blob([json], { type: 'text/plain' });
      const a = document.createElement('a');
      a.download = 'level.json';
      a.href = window.URL.createObjectURL(blob);
      a.click();
      document.removeChild(a);
    },
    clear() {
      if (confirm('\tAre you sure you want to clear?\t\n\t\tThis cannot be undone.')) {
        (this.$refs.grid as Vue & {createGrid: Function}).createGrid();
        localStorage.removeItem('grid');
      }
    },
    upload() {
      const uploadComponent: any = this.$refs.upload;
      uploadComponent.click();
    },
    onFileChanged(e: any) {
      const reader = new FileReader();
      const { grid } = this.$refs;
      const { files } = e.target;
      if (!files.length) return;
      reader.onload = (event: any) => {
        (grid as Vue & {grid: Array<Array<Number>>}).grid = JSON.parse(event.target.result);
      };
      reader.readAsText(files[0]);
    },
  },
});
</script>

<style lang="scss">
@import '~bulma/bulma';
#app {
  overflow-y: hidden;
  text-align: center;
  height: 100vh;
}
.button {
  margin: auto;
}
.instructions {
  position: absolute;
  top: 1%;
  left: 1%;
  font-weight: bold;
}
.credits {
  position: absolute;
  top: 0%;
  right: 1%;
  font-weight: bold;
  font-size: 1.8em;
}
.buttons {
  width: 35%;
  margin: 0.2% auto;
  display: flex;
}
@include until($tablet){
  #app {
      overflow-y: visible;
  }
  .instructions {
    top: none;
    left: none;
    position: relative;
    font-size: 1em;
  }
  .credits {
    top: none;
    left: none;
    position: relative;
    font-size: 1em;
  }
  .buttons {
    width: 100%;
  }
 }
</style>
