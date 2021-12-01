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
      <tr v-for="item in displayedRows" :key="item.id + new Date().getTime()">
        <td
          v-for="column in Object.keys(tableColumns)"
          :key="column + new Date().getTime()"
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
      sortOrders: this.rows,
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
    computedRows() {
      let res;

      if (!this.sortProps.length && !this.filterProps.length) {
        this.setNewRows(this.rows);
      } else {
        if (this.sortProps.length) {
          res = this.sortedRows();
        }

        if (this.filterProps.length) {
          res = res ? this.filterRows(res) : this.filterRows(this.rows);
        }

        this.setNewRows(res);
      }
    },
    toggleSort(column) {
      this.sortDirections[column] =
        this.sortDirections[column] === "desc" ? "asc" : "desc";
      if (!this.sortProps.includes(column)) {
        this.sortProps.push(column);
      }
      this.computedRows();
    },
    filterInputText(e, column) {
      if (!this.filterProps.includes(column)) {
        this.filterProps.push(column);
      }
      this.filterText[column] = e.target.value;
      if (this.filterText[column] === "") {
        this.filterProps.splice(this.filterProps.indexOf(column), 1);
      }
      this.computedRows();
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
    sortedRows() {
      let directions = [];
      this.sortProps.forEach((props) => {
        if (this.sortDirections[props]) {
          directions.push(this.sortDirections[props]);
        }
      });
      return orderBy(this.rows, this.sortProps, directions);
    },
    setNewRows(rows) {
      this.$emit("my-new-rows", rows);
    },
  },
};
</script>

<style scoped>
.table {
  width: 100%;
  border-collapse: collapse;
  border: 3px solid #259463;
  border-radius: 3px;
  background-color: #fff;
  text-align: left;
}
tr {
  height: 59px;
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
  box-sizing: border-box;
  min-width: 75px;
  padding: 10px 20px;
  border: 1px solid lightgray;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
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
