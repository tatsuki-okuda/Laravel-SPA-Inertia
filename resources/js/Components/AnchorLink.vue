<template>
  <inertia-link
    v-if="href"
    :href="href"
    class="px-3 py-2 mr-2 rounded text-white text-sm font-bold whitespace-nowrap"
    :class="classes"
  >
    <slot/>
  </inertia-link>
  <button
    v-else
    class="px-3 py-2 mr-2 rounded text-white text-sm font-bold whitespace-nowrap"
    :class="classes"
  >
    <slot/>
  </button>
</template>

<script>
import { computed } from 'vue';
export default {
  props: {
    href: String,
    mode: {
      type: String,
      default: 'add'
    }
  },
  setup(props) {
      const classes = computed(() => {
        if(props.mode === 'add'){
          return 'bg-blue-600 hover:bg-blue-800'
        }
        if(props.mode === 'edit'){
          return 'bg-yellow-600 hover:bg-yellow-800'
        }
        if(props.mode === 'delete'){
          return 'bg-red-600 hover:bg-red-800'
        }
        if(props.mode === 'view'){
          return 'bg-green-600 hover:bg-green-800'
        }
        return '';
      });
      return {
        classes,
      }
  }
  
}
</script>