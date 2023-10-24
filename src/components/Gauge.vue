<template>
  <div class="gauge">
    <svg :width="size" :height="size">
      <defs>
        <linearGradient :id="gradientId">
          <stop
            v-for="(interval, index) in intervals"
            :key="index"
            :offset="(interval.value * 2) / 3 + '%'"
            :style="{ stopColor: interval.color }"
          />
        </linearGradient>
      </defs>
      <circle
        cx="50%"
        cy="50%"
        :r="size / 2 - 10"
        :style="{ stroke: 'url(#' + gradientId + ')', strokeWidth: '20' }"
        fill="transparent"
        :stroke-dasharray="((circumference * 2) / 3, circumference)"
        :stroke-dashoffset="circumference / 2.5"
        :transform="rotateTransform"
      />
      <line
        x1="50%"
        y1="50%"
        :x2="pointerX"
        :y2="pointerY"
        :style="{ stroke: 'black', strokeWidth: '4' }"
      />
      <text x="50%" y="80%" text-anchor="middle" font-size="24">
        {{ value }}%
      </text>
      <text x="10%" y="65%" text-anchor="middle" font-size="20" fill="black">
        0
      </text>
      <text x="50%" y="10%" text-anchor="middle" font-size="20" fill="black">
        50
      </text>
      <text x="100%" y="60%" text-anchor="middle" font-size="20" fill="black">
        100
      </text>
    </svg>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

interface IntervalItem {
  value: number;
  color: string;
}

export default defineComponent({
  name: "CircularGauge",
  props: {
    intervals: {
      type: Array as PropType<IntervalItem[]>,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },
    size: {
      type: Number,
      required: true,
    },
  },
  computed: {
    gradientId() {
      return "gradient-" + Math.floor(Math.random() * 10000);
    },
    circumference() {
      return Math.PI * (this.size / 2 - 10) * 2;
    },
    pointerX() {
      // Calculate the x-coordinate of the pointer based on the value
      const angle = (this.value / 100) * 360;
      return 50 + 40 * Math.cos((angle - 90) * (Math.PI / 180));
    },
    pointerY() {
      // Calculate the y-coordinate of the pointer based on the value
      const angle = (this.value / 100) * 360;
      return 50 + 40 * Math.sin((angle - 90) * (Math.PI / 180));
    },
    rotateTransform() {
      // Rotate the circle to have the removed part at the bottom
      return `rotate(160 100 100)`;
    },
  },
});
</script>

<style scoped>
.gauge {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
