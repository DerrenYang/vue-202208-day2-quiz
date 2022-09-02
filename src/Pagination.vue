<template>
  第 {{currentP}} 頁 / 共 {{totalP}} 頁
 <ul>
      <li 
        @click.prevent="lastPage"
        class="pager"><a href="#"> &lt; </a></li>
        
        <template v-for="n in 10">
          <li 
            v-if="(n + pageAddAmount) <= totalP"
            @click.prevent="toTargetPage(n)"        
            class="pager" 
            :class="{
              'active': isActive(n)
            }">
            <!-- n 從 1 開始計算 -->
            <a href="#">{{ n + pageAddAmount }}</a>
          </li>
        </template>

      <li 
      @click.prevent="nextPage"
      class="pager"><a href="#"> &gt; </a></li>
    </ul>
</template>
<script>
import { ref, computed } from 'vue'

export default {
  name: 'Pagination',
  props: {
    currentP: Number,
    totalP: Number
  },
  setup(props, context) {
    const { emit } = context

    const pageAddAmount = computed(() => {
      return Math.floor((props.currentP - 1) / 10) * 10
    })

    const nextPage = () => {
      const pageNumber = (props.currentP === props.totalP) ? props.currentP : props.currentP + 1
      emit('click', pageNumber)
      return pageNumber
    }

    const lastPage = () => {
      const pageNumber = (props.currentP > 1) ? props.currentP - 1 : props.currentP
      emit('click', pageNumber)
      return pageNumber
    }

    const toTargetPage = (n) => {
      const pageNumber = n + pageAddAmount.value
      emit('click', pageNumber)
    }

    const isActive = (n) => {
      return props.currentP === (n + pageAddAmount.value)
    }

    return {
      pageAddAmount,
      nextPage,
      lastPage,
      toTargetPage,
      isActive
    }
  }
}
</script>