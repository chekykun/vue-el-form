<!-- 1、暂时没集成快捷选项 -->
<template>
  <el-date-picker
    :readonly="options['readonly']"
    :disabled="options['disabled']"
    :editable="options['editable']"
    :clearable="options['clearable']"
    :size="options['size']"
    :placeholder="options['placeholder']"
    :start-placeholder="options['start-placeholder']"
    :end-placeholder="options['end-placeholder']"
    :type="options['dtype']||'date'"
    :format="options['format']"
    :align="options['align']"
    :popper-class="options['popper-class']"
    :picker-options="options['picker-options']"
    :range-separator="options['range-separator']"
    :default-value="options['default-value']"
    :value-format="options['value-format']"
    :name="options['name']"
    :unlink-panels="options['unlink-panels']"

    @blur="_blur"
    @focus="_focus"
    @change="_change"
    
    ref="date"
    v-model="val"
  ></el-date-picker>
</template>

<script>

export default {
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true,
      default: ''
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
  methods: {
    _change (e) {
      this.$emit('change', e,this.val)
    },
    _blur (e) {
      this.$emit('_blur', e,this.val)
    },
    _focus (e) {
      this.$emit('_focus', e,this.val)
    },
  }
}
</script>