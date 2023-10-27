<template>
  <div>
    <calc :sumProp="sum" />
  </div>
  <div>
    <adder @sendValu="showSumRes" @resetSum="sum = 0" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import adder from "./assets/components/counter-calculator/add-num.vue";
import calc from "./assets/components/counter-calculator/calc-num.vue";

const sum = ref(0);
const showSumRes = (externalUname) => {
  console.log("this function called by the event!");
  sum.value += externalUname;
};
</script>
