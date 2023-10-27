<template>
  <div>
    <input type="number" v-model="inputValue" />
    <button @click="adderEvnt('sendValu', inputValue), reset()">+</button>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
//defineProps({inputValue: Number})
const inputValue = ref();
const adderEvnt = defineEmits({ sendValu: (value) => value });
const reset = () => {
  inputValue.value = "";
};
</script>
