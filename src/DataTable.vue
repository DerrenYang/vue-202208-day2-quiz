<template>
  資料量{{splitedStops.length}}
 <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>場站名稱</th>
          <th>場站區域</th>
          <th @click="setOrder('sbi', !isAsc)">
            目前可用車輛
            <template v-if="currentSort === 'sbi'">
              <i 
              v-if="isAsc"
              class="fa fa-sort-asc" aria-hidden="true"></i>
            <i 
              v-else
              class="fa fa-sort-desc" aria-hidden="true"></i>
            </template>
          </th>
          <th @click="setOrder('tot', !isAsc)">
            總停車格
            <template v-if="currentSort === 'tot'">
              <i 
              v-if="isAsc"
              class="fa fa-sort-asc" aria-hidden="true"></i>
            <i 
              v-else
              class="fa fa-sort-desc" aria-hidden="true"></i>
            </template>
          </th>
          <th>資料更新時間</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="s in splitedStops" :key="s.sno">
          <td>{{ s.sno }}</td>
          <td>{{ s.sna }}</td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ timeFormat(s.mday) }}</td>
        </tr>
      </tbody>
    </table>
</template>
<script>
export default {
  name: 'DataTable',
  props: {
    currentSort: String,
    splitedStops: Array,
    isAsc: Boolean
    
  },
  setup(props, context) {
    const { emit } = context
    const timeFormat = (val) => {
      // 時間格式
      const pattern = /(\d{4})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/
      return val.replace(pattern, '$1/$2/$3 $4:$5:$6')
    }

    const setOrder = (field, order) => {
      emit('order', field, order)
    }
    return {
      timeFormat,
      setOrder    
    }
  }      
}
</script>