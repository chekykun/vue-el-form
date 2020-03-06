<template>
  <base-updownload :actionUrl="options['actionUrl']"
                   :filesList="options['filesList']"
                   :params="options['params']"
                   :disabled="options['disabled']"
                   :dialogStatus="options['dialogStatus']"
                   :dialogFormVisible="options['dialogFormVisible']"
                   :limit="options['limit']"
                   :flag="options['flag']||false"
                   @uploadCallBack="options['uploadCallBack']"
                   @removeCallBack="options['removeCallBack']"
                   @exceedCallBack="exceedCallBack"></base-updownload>
</template>

<script>
import BaseUpdownload from './BaseUpdownload'
export default {
  components: {
    BaseUpdownload
  },
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true
    }
  },
  methods: {
    exceedCallBack (file, fileList) {
      if (this.options.exceedCallBack) {
        this.options.exceedCallBack(file, fileList)
      }
    }
  }
}
</script>

