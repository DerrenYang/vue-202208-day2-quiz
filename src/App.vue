<template>
<Search 
  @search="doSearch" 
  @sort="doSort"
  @order="reOrder"
  @reSort="defaultSort"
/>
    
<DataTable 
  :currentSort="currentSort" 
  :splitedStops="splitedStops" 
  :isAsc="isAsc" 
  @order="setOrder" 
/>

<Pagination 
  :currentP="currentPage" 
  :totalP="totalPages" 
  @click="updateCurrentPage" 
/>

</template>

<script>
import { ref, computed, watch  } from 'vue'
import Pagination from './Pagination.vue'
import Search from './Search.vue'
import DataTable from './DataTable.vue'

export default {
  components: {
    Pagination,
    Search,
    DataTable
  },
  setup() {
    const uBikeStops = ref([]);
    const searchName = ref('');
    const currentSort = ref('');

    // asc 遞增(由小到大); desc 遞減(由大到小)
    const isAsc = ref(true)

    const uBikeData = async () => {
      const response = await fetch('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz')
      const json = await response.json()
      const data = Object.keys(json.retVal).map(key => json.retVal[key])
      uBikeStops.value = data
    }
    uBikeData()
    
    const filtedStops = computed(() => {
      return uBikeStops.value.filter(d => d.sna.includes(searchName.value))
    })

    const sortedStops = computed(() => {
      if (currentSort.value === '') {
        return filtedStops.value
      }

      const stops = [...filtedStops.value]

      if (isAsc.value) {
        stops.sort((a, b) => a[currentSort.value] - b[currentSort.value])
      } else {
        stops.sort((a, b) => b[currentSort.value] - a[currentSort.value])
      }

      return stops
    })

    const setOrder = (field, order) => {
      currentSort.value = field
      isAsc.value = order
    }

        
    const countOfPages = ref(10)
    const currentPage = ref(1)  
    const totalPages = computed(() => {
        return Math.ceil(filtedStops.value.length / countOfPages.value)
    });

    const splitedStops = computed(() => {
      // 0 ~ 9
      let start = (currentPage.value - 1) * countOfPages.value
      let end = Number(start) + Number(countOfPages.value)
      console.log('start : ' + start + ' end : ' + end)
      return sortedStops.value.slice(start, end)
    })

    const updateCurrentPage = (val) => {
      currentPage.value = val
    }

    const doSearch = (val) => {
      searchName.value = val
    }

    const doSort = (val) => {
      countOfPages.value = val
    }

    const reOrder = () => {
      currentPage.value = 1
    }

    const defaultSort = () => {
      currentSort.value = ''
    }

    return {
      searchName,
      filtedStops,
      isAsc,
      sortedStops,
      currentSort,
      setOrder,
      splitedStops,
      totalPages,
      currentPage,
      countOfPages,
      updateCurrentPage,
      doSearch,
      doSort,
      reOrder,
      defaultSort
    }
  }
}
</script>