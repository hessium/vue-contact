<template>
  <li>
    <div class="flex flex-col min-w-0 gap-x-4">
      <p class="text-sm font-semibold leading-6 text-gray-900">{{props.item.name}}</p>
      <div  class="flex min-w-0 gap-x-4">
        <p class="truncate text-xs leading-5 text-gray-500">{{ props.item.email}}</p>
        <p class="truncate text-xs leading-5 text-gray-500">{{props.item.phone}}</p>
      </div>
    </div>
    <div class="flex items-center min-w-0 gap-x-4">
      <button @click="emit('onChangeItem', props.item)" class="inline-flex items-center rounded-md bg-green-50 px-2 py-1 text-xs font-medium text-green-700 ring-1 ring-inset ring-green-600/20">
        Изменить
      </button>
      <button  @click="emit('onDeleteItem', props.item.id)" class="inline-flex items-center rounded-md bg-red-50 px-2 py-1 text-xs font-medium text-red-700 ring-1 ring-inset ring-red-600/10">
        Удалить
      </button>
    </div>
  </li>
</template>
<script setup>
// eslint-disable-next-line no-undef
const props = defineProps({
  item : Object,
})
// eslint-disable-next-line no-undef
const emit = defineEmits([
    'onChangeItem',
    'onDeleteItem'
])

</script>

<style scoped>

</style>