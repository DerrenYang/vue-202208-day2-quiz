<template>
<div>
        站點搜尋: <input type="text" v-model="searchName">
        <button @click="defaultSort">預設排序</button>
        
        顯示筆數: 
        <select v-model="countOfPages">
          <option value="10">10</option>
          <option value="20">20</option>
          <option value="30">30</option>
        </select>
</div>
</template>

<script>
import { ref ,watch} from 'vue'

export default {
    name: 'Search',
    setup(props, context) {
        const { emit } = context
        const searchName = ref('')
        const currentSort = ref('')
        const countOfPages = ref(10)
        watch([searchName, countOfPages], () => {
            emit('order', 1)
            emit('search', searchName.value)
            emit('sort', countOfPages.value)
        })

        const defaultSort = () => {
            emit('reSort')
        }

        return {
            searchName,
            currentSort,
            countOfPages,
            defaultSort
        }
    }
}
</script>