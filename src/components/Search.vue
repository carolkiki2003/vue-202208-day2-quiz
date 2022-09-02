<template>
  <div>
    站點搜尋: <input type="text" v-model="searchName" @keyup="inputSearchName($event.target.value)">
    <button @click="setCurrentSort('')">預設排序</button>
    顯示筆數:
    <select v-model="countOfPages" @change="changeCountOfPages($event.target.value)"><option value="10">10</option><option value="20">20</option><option value="30">30</option></select>
    </div>
</template>
<script setup>
  import { defineProps,defineEmits } from 'vue'
  const props = defineProps({
    searchName:String,
    countOfPages:String
  })
  const emit = defineEmits(['inputSearchName','setCurrentSort','changeCountOfPages'])
  const inputSearchName = (name) => {
    emit('inputSearchName', name)
  }
  const setCurrentSort = (sort) => {
    emit('setCurrentSort', sort)
  }
  const changeCountOfPages = (count)=>{
    emit('changeCountOfPages', count)
  }
</script>
