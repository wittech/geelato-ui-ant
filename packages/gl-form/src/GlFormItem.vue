<template>
  <div>
    <gl-row type="" v-for="(row,rowIndex) in rows" :key="rowIndex">
      <gl-cell v-bind="cell" v-for="(cell,cellIndex) in row.cols" :key="cellIndex">
        <template v-if="cell.rows">
          <gl-form-item :rows="cell.rows" :properties="properties" :form="form" :controlRefs="controlRefs"
                        @propertyUpdate="onPropertyUpdate"
                        @loadRefData="onLoadRefData"></gl-form-item>
        </template>
        <template v-else>
          <gl-label v-if="cell.label" :label="cell.label" :property="getProperty(cell.field)"></gl-label>
          <gl-control :ref="getProperty(cell.field).gid" v-if="!cell.label" :form="form"
                      :property="getProperty(cell.field)"
                      @propertyUpdate="onPropertyUpdate" @loadRefData="onLoadRefData"></gl-control>
        </template>
      </gl-cell>
    </gl-row>
  </div>
</template>

<script>
  export default {
    name: "GlFormItem",
    props: {
      /**
       * 布局
       */
      rows: {
        type: Array,
        default() {
          return []
        }
      },
      properties: {
        type: Object,
        default() {
          return {}
        }
      },
      form: {
        type: Object,
        default() {
          return {}
        }
      },
      controlRefs: {
        type: Object,
        required: true
      }
    },
    watch: {
      // properties: {
      //   handler(val, oval) {
      //     console.log(val, oval)
      //   },
      //   immediate: true,
      //   deep: true
      // }
    },
    mounted() {
      for (let i in this.$refs) {
        // TODO this.$refs[i]是否存在多个值的情况？
        this.controlRefs[i] = this.$refs[i][0]
      }
      console.log('geelato-ui-ant > gl-form-item > mounted() > $refs,controlRefs: ', this.$refs, this.controlRefs)
    },
    destroyed() {
      for (let i in this.$refs) {
        delete this.controlRefs[i]
      }
      console.log('geelato-ui-ant > gl-form-item > destroyed() > $refs,controlRefs: ', this.$refs, this.controlRefs)
    },
    updated() {
    },
    methods: {
      getValue() {

      },
      resetProperty(property, key, value) {

      },
      setAsTableContainer(isTableContainer) {
        if (typeof this.$parent.setAsTableContainer === "function") {
          this.$parent.setAsTableContainer(isTableContainer)
        }
      },
      getProperty(name) {
        if (!name || !this.properties[name]) {
          return {control: 'null', title: ' ', gid: this.$gl.utils.uuid(8)}
        }
        return this.properties[name]
      },
      getFieldNameByCell(cell) {
        return Object.keys(cell)[0]
      },
      onPropertyUpdate({property, val, oval}) {
        this.$emit('propertyUpdate', {property, val, oval})
      },
      onLoadRefData({property}) {
        console.log('geelato-ui-ant > formItem > loadRefData', {property})
        this.$emit('loadRefData', {property})
      }
    }
  }
</script>

<style scoped>

</style>
