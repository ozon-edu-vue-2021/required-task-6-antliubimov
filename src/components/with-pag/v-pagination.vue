<template>
  <div class="pagination">
    <button
      type="button"
      class="btn"
      :disabled="page === 1"
      @click="firstPage()"
    >
      ‹
    </button>
    <button
      type="button"
      class="btn"
      :disabled="page === 1"
      @click="prevPage()"
    >
      «
    </button>
    <button
      type="button"
      :class="['btn', { 'btn--active': page === pageNumber }]"
      v-for="pageNumber in pages.slice(page - 1, page + 4)"
      :key="pageNumber"
      @click="setPage(pageNumber)"
    >
      {{ pageNumber }}
    </button>
    <button
      type="button"
      class="btn"
      :disabled="page === page.length"
      @click="nextPage()"
    >
      »
    </button>
    <button
      type="button"
      class="btn"
      :disabled="page === page.length"
      @click="lastPage()"
    >
      ›
    </button>
  </div>
</template>

<script>
export default {
  name: "v-pagination",
  props: {
    page: {
      type: Number,
      default: 1,
    },
    pages: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    prevPage() {
      this.$emit("prevPage");
    },
    nextPage() {
      this.$emit("nextPage");
    },
    setPage(pageNumber) {
      this.$emit("setPage", pageNumber);
    },
    firstPage() {
      this.$emit("firstPage");
    },
    lastPage() {
      this.$emit("lastPage");
    },
  },
};
</script>

<style scoped>
.pagination {
  margin-top: 20px;
}
.btn {
  width: 40px;
  height: 40px;
  color: #259463;
  font-size: 16px;
  border: 3px solid #259463;
  border-radius: 3px;
}
.btn:not(:last-child) {
  margin-right: 10px;
}
.btn:hover,
.btn--active {
  color: #ffffff;
  background-color: #259463;
}
.btn:disabled {
  pointer-events: none; /* <----------- */
  opacity: 0.2;
}
</style>
