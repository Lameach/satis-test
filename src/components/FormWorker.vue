<template>
  <q-card style="width: 350px">
    <q-card-section class="bg-primary text-white">
      <div class="text-h6">{{ header }}</div>
    </q-card-section>
    <q-card-section>
      <q-form @submit="submit($event, editingWorker)" class="q-gutter-md">
        <q-avatar>
          <img
            :src="editingWorker.avatar ? editingWorker.avatar : EMPTY_AVATAR"
          />
        </q-avatar>
        <q-btn-toggle
          v-model="editingWorker.gender"
          toggle-color="primary"
          flat
          :options="[
            { label: 'Мужской', value: 'male' },
            { label: 'Женский', value: 'female' }
          ]"/>
        <q-input
          filled
          type="url"
          v-model="editingWorker.avatar"
          label="Аватар"
          hint="Ссылка на аватар сотрудника"/>
        <q-input
          filled
          v-model="editingWorker.name"
          label="Имя"
          hint="Имя и
        фамилия"
          :rules="[
            val => !!val || 'Обязательное поле',
            val =>
              (val.split(' ').length > 1 &&
                val.split(' ')[0] !== '' &&
                val.split(' ')[1] !== '') ||
              'Введите имя и фамилию полностью'
          ]"/>
        <q-input
          v-model="editingWorker.birthDate"
          filled
          type="date"
          hint="Дата
        рождения"
          :rules="[
            val => !!val || 'Обязательное поле',
            val =>
              Math.floor(
                (Date.now() - Date.parse(val)) / (1000 * 60 * 60 * 24 * 365.25)
              ) >= 18 || 'Сотрудник должен быть старше 18 лет'
          ]"/>
        <q-input
          v-model="editingWorker.address"
          filled
          label="Адрес"
          hint="Домашний адрес"
          :rules="[val => !!val || 'Обязательное поле']"/>
        <q-input
          v-model="editingWorker.phone"
          filled
          type="tel"
          hint="Телефон"
          mask="phone"
          unmasked-value
          :rules="[val => !!val || 'Обязательное поле']"/>
        <q-input
          v-model="editingWorker.email"
          filled
          type="email"
          hint="Электронная почта"
          :rules="[val => !!val || 'Обязательное поле']"/>
        <div>
          <q-btn label="Сохранить" type="submit" color="primary" />
          <q-btn
            label="Отмена"
            color="primary"
            flat
            class="q-ml-sm"
            v-close-popup
          /></div></q-form></q-card-section
  ></q-card>
</template>

<script>
import { ref } from "vue";
export default {
  props: {
    worker: {
      name: String,
      gender: String,
      avatar: String,
      birthDate: String,
      address: String,
      phone: String,
      email: String
    },
    header: String,
    submit: Function
  },
  setup(props) {
    const EMPTY_AVATAR = "https://pbs.twimg.com/media/DD1g0nFUMAEElE0.jpg";
    const editingWorker = ref({
      name: props.worker.name,
      gender: props.worker.gender,
      avatar: props.worker.avatar,
      birthDate: props.worker.birthDate
        .split(".")
        .reverse()
        .join("-"),
      address: props.worker.address,
      phone: props.worker.phone,
      email: props.worker.email
    });
    return {
      editingWorker,
      EMPTY_AVATAR
    };
  }
};
</script>
