<template>
  <tbody>
  <tr v-for="(tableRow, idx) in paginatedTableRows" :key="idx">
    <td v-for="(tableCol, idx2) in tableHeaders" :key="idx2" class="w-100">
      <span v-if="!tmpData || editRowNum !== idx">
        {{ tableRow[tableCol.code] }}
      </span>
      <input type="text" class="form-control" v-model="tmpData[tableCol.code]" v-else>
    </td>
    <td>
      <button class="btn btn-primary"
              @click="startEditRow(idx)"
              v-show="editRowNum !== idx"
              :disabled="editRowNum !== null"
      >
        Edit
      </button>
      <button class="btn btn-success"
              @click="saveRowChanges(idx)"
              v-show="editRowNum === idx">
        Apply
      </button>
    </td>
  </tr>
  </tbody>
</template>

<script>
export default {
  name: 'TableBody',
  props: {
    paginatedTableRows: {
      type: Array,
      required: true,
    },
    tableHeaders: {
      type: Array,
      required: true,
    }
  },
  data() {
    return {
      editRowNum: null,
      tmpData: null,
    }
  },
  methods: {
    startEditRow(id) {
      this.editRowNum = id;
      this.tmpData = {...this.paginatedTableRows[id]}
      this.$emit('isEditing', { state: true })
    },
    saveRowChanges() {
      const emitData = {
        state: false,
        rowData: Object.assign({}, this.tmpData)
      };
      this.$emit('isEditing', emitData);
      this.$nextTick(() => {
        this.tmpData = null;
        this.editRowNum = null;
      })
    }
  }
}
</script>
