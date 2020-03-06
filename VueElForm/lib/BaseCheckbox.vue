<template>
  <el-checkbox-group
    :size="options['size']"
    :disabled="options['disabled']"
    :min="options['min']"
    :max="options['max']"
    :text-color="options['text-color']"
    :fill="options['fill']"

    @change.native="_change"
    ref="checkbox"
    v-model="val"
  >

    <div v-if="options['checkboxs']">
      <el-checkbox
        v-for="checkbox in options['checkboxs']"
        :key="checkbox['label']"
        :label="checkbox['label']"
        :true-label="checkbox['true-label']"
        :false-label="checkbox['false-label']"
        :disabled="checkbox['disabled']"
        :border="checkbox['border']"
        :size="checkbox['size']"
        :name="checkbox['name']"
        :checked="checkbox['checked']"
        :indeterminate="checkbox['indeterminate']"
      >{{checkbox['text']}}</el-checkbox>
    </div>
    
    <div v-else>
      <el-checkbox-button
        v-for="button in options['checkbox-buttons']"
        :key="button['label']"
        :label="button['label']"
        :true-label="button['true-label']"
        :false-label="button['false-label']"
        :disabled="button['disabled']"
        :name="button['name']"
        :checked="button['checked']"
      >{{button['text']}}</el-checkbox-button>
    </div>

  </el-checkbox-group>

</template>

<script>

export default {
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true
    }
  },
  created(){
  },
  data(){
    return{
      isChecked:{}
    }
  },
  // watch:{
  //   value(newValue,oldValue){
  //     this.val=newValue
  //   },
  //   val(newValue,oldValue){
  //     this.$emit('input', newValue)
  //   }
  // },
  computed: {
    val: {
      get () {
        return this.value.split(',')
      },
      set (value) {
        this.$emit('input', value)
      }
    }
  },
  methods: {
    _change (e,data) {
      // if(e.target.checked){
      //   this.val.push(e.target.defaultValue)
      //   }else{
      //   _.difference(this.val,[e.target.defaultValue])
      // }
      this.$emit('change', e,data)
    }
  }
}
</script>
