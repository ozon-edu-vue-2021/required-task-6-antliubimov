<template>
  <div id="app">
<!--        <base-wrapper></base-wrapper>-->
    <pag-wrapper></pag-wrapper>
  </div>
</template>

<script>
// import BaseWrapper from "@/components/base";
import PagWrapper from "@/components/with-pag";

export default {
  name: "App",
  components: {
    // BaseWrapper,
    PagWrapper,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
