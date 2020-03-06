<template>
  <base-form-item :options="options">
  <slot></slot>
  </base-form-item>
</template>

<script>

import BaseFormItem from './BaseFormItem'

export default {
  components: { BaseFormItem },
  props: {
    options: {
      default: Object,
      required: true
    }
  },
}
</script>
