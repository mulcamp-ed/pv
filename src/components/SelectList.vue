<template>
  <div>
    <div class="d-flex">
      <button class="btn btn-light" @click="resetAll">처음 처럼</button>
      <button class="btn btn-light" @click="resetSkipped">스킵 제거</button>
      <div class="my-auto ml-5" v-if="selectedList.length">
        <span class="font-weight-bold">Selected : </span>
        <span v-for="(item, idx) in selectedList" :key="idx" class="btn btn-warning font-weight-bold text-truncate mx-1 selected-box"> 
          {{ item.name }} 
        </span>
      </div>
    </div>

    <div class="container">
      <div class="row" v-if="dataList.length">
        <SelectListItem class="col-lg-3 my-3 mx-2 card-style" 
          v-for="(item, idx) in dataList" :key="idx" :item="item" :idx="idx" 
          @delete-item="delItem" @switch-skipped="switchSkipped" />
      </div>
      <div class="display-1 font-weight-bold my-3" v-else>
        No Members...
      </div>
    </div>
  </div>
</template>

<script>
import SelectListItem from './SelectListItem.vue'

export default {
  name: 'SelectList',
  components: {
    SelectListItem,
  }, // end components
  props: {
    dataList: Array,
    selectedList: Array,
  }, // end props
  methods: {
    delItem(idx) { 
      this.$emit('delete-item', idx)
    },
    switchSkipped(idx) {
      this.$emit('switch-skipped', idx)
    },
    resetAll() {
      this.$emit('reset-all')
    },
    resetSkipped() {
      this.$emit('reset-skipped')
    }
  }, // end methods
}
</script>

<style scoped>
.card-style {
  max-width: 13rem; 
  height: 5rem;
}

.selected-box {
  width: 7rem;
}
</style>
