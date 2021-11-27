<template>
  <v-table :rows="rows">

  </v-table>
</template>

<script>
import VTable from '@/components/v-table';

export default {
  name: "Wrapper",
  components: {
    VTable
  },
  data() {
    return {
      rows: []
    }
  },
  created() {
    this.getRows();
  },
  methods: {
    async getRows() {
      try {
        const res = await fetch(`https://jsonplaceholder.typicode.com/comments`);
        this.rows = await res.json();
      } catch (e) {
        console.log(e);
      }

    }
  }
}
</script>

<style scoped>

</style>