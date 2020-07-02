<template>
  <div class="d-flex justify-content-between border border-gray rounded-pill"
    :class="classObject" >
    <div class="d-flex w-85" @click="switchSkipped">
      <div class="badge badge-info my-auto badge-box">{{ idx+1 }}</div>
      <div class="text-truncate ml-3 my-auto name-box ">{{ item.name }}</div>
    </div>
    <span class="btn my-auto p-1 del-box" @click="delItem">x</span>
  </div>
</template>

<script>
export default {
  name: 'SelectListItem',
  props: {
    item: Object,
    idx: Number,
  }, // end props
  methods: {
    delItem() {
      this.$emit('delete-item', this.idx)
    },
    switchSkipped() {
      this.$emit('switch-skipped', this.idx)
    },
  }, // end methods
  computed: {
    classObject() {
      return {
        'bg-warning': this.item.isSelected && !this.item.isSkipped,
        'bg-secondary': this.item.isSkipped,
      }
    }
  }, // end computed
}
</script>

<style scoped>
.card-style {
  max-width: 13rem; 
  height: 5rem;
}

.w-85 {
  width: 85%;
}
.name-box {
  /* display: inline-block; */
  font-size: 1.3rem;
  font-weight: bold;
}

.badge-box {
  font-size: 1.5rem;
}

.del-box {
  background-color: lightgray;
  border-radius: 0.5rem;
}

</style>
