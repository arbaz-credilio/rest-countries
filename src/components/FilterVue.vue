<template>
  <select v-model="vmodel" @change="$emit(emit, vmodel)">
    <option value="">select {{ filterName }}</option>
    <option v-for="opt in options" :key="opt" :value="opt">
      {{ opt }}
    </option>
  </select>
</template>
<script setup>
import { ref, defineProps } from "vue";
const props = defineProps({
  filterName: String,
  vModelValue: String,
  emit: String,
  options: Array,
});
const vmodel = ref(props.vModelValue);
</script>
