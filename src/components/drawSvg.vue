<template>
  <div>
      <v-container fluid>
          <v-slide-y-transition mode="out-in">
              <v-layout column align-center>
                  <div class="paper" ref="draw"></div>
              </v-layout>
          </v-slide-y-transition>
      </v-container>
  </div>
</template>

<script>
import SVG from 'svg.js';
import 'svg.draggable.js';

let draw = null;

export default {
  name: 'drawSvg',
  props: {
    title: String,
    parentParams: {
      Type: Object,
      default() { return { radius: 30 }; },
    },
  },
  data() {
    return {
      circle: null,
      rect: null,
      params: { radius: 10 },
    };
  },
  watch: {
    parentParams(newVal, oldVal) {
      this.params = Object.assign({}, newVal);
      this.circle.animate().radius(this.params.radius);
    },
  },
  mounted() {
    this.params = Object.assign({}, this.parentParams);
    draw = SVG(this.$refs.draw);
    draw.text(this.title)
      .center(150, 50)
      .size(25);

    this.circle = draw.circle().radius(this.params.radius)
      .center(150, 150)
      .fill('#ffff1c')
      .stroke({ color: 'red', width: 10 });

    this.rec = draw.rect(200, 120)
      .center(150, 150)
      .radius(10)
      .fill('rgb(0, 200, 255)')
      .opacity(0.5)
      .draggable();
  },
};
</script>

<style scoped lang="scss">
    .paper {
        background: white;
        border: 1px solid black;
        min-height: 400px;
        min-width: 400px;
    }
    .info {
        background: #cfc8cd;
    }
</style>
