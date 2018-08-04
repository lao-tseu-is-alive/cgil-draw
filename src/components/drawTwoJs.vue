<template>
  <div class="hello">
       <div id="draw" class="paper" ref="draw"></div>
  </div>
</template>

<script>
import Two from 'two.js';

// after some testing I stop using this library in favor of svg.js
// this lib implies to call draw.update() when values changes & it does not work well
// lack of clear documentation is also a concern

let draw = null;

export default {
  name: 'drawTwoJs',
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
      params: { radius: 10 },
    };
  },
  watch: {
    parentParams(newVal, oldVal) {
      this.params = Object.assign({}, newVal);
      console.log('Watch parentParams', this.params, newVal);
      draw.update(); // this one does not update screen...
    },
  },
  created() {
    this.$nextTick(function () {
      if (draw != null) draw.update();
      console.log('nextTick : ', this.params);
    });
  },
  mounted() {
    this.params = Object.assign({}, this.parentParams);
    this.$nextTick(function () {
      if (draw != null) draw.update();
      console.log('nextTick : ', this.params);
    });
    draw = new Two({ width: 300, height: 300 }).appendTo(this.$refs.draw);
    console.log(draw);

    const text = new Two.Text(this.title, 150, 50);
    text.size = 25;
    draw.add(text);

    const circle = draw.makeCircle(150, 150, this.params.radius);
    this.circle = circle;
    circle.r = 100;
    circle.fill = '#ffff1c';
    circle.stroke = 'red';
    circle.linewidth = 10;

    const rect = draw.makeRoundedRectangle(150, 150, 200, 120, 10);
    rect.fill = 'rgb(0, 200, 255)';
    rect.opacity = 0.5;
    rect.noStroke();

    draw.update();
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
</style>
