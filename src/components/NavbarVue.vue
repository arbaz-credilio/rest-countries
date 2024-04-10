<template>
  <nav :class="darkMode && 'dark-navbar'">
    <h1>Where in the world?</h1>
    <button @click="handleMode">
      {{ darkMode ? "Dark Mode" : "Light Mode" }}
    </button>
  </nav>
</template>
<script setup>
import { ref, defineEmits } from "vue";
let darkMode = ref(false);
const emit = defineEmits(["color-mode"]);
const handleMode = () => {
  emit("color-mode", !darkMode.value);
  darkMode.value = !darkMode.value;
};

</script>
