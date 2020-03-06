<template>
  <base-input :options="options" v-model="val" @change.native="_change" @focus.native="options.focus" @blur.native="options.blur"></base-input>
</template>

<script>

import BaseInput from './BaseInput'

export default {
  components: { BaseInput },
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true
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
  },
  methods:{
    _change(e){
      if(this.options.change){
        this.options.change(e,this.val)
      }
    }
  }
}
</script>