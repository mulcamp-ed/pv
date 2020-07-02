<template>
  <div>
    <label class="display-4" for="input-name" >인원 추가</label><br>
    <input class="form-control" type="text" id="input-name" v-model="inputName" @keypress.enter="addName">
    <button class="btn btn-primary btn-lg mt-3" @click="addName">추가</button>
  </div>
</template>

<script>
export default {
  name: 'SelectAdd',
  data () {
    return {
      inputName: '',
    }
  },
  methods: {
    addName() {
      if (this.inputName.trim()){
        this.$emit('add-name', this.inputName)
        this.inputName = ''
      }
    },
  }
}
</script>

<style>

</style>
