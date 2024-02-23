<template>
  <div class="modal" v-if="show" @click="hideModal()">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'modal-win',
  props: {
    show: {
      type: Boolean,
      defaul: false
    }
  },
  methods: {
    hideModal() {
      this.$emit('update')
    }
  }
}
</script>


<style scoped>
.modal {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}
</style>