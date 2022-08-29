<template>
    <div>
        站點搜尋: <input type="text" @keyup="$emit('update:name',$event.target.value)">
  </div>
</template>
<style scope>
</style>
<script setup>
import { provide, ref,reactive,nextTick } from 'vue'

const props = defineProps({
    name:{
        type:String,
        required:true
    }
})


</script>