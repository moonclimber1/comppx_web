<template>
  <div class="synthwave">
    <div class="scene">
      <p class="typewriter"></p>
      <img class="oldcomputer" src="@/assets/oldcomputer.png" alt="" />
      <video autoplay="autoplay">
        <source src="@/assets/edited.mp4" type="video/mp4" />
      </video>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import { gsap } from "gsap";
import Typed from "typed.js";
// import { CustomEase } from "gsap/CustomEase";
// import { ScrollTrigger } from "gsap/ScrollTrigger";

export default {
  name: "Synthwave",
  components: {},
  mounted() {
    
    let scale, imgHeight

    function scaleToWindow() {
      console.log("scaledToWindow");
      imgHeight = gsap.getProperty(".scene", "height");
      const scaleWidth = window.innerWidth / gsap.getProperty(".scene", "width");
      const scaleHeight = window.innerHeight / imgHeight;
      scale = Math.max(scaleWidth, scaleHeight);
      gsap.set(".scene", { scale: scale * 1.2, y: imgHeight * scale * 0.1 });
    }

    const tl = gsap.timeline();
    tl.add(gsap.delayedCall(0.2, scaleToWindow));

    const videoScaleWidth = window.innerWidth / gsap.getProperty("video", "width");
    const videoScaleHeight = window.innerHeight / gsap.getProperty("video", "height");

    const videoScale = Math.max(videoScaleWidth, videoScaleHeight);

    tl.to("body", { duration: 8.1 });
    tl.to(".scene", { duration: 15, x: -38, y: 900, scale: videoScale * 1.33, ease: "power2.inOut" });
    tl.to("body", { duration: 59 });
    tl.call(scaleBackAnimation);
    
    function scaleBackAnimation(){
        console.log("back animation called");
        gsap.to(".scene", { duration: 8, x: 0, y: 0, scale: scale,  ease: "power2.inOut" });
    }

    var options = {
      strings: ["Loading floppy disk...", "Starting Time Machine...", "Turn sound on", "Go wild", ""],
      typeSpeed: 110,
    };
    var typed = new Typed('.typewriter', options);

  },
};
</script>

<style lang="scss" scoped>
.synthwave {
  position: relative;
  height: 100vh;
  overflow: hidden;

  .typewriter {
    position: absolute;
    top: 15%;
    width: 100%;
    z-index: 100;
    padding: 0 30%;

    font-size: 12vmax;
    color: white;
    text-transform: uppercase;
  }

  .scene {
    width: 10000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    .oldcomputer {
      width: 100%;
    }
    video {
      position: absolute;
      top: 29.5%;
      left: 39.7%;
      width: 21%;
      z-index: -1;
      transform: scaleX(0.9);
    }
  }
}
</style>
