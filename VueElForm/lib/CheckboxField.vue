<template>
  <base-checkbox v-loading="loading" :options="options" v-model="val" @change.native="_change"></base-checkbox>
</template>

<script>

import BaseCheckbox from './BaseCheckbox'

export default {
  components: { BaseCheckbox },
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
    'options.checkboxs' () {
      this.setValues()
    },
    value(val){
      this.val=val
    }
  },
  data(){
    return{
      loading:false
    }
  },
  computed: {
    val: {
      get () {
        return this.value ? this.value.join(',') : ''
      },
      set (value) {
        this.$emit('input', value)
      }
    }
  },
  methods: {
    setValues () {
      if (this.options.options && this.multiple === true) {
        // this.options.options.forEach(o => {
        //   o.value = o.value.toString()
        // })
      }
    },
    _change(val,data){
      this.$emit('change',val,data)
    }
  }
}
</script>