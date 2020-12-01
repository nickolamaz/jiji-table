<template>
  <div>
    <div>
      <label for="search">Search:</label>
      <input type="text" id="search" v-model="querySearch">
    </div>
    <table v-if="tableData && tableHeaders">
      <thead>
      <tr>
        <th v-for="(tableCol, idx) in tableHeaders" :key="idx" @click="sortBy(tableCol)">
          {{tableCol.title}}
          <span v-if="tableCol.title">
            <i
                class="arrow up"
                v-if="!sortReverse && tableCol.code === sortKey"
            ></i>
            <i class="arrow down"
                v-if="sortReverse && tableCol.code === sortKey"
            ></i>
          </span>
        </th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(tableRow, idx) in paginatedTableRows" :key="idx">
        <td v-for="(tableCol, idx2) in tableHeaders" :key="idx2">
          {{getCellContent(tableRow, tableCol)}}
        </td>
      </tr>
      </tbody>
    </table>
    <div v-else>
      No table data provided
    </div>
    <table-pagination
        ref="tablePaginationRef"
        :page-limit="sortedTableDataRows.length"
        :page-size="pageSize"
        @goToPage="goToPageHandler"
    >
    </table-pagination>
  </div>
</template>

<script>
import initialJsonData from '@/const/initial-data.json';
import TablePagination from '@/components/TablePagination';

export default {
  name: 'TableWrapper',
  components: { TablePagination },
  props: {
    tableData: {
      type: Object,
      required: false,
      default() {
        return initialJsonData;
      },
    },
    pageSize: {
      required: false,
      type: Number,
      default: 10,
    }
  },
  data() {
    return {
      tableHeaders: null,
      querySearch: null,
      pageStartIndex: 0,
      sortReverse: false,
      sortKey: '',
    }
  },
  created() {
    this.getTableData();
  },
  computed: {
    paginatedTableRows() {
      return this.sortedTableDataRows.slice(this.pageStartIndex, this.pageStartIndex + this.pageSize);
    },
    sortedTableDataRows() {
      if (!this.tableData.items) {
        return [];
      }
      const origArr = this.tableData.items.slice();
      let sortedArr = origArr.sort((a, b) => {
        let sortA = a[this.sortKey];
        let sortB = b[this.sortKey];
        if (!isNaN(sortA)) {
          sortA = parseInt(sortA)
        }
        if (!isNaN(sortB)) {
          sortB = parseInt(sortB)
        }
        if (sortA < sortB) {
          return -1;
        }
        if (sortA > sortB) {
          return 1;
        }
        return 0;
      });
      sortedArr = this.filteredTableDataRows(sortedArr);
      sortedArr = this.sortReverse ? sortedArr.reverse() : sortedArr;
      return sortedArr;
    },
  },
  methods: {
    getTableData() {
      if (!(this.tableData && this.tableData.titles && this.tableData.items)) {
        return;
      }
      this.tableHeaders = Object.keys(this.tableData.titles).map((td) => {
        return { code: td, title: this.tableData.titles[td] };
      });
      // Update each object with id to have ability to modify filtered array
      let updatedItems = [...this.tableData.items];
      updatedItems = updatedItems.map((i, idx) => {
        const newObj = Object.assign({}, i);
        newObj.id = idx;
        return newObj;
      });
      this.$set(this.tableData, 'items', updatedItems.slice());
    },
    getCellContent(row, col) {
      return row[col.code];
    },
    goToPageHandler(event) {
      this.pageStartIndex = event * this.pageSize - this.pageSize;
    },
    sortBy(sortObj) {
      this.sortReverse = this.sortKey === sortObj.code ? !this.sortReverse : false;
      this.sortKey = sortObj.code;
      this.$refs.tablePaginationRef.goToPage(1);
    },
    filteredTableDataRows(sortedArr) {
      return sortedArr.filter((i) => {
        if (!this.querySearch) {
          return i;
        }
        return Object.keys(i).some(k => i[k].toString().toLowerCase().includes(this.querySearch.toLowerCase()));
      });
    }
  }
}
</script>

<style>
th, td {
  padding: .5rem;
}
th {
  cursor: pointer;
}
.arrow {
  border: solid black;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}

.up {
  transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
}

.down {
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}
</style>
