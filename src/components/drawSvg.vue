<template>
  <div>
       <div id="drawing" ref="draw" class="paper"></div>
  </div>
</template>

<script>
import SVG from 'svg.js';

export default {
  name: 'drawSvg',
  props: {
    title: String,
    radius: Number,
  },
  data() {
    return {
      circle: null,
    };
  },
  watch: {
    radius(newVal, oldVal) { // watch it
      console.log('drawSvg.js radius changed: ', newVal, ' | was: ', oldVal);
      this.circle.radius(newVal);
    },
  },
  mounted() {
    const draw = SVG(this.$refs.draw).size(300, 300);
    draw.text(this.title)
      .center(150, 50)
      .size(25);
    this.circle = draw.circle().radius(this.radius)
      .center(150, 150)
      .fill('#ffff1c')
      .stroke({ color: 'red', width: 10 });

    draw.rect(200, 120)
      .center(150, 150)
      .radius(10)
      .fill('rgb(0, 200, 255)')
      .opacity(0.5);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .paper {
        background: white;
        border: 1px solid black;
        width: 100%;
        height: 100%;
    }
    .hello {

    }
</style>
