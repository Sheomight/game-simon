<template>
  <div >
    <CustomRadio 
    v-for="option in options" 
    :key="option.id"
    :name="option.name" 
    :id="option.id" 
    :label="option.label" 
    :value="option.value" 
    :disabled="disabled"
    v-model="selectedOption"
    @change="$emit('change', selectedOption)"
    />
  </div>
</template>

<script>
import CustomRadio from './CustomRadio.vue';

export default {
  model: {
    prop: 'modelValue',
    event: 'change'
  },
  props: {
    options: {
      type: Array,
      required: true 
    },
    disabled: Boolean,
    modelValue: [String, Number]
  },
  components: {
    CustomRadio
  },
  data() {
    return {
      selectedOption: null
    }
  },
  methods: {
    changeOption(value) {
      this.selectedOption = value
    }
  }
}
</script>