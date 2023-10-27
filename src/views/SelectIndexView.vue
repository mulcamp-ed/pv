<template>
  <div>
    <h1>팀원 선택!</h1>
    
    <div class="d-flex">
      <SelectAdd class="w-50 p-3 box-line" @add-name="addName" />
      <SelectChoice class="w-50 p-3 box-line" @select-name="selectName" :maxValue="computedMax" />
    </div>

    <SelectList class="my-3 box-line" :dataList="dataList" :selectedList="selectedList"
      @delete-item="delItem" @switch-skipped="switchSkipped" @reset-all="resetAll" @reset-skipped="resetSkipped" />

  </div>
</template>

<script>
import _ from 'lodash'

import SelectAdd from '@/components/SelectAdd.vue'
import SelectChoice from '@/components/SelectChoice.vue'
import SelectList from '@/components/SelectList.vue'

const STORAGE_DATA_LIST_KEY = 'data-list'
const dataStorage = {
  get() {
    const items = JSON.parse(localStorage.getItem(STORAGE_DATA_LIST_KEY) || '[]')
    items.forEach(item => {
      item.isSelected = false
      item.isSkipped = false
    })
    return items
  },
  save(items) {
    localStorage.setItem(STORAGE_DATA_LIST_KEY, JSON.stringify(items))
  }
}

export default {
  name: 'SelectIndexView',
  components: {
    SelectAdd,
    SelectList,
    SelectChoice,
  }, // end components
  data () {
    return {
      dataList: dataStorage.get(), // 목록 보여주기 위한 용도
      selectedList: [], // 선택되어진 목록
      nameList: [], // 제외 목록을 빼고 랜덤하게 뽑기 위한 용도
    }
  }, // end data
  methods: {
    addName(inputName) {
      this.dataList.push({
        name: inputName,
        isSelected: false,
        isSkipped: false,
      })
    },

    selectName(values) {
      number = values[0]
      console.log(values[1])
      if (number > this.computedMax){
        number = this.computedMax
      }
      this.resetSelected(this.selectedList) // 기존에 선택 된 것을 초기화 (카드색 원래대로)

      this.selectedList = _.sampleSize(this.nameList, number)
      this.setSelected(this.selectedList)
    },

    delItem(idx){
      this.dataList.splice(idx, 1)
    },

    setSelected(items) {
      items.forEach(item => item.isSelected = true)
    },

    resetSelected(items) {
      items.forEach(item => item.isSelected = false)
    },

    switchSkipped(idx) {
      this.dataList[idx].isSkipped = !this.dataList[idx].isSkipped
      // this.setNameList()
    },

    setNameList() {
      this.nameList = this.dataList.filter(item => !item.isSkipped)
    },
    
    resetSkipped() {
      this.dataList.forEach(item => item.isSkipped = false)
    },

    resetAll() {
      this.resetSelected(this.dataList)
      this.resetSkipped(this.dataList)
      this.selectedList = []
    },
  }, // end methods
  computed: {
    computedMax() {
      return this.nameList.length
    },
  }, // end computed
  created() {
    this.setNameList()
  },
  watch: {
    dataList: {
      handler(item) {
        dataStorage.save(item)
        this.setNameList()
      },
      deep: true,
    }
  }
}
</script>

<style scoped>
.box-line {
  border: 1px solid lightgrey;
  border-radius: 0.5rem;
}
</style>
