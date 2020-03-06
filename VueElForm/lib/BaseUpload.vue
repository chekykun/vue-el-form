<!-- 1、这个组件的钩子全部处理成了属性而不是事件，这里做点修改 -->
<!-- 2、组件自定义程度比较高，先封装成简单的功能 -->
<template>
  <el-upload :action="options['action']"
             :headers="options['headers']"
             :multiple="options['multiple']"
             :data="options['data']"
             :name="options['name']"
             :with-credentials="options['with-credentials']"
             :show-file-list="options['show-file-list']"
             :drag="options['drag']"
             :accept="options['accept']"
             :list-type="options['list-type']"
             :auto-upload="options['auto-upload']"
             :http-request="options['http-request']"
             :disabled="options['disabled']"
             :limit="options['limit']"
             :before-upload="options['before-upload']"
             :on-preview="options['on-preview']"
             :on-remove="options['on-remove']"
             :on-success="options['on-success']"
             :on-error="options['on-error']"
             :on-progress="options['on-progress']"
             :on-change="options['on-change']"
             :on-exceed="options['on-exceed']"
             ref="upload"
             :file-list="options['fileList']"
             :uploadFiles="options['fileList']">

    <slot name="tip"
          slot="tip"></slot>
    <slot name="button"></slot>
  </el-upload>
</template>

<script>

export default {
  props: {
    'options': {
      default: Object,
      required: true
    },
    'value': {
      required: true
    }
  },
  methods: {
    '_before-upload' (file) {
      this.$emit('before-upload', file)
    },
    '_on-preview' (file) {
      this.$emit('on-preview', file)
    },
    // * 这个移除要确认下是本地移除还是服务端
    '_on-remove' (file, fileList) {
      this.$emit('input', fileList)
      this.$emit('on-remove', file, fileList)
    },
    // *
    '_on-success' (response, file, fileList) {
      this.$emit('input', fileList)
      this.$emit('on-success', response, file, fileList)
      this.$refs.upload.clearFiles()
    },
    // 报错说明文件没变化，不处理
    '_on-error' (err, file, fileList) {
      this.$emit('on-error', err, file, fileList)
    },
    '_on-progress' (event, file, fileList) {
      this.$emit('on-progress', event, file, fileList)
    },
    // 添加、上传成功、失败
    '_on-change' (file, fileList) {
      this.$emit('on-change', file, fileList)
    },
    '_on-exceed' (file, fileList) {
      this.$emit('on-exceed', file, fileList)
    },
    clearFiles () {
      this.$refs('upload').clearFiles()
    },
    abort () {
      this.$refs('upload').abort()
    }
  }
}
</script>
<style lang="scss">
.el-upload--picture-card {
  height: 172px;
  line-height: 172px;
  .upload-img {
    height: 100%;
  }
}
</style>

