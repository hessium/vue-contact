<template>
  <form  @submit.prevent="addContact" class="space-y-6">
    <!--        <ContactInput v-model="name" :placeholder="'Имя'" :type="'text'" @validateInput="validateInput" :validate="isValidEmail"/>-->
    <div v-if="props.isItemSelected">
        <p>Изменяется элемент {{props.itemSelected.id}}</p>
      </div>
      <div>
          <div class="error">{{errorName}}</div>
          <input v-model="name"
               type="text"
               placeholder="Имя"
               @keyup="validateName"
               @blur="validateName"
               class="block w-full rounded-md border-0 p-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300
               placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
               required
          />
      </div>
      <div>
          <div class="error">{{errorEmail}}</div>
          <input v-model="email"
               type="text"
               placeholder="Email"
               @keyup="validateEmail"
               @blur="validateEmail"
               class="block w-full rounded-md border-0 p-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300
               placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
               required
          />
      </div>
      <div>
          <input v-model="phone"
               type="text"
               placeholder="Номер телефона"
               class="block w-full rounded-md border-0 p-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300
               placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
               required
          />
      </div>
      <button v-if="!props.isItemSelected" type="submit" class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600" >Добавить</button>
      <button v-else  @click="onChangeItem(props.itemSelected.id)" class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Изменить</button>
  </form>
</template>

<script setup>

import { onMounted, ref, watch} from "vue";

// eslint-disable-next-line no-undef
const props = defineProps([
    "itemSelected",
    "isItemSelected",
])
// eslint-disable-next-line no-undef
const name = defineModel('name');
// eslint-disable-next-line no-undef
const email = defineModel('email');
// eslint-disable-next-line no-undef
const phone = defineModel('phone');
// eslint-disable-next-line no-undef
let errorName = ref('');
// eslint-disable-next-line no-undef
let errorEmail = ref('');
// eslint-disable-next-line no-undef
const emit = defineEmits([
    'updateList',
    "onChangeSelected"
]);


watch(
    () => props.itemSelected,
    () => {
      name.value = props.itemSelected.name;
      email.value = props.itemSelected.email;
      phone.value = props.itemSelected.phone;
    }
)

onMounted(() => {
  if (props.itemSelected) {
    name.value = props.itemSelected.name;
    email.value = props.itemSelected.email;
    phone.value = props.itemSelected.phone;
  }
})

const validateName = () => {
  errorName.value =  !name.value ? "Введите имя" : "";
};

const validateEmail = () => {
  const regex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
  errorEmail.value =  !regex.test(email.value) ? 'Не валидный email' : '';
};

const addContact = () => {
  if(!name.value || !email.value || !phone.value) return;
  const contacts = JSON.parse(localStorage.getItem('contacts') || '[]');
  const contact = {
    name: name.value,
    email: email.value,
    phone: phone.value,
    id: Date.now()
  }
  contacts.push(contact);
  localStorage.setItem('contacts', JSON.stringify(contacts));
  emit('updateList');
  resetForm();
}

const onChangeItem  = (id) =>  {
  if(!name.value || !email.value || !phone.value) return
  const contacts = JSON.parse(localStorage.getItem('contacts') || '[]');
  contacts.forEach((item) => {
        if (item.id === id) {
              item.name = name.value,
              item.email = email.value,
              item.phone = phone.value
        }
      }
  )
  localStorage.setItem('contacts', JSON.stringify(contacts));
  emit('updateList');
  emit('onChangeSelected');
  resetForm();
}

const resetForm = () => {
  name.value = "";
  email.value = "";
  phone.value = "";
}
</script>

<style scoped>
.error {
  color: red;
  margin-bottom: 1rem;
}
</style>