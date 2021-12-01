<template>
  <div id="app">
    <!--        <base-wrapper></base-wrapper>-->
    <!--    <pag-wrapper></pag-wrapper>-->
    <!--    <virt-scroll-wrapper></virt-scroll-wrapper>-->
    <infinite-scroll-wrapper></infinite-scroll-wrapper>
  </div>
</template>

<script>
// import BaseWrapper from "@/components/base";
// import PagWrapper from "@/components/with-pag";
// import VirtScrollWrapper from "@/components/virt-scroll";
import InfiniteScrollWrapper from "@/components/infinite-scroll";

export default {
  name: "App",
  components: {
    // BaseWrapper,
    // PagWrapper,
    // VirtScrollWrapper,
    InfiniteScrollWrapper,
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
  margin-top: 0px;
}
</style>
