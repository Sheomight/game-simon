<template>
  <div>
    <input 
    type="radio" 
    :name="name" 
    :id="id" 
    :value="value"
    :disabled="disabled"
    @change="$emit('change', +$event.target.value)"
    >
    <label :for="id">{{ label }}</label>
  </div>
</template>

<script>

export default {
  props: {
    modelValue: [String, Number],
    label: [String, Number],
    value: [String, Number],
    id: [String, Number],
    name: String,
    disabled: Boolean
  }, 
  methods: {
    suka() {
      alert('asd')
    }
  }
}
</script>

<style></style>
