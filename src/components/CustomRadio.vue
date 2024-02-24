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
  model: {
    prop: 'modelValue',
    event: 'change'
  },
  props: {
    modelValue: [String, Number],
    label: [String, Number],
    value: [String, Number],
    id: [String, Number],
    name: String,
    disabled: Boolean
  }
}
</script>

<style></style>
