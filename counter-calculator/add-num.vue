<template>
  <div>
    <input type="number" v-model="inputValue" />
    <button @click="adderEvnt('sendValu', inputValue), reset()">+</button>
    <button @click="resetSum">Reset Sum</button>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
const inputValue = ref();
const adderEvnt = defineEmits({ sendValu: (value) => value }, "resetSum");
const reset = () => {
  inputValue.value = "";
};
const resetSum = () => adderEvnt("resetSum");
</script>
