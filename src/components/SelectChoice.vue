<template>
  <div>
    <label class="display-4" for="input-number">몇 명?</label><br>
    <input class="form-control" type="number" id="input-number" v-model="inputNumber" min="1" :max="maxValue">
    <button class="btn btn-success btn-lg mt-3" id="pick" @click="selectName">뽑기</button>
  </div>
</template>

<script>
export default {
  name: 'SelectChoice',
  data () {
    return {
      inputNumber: 1, // 기본 1명 뽑기 위함.
    }
  }, // end data
  props:{
    maxValue: Number,
  }, // end props
  methods: {
    selectName() {
      const pic = document.querySelector('#pick')
      pic.addEventListener('keydown', function (e) {
        console.log(e.code)
      })
      this.$emit('select-name', this.inputNumber)
    }
  }, // end methods
}
</script>

<style>

</style>
