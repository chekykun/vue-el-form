<template>
<span>
    <el-form-item 
            :prop="options['prop']"
            :label="options['label']"
            :label-width="options['label-width']"
            :required="options['required']"
            :rules="options['rules']"
            :error="options['error']"
            :show-message="options['show-message']"
            :inline-message="options['inline-message']"
            :size="options['size']"
            v-if="options['label']"
          >
          <slot></slot>
          </el-form-item>
          <div class="no-label-item" v-else>
              <slot ></slot>
          </div>
</span>
</template>
<script>
export default {
    props:['options']
}
</script>
<style lang="scss">
.no-label-item{
    text-align: center;
    padding: 0 1rem;
}
</style>
