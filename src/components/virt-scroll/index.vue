<template>
  <div
    class="container"
    :style="{ height: viewportHeight + 'px', overflow: 'auto' }"
    @scroll="scrollRows($event)"
  >
    <div
      :style="{ height: totalContentHeight + 'px', overflow: 'hidden' }"
    ></div>

    <div :style="{ transform: 'translateY(' + offsetY + 'px)' }">
      <v-table :rows="displayedRows" :offsetY="offsetY"></v-table>
    </div>
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
    this.getRows();
  },
  computed: {
    displayedRows() {
      return this.rows.slice(this.start, this.start + this.visibleRows + 1);
    },
    viewportHeight() {
      return this.rowHeight * this.visibleRows;
    },
    totalContentHeight() {
      return this.rowHeight * this.rows;
    },
    offsetY() {
      return this.start * this.rowHeight;
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
      this.start = Math.floor(e.target.scrollTop / this.rowHeight);
    },
  },
};
</script>

<style scoped>
.container {
  overflow: auto;
}
</style>
