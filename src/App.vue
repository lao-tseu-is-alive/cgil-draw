<template>
  <div id="app">
    <div class="controls">
      <input type="number" v-model="radius"/>
    </div>
    <div class="container">
      <draw-svg class="drawing" :radius="parseInt(radius)" title="Svg.js"></draw-svg>
      <draw-two-js class="drawing" :radius="parseInt(radius)" title="Two.js"/>
    </div>
  </div>
</template>

<script>
import drawTwoJs from './components/drawTwoJs.vue';
import drawSvg from './components/drawSvg.vue';

export default {
  name: 'app',
  components: {
    drawTwoJs,
    drawSvg,
  },
  data() {
    return {
      radius: 50,
    };
  },
};
</script>

<style lang="scss">
  .container {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
  }
  .drawing {
    background: #ffffff;
    color: yellow;
    display: table-cell;
    flex: 1;
    padding: 10px;
    min-width: 300px;
    min-height: 300px;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: aqua;
  color: #2c3e50;
  margin-top: 30px;
  min-width: 330px;
}
</style>
