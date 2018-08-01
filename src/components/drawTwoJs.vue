<template>
  <div class="hello">
       <div id="draw" class="paper" ref="draw"></div>
  </div>
</template>

<script>
import Two from 'two.js';

export default {
  name: 'drawTwoJs',
  props: {
    title: String,
    radius: Number,
  },
  data() {
    return {
      two: null,
      circle: null,
    };
  },
  watch: {
    radius(newVal, oldVal) { // watch it
      console.log('drawTwojs radius changed: ', newVal, ' | was: ', oldVal);
      this.circle.radius = newVal;
      this.two.update();
    },
  },
  mounted() {
    const two = new Two({ width: 300, height: 300 }).appendTo(this.$refs.draw);
    this.two = two;
    console.log(two);

    const text = new Two.Text(this.title, 150, 50);
    text.size = 25;
    two.add(text);
    const circle = two.makeCircle(150, 150, this.radius);
    this.circle = circle;
    circle.r = 100;
    circle.fill = '#ffff1c';
    circle.stroke = 'red';
    circle.linewidth = 10;
    console.log(circle);

    const rect = two.makeRoundedRectangle(150, 150, 200, 120, 10);
    rect.fill = 'rgb(0, 200, 255)';
    rect.opacity = 0.5;
    rect.noStroke();

    // Don't forget to tell two to render everything
    // to the screen
    two.update();
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
