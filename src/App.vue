<template>
  <the-search></the-search>
</template>

<script>
import TheSearch from "@/components/TheSearch.vue";

export default {
  name: "App",
  components: {
    TheSearch,
  },
};
</script>

<style lang="scss">
body {
  font-family: 'Space Mono', monospace;
  margin: 0;
  padding: 0;
}
</style>
