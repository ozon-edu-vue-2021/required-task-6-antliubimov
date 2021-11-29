<template>
  <div
    class="container"
    :style="{ height: blockHeight + 'px' }"
    @scroll="scrollRows($event)"
  >
    <div :style="{ height: topHeight + 'px' }"></div>
    <v-table :rows="displayedRows" :start="start"></v-table>
    <div :style="{ height: bottomHeight + 'px' }"></div>
  </div>
</template>

<script>
import VTable from "@/components/virt-scroll/v-table";

export default {
  name: "VirtScrollWrapper",
  components: {
    VTable,
  },
  data() {
    return {
      baseUrl: "//jsonplaceholder.typicode.com/",
      rows: [],
      visibleRows: 10,
      rowHeight: 59,
      start: 0,
    };
  },
  created() {
    window.addEventListener("scroll", this.scrollRows);
    this.getRows();
  },
  destroyed() {
    window.removeEventListener("scroll", this.scrollRows);
  },
  computed: {
    displayedRows() {
      return this.rows.slice(this.start, this.start + this.visibleRows + 1);
    },
    topHeight() {
      return this.rowHeight * this.start;
    },
    bottomHeight() {
      return (
        this.rowHeight *
        (this.rows.length - (this.start + this.visibleRows + 1))
      );
    },
    blockHeight() {
      return this.rowHeight * (this.visibleRows + 1);
    },
  },
  methods: {
    async getRows() {
      try {
        const res = await fetch(`${this.baseUrl}comments`);
        this.rows = await res.json();
      } catch (e) {
        console.log(e);
      }
    },
    scrollRows(e) {
      this.start = Math.min(
        this.rows.length - this.visibleRows - 1,
        Math.floor(e.target.scrollTop / this.rowHeight)
      );
    },
  },
};
</script>

<style scoped>
.container {
  overflow: auto;
}
</style>
