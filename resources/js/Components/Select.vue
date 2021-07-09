<template>
   <select 
      class="focus:ring focus:ring-opacity-50 rounded-md shadow-sm"
      :class="classes" 
      :value="modelValue"
      v-bind="{
        ...$attrs,
        onChange: ($event) => {
          $emit('update:modelValue', $event.target.value);
        },
      }"
    >
    <option value="">-Please Select-</option>
    <option 
      v-for="option in options" 
      :key="option.id" 
      :value="option.id" 
      :selected="option.id === modelValue"
    >
      {{ option.label }}
    </option>
  </select>
</template>



<script>
import { computed } from 'vue';
export default {
  props: {
    error: {
      type: String,
      default: "",
    },
    modelValue: {
      type: [String, Number],
      default: "",
    },
    options: {
      type: Array,
      required: true,
    },
  },
  setup(props) {
    const classes = computed(()=>{
      return props.error
        ? "border-red-300 focus:border-red-600 focus:ring-red-200"
        : "border-gray-300 focus:border-indigo-300 focus:ring-indigo-200";
    });

    return {
      classes
    }
  },
}
</script>