<template>
  <input v-model="model"
         :type="props.type"
         :placeholder="props.placeholder"
         class="block w-full rounded-md border-0 p-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300
         placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
         required
  />
</template>

<script setup>

// eslint-disable-next-line no-undef
const model = defineModel();
// eslint-disable-next-line no-undef
const props = defineProps([
    'type',
    'placeholder'
])

</script>

<style scoped>

</style>