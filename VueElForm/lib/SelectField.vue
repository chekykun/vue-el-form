<template>
  <base-select :options="options" v-model="val" @change="change"></base-select>
</template>

<script>

import BaseSelect from './BaseSelect'

export default {
  components: { BaseSelect },
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true
    }
  },
  created () {
    this.setValues()
  },
  watch: {
    'options.options' () {
      this.setValues()
    }
  },
  computed: {
    multiple () {
      return this.options.multiple
    },
    val: {
      get () {
          return this.value
      },
      set (value) {
          this.$emit('input', value)
      }
    }
  },
  methods: {
    // 多选时element-ui内部通过 === 匹配标签值
    // 而包装split时无法判断原始值是数值还是字符串，所以统一转换成字符串
    setValues () {
      if (this.options.options && this.multiple === true) {
        // this.options.options.forEach(o => {
        //   o.value = o.value.toString()
        // })
      }
    },
    change(val){
      if(this.options.change){
        this.options.change(val)
      }
    }
  }
}
</script>