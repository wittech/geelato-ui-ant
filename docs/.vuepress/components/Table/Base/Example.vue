<template>
  <div>
    <gl-table ref="table" :opts="config" :query="{em:'platform_user'}"></gl-table>
  </div>
</template>
<script>
  import data from './data.js'

  export default {
    components: {},
    data() {
      return {
        config: data
      }
    },
    computed: {},
    created() {
    },
    mounted() {

    },
    methods: {
      refreshTable() {
        this.$refs.table.refresh(true)
      }
    }
  }
</script>
<style>

</style>
