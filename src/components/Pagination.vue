<template>
    <ul v-if="props.total > 0">
        <li @click.prevent="setPage(props.currentPage - 1)" class="pager"><a href="#">
                &lt;
            </a></li>
        <template v-for="n in pages">
            <li class="pager" :class="{ 'active': props.currentPage === n }" @click="setPage(n)">
                <a href="#">{{  n  }}</a>
            </li>
        </template>

        <li @click.prevent="setPage(props.currentPage + 1)" class="pager"><a
                href="#"> &gt; </a></li>
    </ul>
    顯示筆數:
    <select @change="$emit('update:prePage', parseInt($event.target.value))">
        <option value="10">10</option>
        <option value="20">20</option>
        <option value="30">30</option>
    </select>
</template>
<style scope>
.pager {
    float: left;
    display: block;
    height: 25px;
    text-align: center;
    margin-right: 5px;
    border: 1px solid #aaa;
}

.pager.active a {
    background-color: rgb(99, 125, 238);
    color: #fff !important;
}

.pager a {
    padding: 5px;
    line-height: 25px;
    text-decoration: none;
}
</style>
<script setup>
import { computed } from '@vue/reactivity';
import { provide, ref, reactive, nextTick } from 'vue'

const props = defineProps({
    currentPage: {
        type: Number
    },
    total: {
        type: Number
    },
    prePage: {
        type: Number
    }
})

const emit = defineEmits(['update:currentPage','update:prePage'])

const lastPage = computed(() => {
    return Math.ceil((props.total / props.prePage))
})

const pages = computed(() => {
    let first = props.currentPage - 5
    if (first < 1) first = 1
    let last = first + 9
    if (last > lastPage.value) {
        last = lastPage.value
        first = last - 9 >= 1 ? last - 9 : 1
    }
    let arr = []
    for (let i = first; i <= last; i++) {
        arr.push(i)
    }
    return arr
})

const setPage = (page) => {
    if(page >= 1 && page <= lastPage.value){
        emit('update:currentPage',page)
    }
}

</script>