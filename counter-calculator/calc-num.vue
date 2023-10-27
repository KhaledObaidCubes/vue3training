<template>
  <div>
    <h3>sum ={{ sumProp }}</h3>
  </div>
</template>

<script setup>
import { defineProps } from "vue";
defineProps({ sumProp: Number });
</script>
