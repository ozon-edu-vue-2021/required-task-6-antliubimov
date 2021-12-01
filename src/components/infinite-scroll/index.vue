<template>
  <div>
    <div v-if="isLoading" id="loading">
      <div class="loadingio-spinner-spinner-xeae5viqv2b">
        <div class="ldio-6t03d05r6cb">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
    </div>
    <v-table
      v-show="!isLoading"
      :displayedRows="displayedRows"
      :rows="rows"
      @my-new-rows="setNewCurrRows($event)"
    ></v-table>
  </div>
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
      isLoading: true,
    };
  },
  beforeMount() {
    this.visibleRows = Math.ceil(window.innerHeight / this.rowHeight);
    this.getRows();
    this.getAllRows();
  },
  mounted() {
    setTimeout(() => {
      this.isLoading = false;
    }, 2000);
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

<style scoped>
@keyframes ldio-6t03d05r6cb {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.ldio-6t03d05r6cb div {
  left: 132px;
  top: 67px;
  position: absolute;
  animation: ldio-6t03d05r6cb linear 1s infinite;
  background: #6492ac;
  width: 16px;
  height: 33px;
  border-radius: 8px / 16px;
  transform-origin: 8px 73px;
}

.ldio-6t03d05r6cb div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -0.9166666666666666s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -0.8333333333333334s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.75s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.6666666666666666s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.5833333333333334s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.5s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.4166666666666667s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.3333333333333333s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.25s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.16666666666666666s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.08333333333333333s;
  background: #6492ac;
}

.ldio-6t03d05r6cb div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
  background: #6492ac;
}

.loadingio-spinner-spinner-xeae5viqv2b {
  width: 281px;
  height: 281px;
  display: inline-block;
  overflow: hidden;
  background: #ffffff;
}

.ldio-6t03d05r6cb {
  width: 100%;
  height: 100%;
  position: relative;
  transform: translateZ(0) scale(1);
  backface-visibility: hidden;
  transform-origin: 0 0; /* see note above */
}

.ldio-6t03d05r6cb div {
  box-sizing: content-box;
}

/* generated by https://loading.io/ */
</style>
