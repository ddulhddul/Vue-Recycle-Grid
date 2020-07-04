<template>
  <div>
    <h1>Recycle Grid : {{ list.length }}</h1>
    <RecycleGrid
      v-model="list"
      :columns="columns"
      :page-unit="10"
    />
  </div>
</template>
<script>
import RecycleGrid from './components/RecycleGrid.vue'
export default {
  components: {
    RecycleGrid
  },
  data () {
    return {
      list: [],
      columns: [
        { key: 'no', name: 'No.', width: 150 }
      ].concat(Array.from(Array(20)).map((obj, index) => {
        return {
          key: ['col', index].join(''),
          name: ['col', index].join(''),
          width: 150
        }
      }))
    }
  },
  mounted () {
    this.list = this.makeList()
  },
  methods: {
    makeList () {
      return Array.from(Array(100000)).map((obj, index) => {
        const result = this.columns.reduce((entry, obj) => {
          entry[obj.key] = this.randomData()
          return entry
        }, {})
        result.no = index + 1
        return result
      })
    },
    randomData () {
      return Math.floor(Math.random() * Math.pow(10,5))
    }
  }
}
</script>
