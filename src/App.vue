<script setup>
import { ref, reactive, computed, watch } from 'vue'
import Search from './components/Search.vue'
import UbileTable from './components/UbileTable.vue'
import Pagination from './components/Pagination.vue'

// 欄位說明:
// sno：站點代號、 sna：場站名稱(中文)、 tot：場站總停車格、
// sbi：場站目前車輛數量、 sarea：場站區域(中文)、 mday：資料更新時間、
// lat：緯度、 lng：經度、 ar：地(中文)、 sareaen：場站區域(英文)、
// snaen：場站名稱(英文)、 aren：地址(英文)、 bemp：空位數量、 act：全站禁用狀態

const search = reactive({
  name: ''
})

const table = reactive({
  origin:[],
  filted:computed(()=>{
    return  [...table.origin].filter(d => d.sna.includes(search.name))
  }),
  sorted:computed(()=>{
    if(table.sort == '')
      return [...table.filted]
    if (table.sort == 'asc') {
      return [...table.filted].sort((a, b) => a[table.order] - b[table.order]);
    } else {
      return [...table.filted].sort((a, b) => b[table.order] - a[table.order]);
    }
  }),
  items: computed(()=>{
    return [...table.sorted].splice((pagination.currentPage - 1) * pagination.prePage, pagination.prePage)
  }),
  order: '',
  sort: ''
})

const pagination = reactive({
  currentPage: 1,
  prePage: 10,
  total:computed(()=>{
    return table.sorted.length
  })
})

fetch('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz')
  .then(res => res.json())
  .then(json => {
    const stops = Object.keys(json.retVal).map(key => json.retVal[key]);
    table.origin = stops
  });

const resetPage = computed(()=>{
  return search.name + pagination.prePage
})

watch(resetPage,()=>{
  pagination.currentPage = 1
})

</script>

<template>
    <search v-model:name="search.name"></search>
    <ubile-table v-model:order="table.order" v-model:sort="table.sort" :items="table.items"></ubile-table>
    <pagination v-model:currentPage="pagination.currentPage" v-model:prePage="pagination.prePage" :total="pagination.total"></pagination>  
</template>

<style scoped>
body {
  padding: 1em;
}

ul {
  display: block;
  margin: 1rem auto 2rem;
  overflow: hidden;
}
</style>
