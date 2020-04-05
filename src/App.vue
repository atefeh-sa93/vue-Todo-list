<template>
  <div>
    <div id="app">
      <Home />
    </div>
  </div>
</template>
<script>
import Home from "@/views/Home";

export default {
  name: "App",
  // eslint-disable-next-line vue/no-unused-components
  components: { Home }
};
</script>
<style>
@import "./assets/Style.css";
</style>
