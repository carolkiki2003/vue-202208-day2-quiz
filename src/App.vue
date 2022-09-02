<template>
  <Search :searchName="searchName" :countOfPages="countOfPages" @inputSearchName="inputSearchName" @setCurrentSort="setCurrentSort" @changeCountOfPages="changeCountOfPages"/>
  <UbikeTable @setOrder="setOrder" :splitedStops="splitedStops" :currentSort="currentSort" :isAsc="isAsc"/>
  <Pagination :currentPage="currentPage" :pageAddAmount="pageAddAmount" :totalPages="totalPages" @setCurrentPage="setCurrentPage"/>
</template>
<script setup>
  import UbikeTable from './components/UbikeTable.vue'
  import Search from './components/Search.vue'
  import Pagination from './components/Pagination.vue'
  import { ref,computed,watch } from 'vue'
  const uBikeStops = ref([]);
  const searchName = ref('');
  const currentSort = ref('');
          
  // asc 遞增(由小到大); desc 遞減(由大到小)
  const isAsc = ref(true);
  
  fetch('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz')
    .then(res => res.json())
    .then(json => {
      const stops = Object.keys(json.retVal).map(key => json.retVal[key]);
      uBikeStops.value = stops;
    });
  
  const filtedStops = computed(() => {
    return uBikeStops.value.filter(d => d.sna.includes(searchName.value));
  });
  
  const sortedStops = computed(() => {
    if(currentSort.value === '') {
      return filtedStops.value
    }
    const stops = [...filtedStops.value];
    if(isAsc.value) {
      stops.sort((a, b) => a[currentSort.value] - b[currentSort.value]);
    }else {
      stops.sort((a, b) => b[currentSort.value] - a[currentSort.value]);
    }
  
    return stops;
  });
  
  
  const countOfPages = ref(10);
  const currentPage = ref(1);
  const totalPages = computed(() => Math.ceil(filtedStops.value.length / countOfPages.value));
  const splitedStops = computed(() => {
    // 0 ~ 9
    let start=(currentPage.value-1)*countOfPages.value
    let end =currentPage.value*countOfPages.value
    return sortedStops.value.slice(start, end);
  });
  const pageAddAmount = computed(()=>{return Math.floor((currentPage.value-1) / 10) * 10;})
  const setOrder = (field, order) => {
    currentSort.value = field;
    isAsc.value = order;
  }
  const inputSearchName = (name)=>{
    searchName.value= name
  }
  const setCurrentSort = (sort)=>{
    currentSort.value=sort
  }
  const changeCountOfPages = (count)=>{
    countOfPages.value=count
  }
  const setCurrentPage= (page)=>{
    currentPage.value=page
  }
  watch(searchName,()=>{
    currentPage.value=1
  })
  </script>
