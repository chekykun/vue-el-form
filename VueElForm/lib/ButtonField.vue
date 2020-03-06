<template>
    <base-button :options="options"></base-button>
</template>

<script>
import BaseButton from './BaseButton'
export default {
    components:{BaseButton},
    props:{
        options:{
            required:true
        }
    },
}
</script>

