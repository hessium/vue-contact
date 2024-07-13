
<template>
  <div class="mt-10 sm:mx-auto max-w-xl space-y-6 px-6">
    <h1>Список контактов</h1>
    <SearchBar @updateList="onUpdateList" v-model="searchInput"/>
    <ContactForm :itemSelected="itemSelected"
                 @updateList="onUpdateList" :isItemSelected="isItemSelected" @onChangeSelected="onChangeSelected"/>
    <ContactList :list="searchList"
                 @onDeleteItem="onDeleteItem"
                 @onChangeItem="onChangeItem"/>
    <div v-show="!searchList.length" >
      <p>Здесь пока пусто</p>
    </div>
  </div>
</template>

<script setup >
import ContactList from "@/components/ContactList";
import ContactForm from "@/components/ContactForm";
import SearchBar from "@/components/SearchBar";

import { onMounted,  ref, watch, computed} from "vue";

let contactsList = ref([]);
const isUpdateList = ref(false);
const itemSelected = ref(null);
const isItemSelected = ref(false);
const searchInput = ref('');

const onUpdateList = () => {
  isUpdateList.value = !isUpdateList.value;
}

const onChangeSelected = () => {
  isItemSelected.value = !isItemSelected.value;
}

const onChangeItem = (item) => {
  isItemSelected.value = !isItemSelected.value;
  itemSelected.value = item;
}

const onDeleteItem = (id) => {
  let newList = contactsList.value.filter((item) => item.id !== id)
  localStorage.setItem('contacts', JSON.stringify(newList));
  contactsList.value = newList
}

// eslint-disable-next-line
const searchList = computed(() => {
  if(!contactsList.value) return []
  return contactsList.value.filter(item => item.name?.toLowerCase().includes(searchInput.value.toLowerCase()))
})

watch(
    () => isUpdateList.value,
    () => {
      contactsList.value = JSON.parse(localStorage.getItem('contacts'))
    }
)
onMounted(() => {
  contactsList.value = JSON.parse(localStorage.getItem('contacts' || '[]'))
})

</script>
<style scoped>
h1 {
  font-size: 3rem;
  margin: 0 auto;
  font-weight: 700;
  text-align: center;
}
</style>