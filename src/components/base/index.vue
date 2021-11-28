<template>
  <v-table :rows="rows"> </v-table>
</template>

<script>
import VTable from "@/components/base/v-table";

export default {
  name: "BaseWrapper",
  components: {
    VTable,
  },
  data() {
    return {
      baseUrl: "//jsonplaceholder.typicode.com/",
      rows: [],
    };
  },
  created() {
    this.getRows();
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
  },
};
</script>

<style scoped></style>
