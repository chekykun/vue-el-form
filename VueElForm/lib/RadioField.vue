<template>
  <base-radio :options="options" v-model="val" @change="_change"></base-radio>
</template>

<script>

import BaseRadio from './BaseRadio'

export default {
  components: { BaseRadio },
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true
    }
  },
  methods:{
    _change(e,val){
      this.options['change'](e,this.value)
    }
  },
  computed: {
    val: {
      get () {
        return this.value
      },
      set (value) {
        this.$emit('input', value)
      }
    }
  }
}
</script>