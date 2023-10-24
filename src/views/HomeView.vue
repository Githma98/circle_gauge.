<template>
  <div class="home">
    <Gauge :intervals="intervals" :value="currentValue" :size="gaugeSize" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import Gauge from "@/components/Gauge.vue"; // @ is an alias to /src

export default defineComponent({
  name: "HomeView",
  components: {
    Gauge,
  },
  setup() {
    const intervals = [
      { value: 0, color: "#469ED5" },
      { value: 50, color: "#75C952" },
      { value: 100, color: "#E1CA46" },
    ];
    const currentValue = ref(50);
    const gaugeSize = 200; // Set the gauge size

    return {
      intervals,
      currentValue,
      gaugeSize,
    };
  },
});
</script>
