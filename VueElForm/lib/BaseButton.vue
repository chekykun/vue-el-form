<template>
    <!-- 按钮 -->
    <el-button 
                :key="options['text']"

                :size="options['size']"
                :type="options['dtype']||'default'"
                :plain="options['plain']"
                :round="options['round']"
                :loading="options['loading']"
                :disabled="options['disabled']"
                :icon="options['icon']"
                :autofocus="options['autofocus']"
                :native-type="options['native-type']"

                @click.native="options['click']"
              >
              <svg-icon v-if='options.sicon'
                    class-name="international-icon"
                    :icon-class="options.sicon" />
                    {{options['text']}}
              </el-button>
</template>
<script>
export default {
    props:['options']
}
</script>
