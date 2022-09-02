<template>
  <ul>
    <li class="pager"><a href="#" @click.prevent="currentPage>1?setCurrentPage(currentPage -1):setCurrentPage(currentPage)"> &lt; </a></li>
    <template v-for="n in 10">
      <li 
        v-if="(n + pageAddAmount) <= totalPages"
        @click.prevent="setCurrentPage(n + pageAddAmount)"        
        class="pager" 
        :class="{
          'active': currentPage === (n + pageAddAmount)
        }">
        <!-- n 從 1 開始計算 -->
        <a href="#">{{ n + pageAddAmount }}</a>
      </li>
    </template>

    <li class="pager"><a href="#" @click.prevent="currentPage===totalPages?setCurrentPage(currentPage):setCurrentPage(currentPage+1)"> &gt; </a></li>
  </ul>
</template>
<script setup>
  import { defineProps,defineEmits } from 'vue'
  const props = defineProps({
    currentPage:Number,
    pageAddAmount:Number,
    totalPages:Number
  })
  const emit = defineEmits(['setCurrentPage'])
  const setCurrentPage = (page) => {
    emit('setCurrentPage', page)
  }
</script>
