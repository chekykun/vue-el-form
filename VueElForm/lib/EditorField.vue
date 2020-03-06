<template>
  <base-richtext :options="options" v-model="val" @blur="options.blur" @focus="options.focus" @ready="options.ready"></base-richtext>
</template>

<script>

import BaseRichtext from './BaseEditor'

export default {
  components: { BaseRichtext },
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
  }
}
</script>