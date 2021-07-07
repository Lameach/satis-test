<template>
  <q-expansion-item>
    <template v-slot:header>
      <q-item-section avatar>
        <q-avatar>
          <img :src="worker.avatar ? worker.avatar : EMPTY_AVATAR" />
        </q-avatar>
      </q-item-section>
      <q-item-section>
        {{ worker.name }}
      </q-item-section>
      <q-btn
        flat
        round
        color="primary"
        icon="edit"
        @click="e => editClickHandler(e, worker.name)"
      />
      <q-btn
        flat
        round
        color="red"
        icon="delete"
        @click="e => deleteClickHandler(e, worker.name)"
      />
    </template>
    <q-card>
      <q-list>
        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="grey" name="cake" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ worker.birthDate }}</q-item-label>
            <q-item-label caption>Дата рождения</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="grey" name="home" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ worker.address }}</q-item-label>
            <q-item-label caption>Домашний адрес</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="grey" name="phone" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ worker.phone }}</q-item-label>
            <q-item-label caption>Номер телефона</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable>
          <q-item-section avatar>
            <q-icon color="grey" name="email" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ worker.email }}</q-item-label>
            <q-item-label caption>Электронная почта</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-card>
  </q-expansion-item>
</template>

<script>
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
    editClickHandler: Function,
    deleteClickHandler: Function
  },
  setup() {
    const EMPTY_AVATAR = "https://pbs.twimg.com/media/DD1g0nFUMAEElE0.jpg";
    return {
      EMPTY_AVATAR
    };
  }
};
</script>
