<template>
  <v-table
    :displayedRows="displayedRows"
    :rows="rows"
    @my-new-rows="setNewCurrRows($event)"
  ></v-table>
</template>

<script>
import VTable from "@/components/infinite-scroll/v-table";

export default {
  name: "InfiniteScrollWrapper",
  components: {
    VTable,
  },
  data() {
    return {
      baseUrl: "//jsonplaceholder.typicode.com/",
      rows: [],
      currRows: [],
      filteredSortedRows: [],
      visibleRows: 0,
      tmpVisibleRows: 0,
      rowHeight: 59,
      start: 0,
    };
  },
  beforeMount() {
    this.visibleRows = Math.ceil(window.innerHeight / this.rowHeight);
    this.getRows();
    this.getAllRows();
  },
  mounted() {
    this.getNextRows();
  },
  computed: {
    displayedRows() {
      return this.currRows;
    },
  },
  methods: {
    async getAllRows() {
      try {
        const res = await fetch(`${this.baseUrl}comments`);
        this.rows = await res.json();
      } catch (e) {
        console.log(e);
      }
    },
    async getRows() {
      let tmpRows = [];

      for (let i = 1; i <= this.visibleRows; i++) {
        try {
          const res = await fetch(`${this.baseUrl}comments/${i}`);
          let row = await res.json();
          tmpRows.push(row);
        } catch (e) {
          console.log(e);
        }
      }
      this.currRows = [...tmpRows];
    },
    getNextRows() {
      window.onscroll = async () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          this.visibleRows += 1;
          try {
            const res = await fetch(
              `${this.baseUrl}comments/${this.visibleRows}`
            );
            let row = await res.json();
            this.currRows.push(row);
          } catch (e) {
            console.log(e);
          }
        }
      };
    },
    async getFilteredSortedRows() {
      let tmpRows = [];
      for (let i = 0; i < this.tmpVisibleRows; i++) {
        try {
          const res = await fetch(
            `${this.baseUrl}comments/${this.filteredSortedRows[i]}`
          );
          let row = await res.json();
          tmpRows.push(row);
        } catch (e) {
          console.log(e);
        }
      }
      this.currRows = [...tmpRows];
      window.onscroll = this.getNextFilteredSortedRows;
    },
    getNextFilteredSortedRows() {
      window.onscroll = async () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          try {
            const res = await fetch(
              `${this.baseUrl}comments/${
                this.filteredSortedRows[this.tmpVisibleRows]
              }`
            );
            let row = await res.json();
            this.currRows.push(row);
            this.tmpVisibleRows += 1;
          } catch (e) {
            console.log(e);
          }
        }
      };
    },
    setNewCurrRows(rows) {
      let tmpRows = [];
      this.tmpVisibleRows = this.visibleRows;
      rows.forEach((row) => tmpRows.push(row.id));
      this.filteredSortedRows = [...tmpRows];
      this.getFilteredSortedRows();
    },
  },
};
</script>

<style scoped></style>
