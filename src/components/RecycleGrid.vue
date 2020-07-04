<template>
  <div>
    <div
      v-if="columns && columns.length"
      :style="{ overflow: 'auto' }"
    >
      <table style="width: 100%;">
        <colgroup>
          <col
            v-for="column of columns"
            :key="column.key"
            :width="column.width + 'px'"
          >
        </colgroup>
        <thead>
          <tr>
            <th
              v-for="column of columns"
              :key="column.key"
              class="recycleGridTh"
            >
              {{ column.name }}
            </th>
          </tr>
        </thead>
      </table>
    </div>
    <div style="position: relative;">
      <div
        v-if="value && value.length"
        :style="{ overflowX: 'auto' }"
        @mousewheel="mousewheel"
      >
        <table>
          <colgroup>
            <col
              v-for="column of columns"
              :key="column.key"
              :width="column.width + 'px'"
            >
          </colgroup>
          <tbody>
            <tr
              v-for="(data, index) of showData"
              :key="index"
              :style="{ height: rowHeight + 'px' }"
            >
              <td
                v-for="column of columns"
                :key="column.key"
                class="recycleGridTd"
              >
                {{ data[column.key] }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div
        ref="scrollDiv"
        :style="{
          height: tableHeight + 'px',
          top: 0,
          right: 0,
          overflowY: 'scroll',
          position: 'absolute',
          width: '17px'
        }"
        @scroll="scroll"
      >
        <div :style="{ height: totalHeight + 'px' }" />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'RecycleGrid',
  props: {
    value: { type: Array, default: undefined },
    columns: { type: Array, default: undefined },
    rowHeight: { type: Number, default: 26 },
    pageUnit: { type: Number, default: 10 }
  },
  watch: {
    value (newValue) {
      this.displayData(newValue)
    }
  },
  data () {
    return {
      showData: [],
      totalHeight: 0,
      tableHeight: 0
    }
  },
  mounted () {
    this.displayData(this.value)
  },
  methods: {
    displayData (list = []) {
      this.totalHeight = list.length * this.rowHeight
      this.tableHeight = this.pageUnit * this.rowHeight
      const startIndex = Math.ceil(this.$refs.scrollDiv.scrollTop / this.rowHeight)
      const endIndex = startIndex + this.pageUnit
      this.showData = list.slice(startIndex, endIndex)
    },
    scroll (event) {
      this.displayData(this.value)
    },
    mousewheel (event) {
      this.$refs.scrollDiv.scrollTop += (event.wheelDelta < 0 ? this.rowHeight : -this.rowHeight)
    }
  }
}
</script>