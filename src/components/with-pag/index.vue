<template>
  <div>
    <v-table
      :displayedRows="displayedRows"
      :rows="rows"
      @firstPage="firstPage"
      @my-new-rows="setNewCurrRows($event)"
    ></v-table>
    <v-pagination
      :page="activePage"
      :pages="pages"
      @prevPage="prevPage()"
      @nextPage="nextPage()"
      @setPage="setPage($event)"
      @firstPage="firstPage()"
      @lastPage="lastPage()"
    ></v-pagination>
  </div>
</template>

<script>
import VTable from "@/components/with-pag/v-table";
import VPagination from "@/components/with-pag/v-pagination";

export default {
  name: "PagWrapper",
  components: {
    VTable,
    VPagination,
  },
  data() {
    return {
      baseUrl: "https://jsonplaceholder.typicode.com/",
      rows: [],
      currRows: [],
      activePage: 1,
      perPage: 10,
      pages: [],
    };
  },
  created() {
    this.getRows();
  },
  computed: {
    displayedRows() {
      this.setPages();
      return this.paginate(this.currRows);
    },
  },
  methods: {
    async getRows() {
      try {
        const res = await fetch(`${this.baseUrl}comments`);
        this.rows = await res.json();
        this.currRows = this.rows;
      } catch (e) {
        console.log(e);
      }
    },
    setPages() {
      let numberOfPages = Math.ceil(this.rows.length / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },
    paginate(rows) {
      let page = this.activePage;
      let perPage = this.perPage;
      let fromRow = page * perPage - perPage;
      let toRow = page * perPage;
      return rows.slice(fromRow, toRow);
    },
    setNewCurrRows(rows) {
      this.currRows = rows;
    },
    prevPage() {
      this.activePage--;
    },
    nextPage() {
      this.activePage++;
    },
    setPage(pageNumber) {
      this.activePage = pageNumber;
    },
    firstPage() {
      this.activePage = 1;
    },
    lastPage() {
      this.activePage = this.pages.length;
    },
  },
};
</script>

<style scoped></style>
