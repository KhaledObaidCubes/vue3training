<template>
  <div>
    <h3>sum = {{ sum }}</h3>
  </div>
  <div>
    <adder @sendValu="showSumRes" @resetSum="sum = 0" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import adder from "./assets/components/counter-calculator/add-num.vue";

const sum = ref(0);
const showSumRes = (valueFromEvent) => {
  console.log("this function called by the event!");
  sum.value += valueFromEvent;
};
</script>
