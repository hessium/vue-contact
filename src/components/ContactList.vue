<template>
    <transition-group name="contact-list" role="list" class="divide-y divide-gray-100">
      <ContactItem  v-for="item in props.list"
                    :key="item.id" class="flex justify-between gap-x-6 py-5"
                    @onDeleteItem="onDeletItem"
                    @onChangeItem="onChangeItem"
                    :item="item" />
    </transition-group>
</template>

<script setup>
import ContactItem from "@/components/ContactItem";

// eslint-disable-next-line no-undef
const props = defineProps({
      list : Array
});

// eslint-disable-next-line no-undef
const emit = defineEmits([
    'onChangeItem',
    'onDeleteItem'
])

const onChangeItem = (item) => {
  emit('onChangeItem', item)
}

const onDeletItem = (id) => {
  emit('onDeleteItem', id)
}

</script>

<style scoped>
.contact-list-enter-active {
  transition: all 0.3s ease-out;
}

.contact-list-leave-active {
  transition: all 0.4s cubic-bezier(1, 0.5, 0.8, 1);
}

.contact-list-fade-enter-from,
.contact-list-leave-to {
  transform: translateX(120px);
  opacity: 0;
}
.contact-list-move {
  transition: transform 0.8s ease;
}
</style>