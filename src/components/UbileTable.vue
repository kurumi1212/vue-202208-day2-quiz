<template>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>場站名稱</th>
          <th>場站區域</th>
          <th @click="setOrder('sbi')">
            目前可用車輛
            <template v-if="props.order == 'sbi'">
              <i v-if="isAsc" class="fa fa-sort-asc" aria-hidden="true"></i>
              <i v-else class="fa fa-sort-desc" aria-hidden="true"></i>
            </template>
          </th>
          <th @click="setOrder('tot')">
            總停車格
            <template v-if="props.order == 'tot'">
              <i v-if="isAsc" class="fa fa-sort-asc" aria-hidden="true"></i>
              <i v-else class="fa fa-sort-desc" aria-hidden="true"></i>
            </template>
          </th>
          <th>資料更新時間</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="s in props.items" :key="s.sno">
          <td>{{  s.sno  }}</td>
          <td>{{  s.sna  }}</td>
          <td>{{  s.sarea  }}</td>
          <td>{{  s.sbi  }}</td>
          <td>{{  s.tot  }}</td>
          <td>{{  timeFormat(s.mday)  }}</td>
        </tr>
      </tbody>
    </table>
</template>
<style scope>
</style>
<script setup>
import { computed } from '@vue/reactivity';
import { provide, ref,reactive,nextTick } from 'vue'

const props = defineProps({
    items:{
        type:Array
    },
    order:{
        type:String
    },
    sort:{
        type:String
    }
})

const emit = defineEmits(['update:sort','update:order'])

const isAsc = computed(()=>{
    props.sort == 'asc'
})

const setOrder = (field) => {
  emit('update:order',field)
  emit('update:sort',props.sort == 'asc'? 'desc' : 'asc')
}

const timeFormat = (val) => {
  // 時間格式
  const pattern = /(\d{4})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/;
  return val.replace(pattern, '$1/$2/$3 $4:$5:$6');
};

</script>