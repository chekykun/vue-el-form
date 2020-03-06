<template>
  <base-date :options="options" v-model="val" @change="_change" @blur="_blur" @focus="_focus"></base-date>
</template>

<script>

import BaseDate from './BaseDate'

export default {
  components: { BaseDate },
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
        // avoid Invalid Date
        return this.value
      },
      set (value) {
        // var time = ''
        // if (value) {
        //   let year = value.getFullYear()
        //   let month = value.getMonth() + 1
        //   let day = value.getDate()
        //   let hour = value.getHours()
        //   let minute = value.getMinutes()
        //   let second = value.getSeconds()
        //   let real = function (number) {
        //     return number >= 10 ? number.toString() : '0' + number
        //   }
        //   time = [
        //     year,
        //     '-',
        //     real(month),
        //     '-',
        //     real(day),
        //     'T',
        //     real(hour),
        //     ':',
        //     real(minute),
        //     ':',
        //     real(second)
        //   ].join('')
        // }
        this.$emit('input', value)
      }
    }
  },
  methods:{
    _change(e,val){
      if(this.options.change){
        this.options.change(e,val)
      }
    },
    _blur(e,val){
      if(this.options.blur){
        this.options.blur(e,val)
      }
    },
    _focus(e,val){
      if(this.options.focus){
        this.options.focus(e,val)
      }
    }
  }
}
</script>