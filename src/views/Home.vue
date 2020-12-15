<template>
  <div class="home">
    <div id="canvas-wrapper" class="canvas-wrapper" v-bind:class="{ visible: hover }"></div>
    <a href="/#/synthwave">
      <img src="@/assets/floppydisk.png" alt="Floppy Disk" @mouseover="hover = true" @mouseleave="hover = false" />
    </a>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import { gsap } from "gsap";
import P5 from "p5";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      hover: false,
    };
  },
  created() {},
  mounted() {
    const sketch = (s) => {
      let w = window.innerWidth;
      let h = window.innerHeight;

      let rotation = 0;

      s.setup = () => {
        s.createCanvas(w, h);
      };

      s.draw = () => {
        if (this.hover) {
          rotation += 0.006;
          s.translate(window.innerWidth / 2, window.innerHeight / 2);
          s.rotate(rotation);
          s.drawStripes();
        } else {
          s.background(33, 33, 33);
        }
      };

      s.drawStripes = () => {
        s.noStroke();
        const points = s.getCirlcePoints(Math.max(window.innerHeight, window.innerWidth));

        points.forEach((point, index) => {
          const nextPointIndex = (index + 1) % points.length;
          const nextPoint = points[nextPointIndex];

          if (index % 2 == 0) {
            s.fill(s.color("#DC3A2F"));
          } else {
            s.fill("#F3F4F6");
          }

          s.triangle(0, 0, point.x, point.y, nextPoint.x, nextPoint.y);
        });
      };

      s.getCirlcePoints = (radius) => {
        const points = [];
        for (let a = 0; a <= s.TWO_PI; a += 0.157) {
          const x = s.cos(a) * radius;
          const y = s.sin(a) * radius;
          points.push(s.createVector(x, y));
        }
        return points;
      };
    };
    let canvas = new P5(sketch, "canvas-wrapper");
  },
};
</script>

<style lang="scss" scoped>
.home {
  position: relative;
  height: 100vh;
  overflow: hidden;
  background: #212121;

  img {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    transition: transform 0.6s;
    width: 20vmax;

    &:hover {
      transform: translate(-50%, -50%) scale(1.3);
    }
  }

  .canvas-wrapper {
    opacity: 0;
    transition: opacity 3s;
  }

  .visible {
    opacity: 1;
  }
}
</style>
