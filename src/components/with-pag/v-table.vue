<template>
  <table class="table">
    <thead>
      <tr>
        <th v-for="column in Object.entries(tableColumns)" :key="column[0]">
          <div @click="toggleSort(column[0])">
            {{ column[1] }}
            <span class="arrow" :class="sortDirections[column[0]]"></span>
          </div>
          <div>
            <input type="text" @input="filterInputText($event, column[0])" />
          </div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in displayedRows" :key="item.id">
        <td
          v-for="column in Object.keys(tableColumns)"
          :key="item[column] + Math.random()"
        >
          <a v-if="column === 'email'" :href="`mailto:${item[column]}`">{{
            item[column]
          }}</a>
          <span v-else>{{ item[column] }}</span>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import { orderBy } from "lodash/collection";

export default {
  name: "VTable",
  props: {
    rows: {
      type: Array,
      default: () => [],
    },
    displayedRows: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      tableColumns: {
        id: "ID",
        postId: "Post ID",
        name: "Name",
        email: "Email",
      },
      sortKeys: [],
      sortProps: [],
      sortDirections: {
        id: "",
        postId: "",
        name: "",
        email: "",
      },
      filterProps: [],
      filterText: {
        id: "",
        postId: "",
        name: "",
        email: "",
      },
    };
  },
  computed: {},
  methods: {
    sortedRows() {
      let res;

      if (!this.sortProps.length && !this.filterProps.length) {
        this.pag(this.rows);
      } else {
        if (this.sortProps.length) {
          let directions = [];
          this.sortProps.forEach((props) => {
            if (this.sortDirections[props]) {
              directions.push(this.sortDirections[props]);
            }
          });
          res = orderBy(this.rows, this.sortProps, directions);
        }

        if (this.filterProps.length) {
          if (res) {
            res = this.filterRows(res);
          } else {
            res = this.filterRows(this.rows);
          }
        }
        this.pag(res);
      }
    },
    toggleSort(column) {
      this.firstPage();
      this.sortDirections[column] =
        this.sortDirections[column] === "desc" ? "asc" : "desc";
      if (!this.sortProps.includes(column)) {
        this.sortProps.push(column);
      }
      this.sortedRows();
    },
    filterInputText(e, column) {
      this.firstPage();
      this.filterProps.push(column);
      this.filterText[column] = e.target.value;
      this.sortedRows();
    },
    filterRows(rows) {
      this.filterProps.forEach((prop) => {
        {
          rows = rows.filter(
            (row) =>
              String(row[prop]).search(this.filterText[prop].toLowerCase()) > -1
          );
        }
      });
      return rows;
    },
    firstPage() {
      this.$emit("firstPage");
    },
    pag(rows) {
      this.$emit("my-new-rows", rows);
    },
  },
};
</script>

<style scoped>
.table {
  border: 3px solid #259463;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: #259463;
  color: #ffffff;
  cursor: pointer;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}
th,
td {
  min-width: 75px;
  padding: 10px 20px;
}

.arrow {
  display: inline-block;
  position: relative;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}
.arrow:after {
  content: "↑↓";
  position: absolute;
  top: -18px;
  left: -7px;
  font-size: 20px;
}

.arrow.asc:after {
  content: "↓";
}

.arrow.desc:after {
  content: "↑";
}
</style>
