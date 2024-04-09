<template>
  <nav :class="darkMode && 'dark-navbar'">
    <h1>Where in the world?</h1>
    <button @click="handleMode">
      {{ darkMode ? "Dark Mode" : "Light Mode" }}
    </button>
  </nav>
</template>
<script>
export default {
  name: "NavbarVue",
  data() {
    return {
      darkMode: false,
    };
  },
  methods: {
    handleMode() {
      this.$emit("color-mode", !this.darkMode);
      this.darkMode = !this.darkMode;
    },
  },
  emits: ["color-mode"],
};
</script>
