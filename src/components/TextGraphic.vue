<template>
  <div class="text-graphic" :style="{background: item.color}">
    <div class="quote" :style="{'font-size': `${item.fontSize}em`}">
      <div class="inner" :style="{'font-variation-settings': font}">«{{ item.quote }}»</div>
    </div>
    <svg :width="width" :height="height">
      <rect class="bg" :width="width * progress" :height="height"></rect>
      <text class="step" :y="height / 2" dy="0.35em" :x="width / 2">{{ step }}</text>
    </svg>
  </div>
</template>

<script>
import { scaleLinear } from "d3-scale";
export default {
  name: "text-graphic",
  props: {
    width: Number,
    height: Number,
    step: Number,
    progress: Number,
    item: Object
  },
  data() {
    return {};
  },
  computed: {
    font() {
      const { progress } = this;
      const weights = [300, 900];
      const slants = [0, -10];
      const steps = [0, 1];
      const scaleWeight = scaleLinear()
        .domain(steps)
        .range(weights);
      const scaleSlant = scaleLinear()
        .domain(steps)
        .range(slants);
      return `"wght" ${scaleWeight(progress)}, "slnt" ${scaleSlant(progress)}`;
    }
  }
};
</script>

<style scoped>
.text-graphic {
  transition: background 0.4s;
  position: relative;
}
.quote {
  position: absolute;
  width: 100%;
  height: 100%;
  color: white;
  word-break: break-all;
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}
.quote .inner {
  margin: -1.5rem;
}
svg {
  display: block;
}
</style>
