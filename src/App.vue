<template>
  <q-layout view="hHh lpR fFf" class="page">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title>
          Список сотрудников
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container style="width: 650px">
      <workers-list></workers-list>
    </q-page-container>
  </q-layout>
</template>

<script>
import { ref } from "vue";
import WorkersList from "./components/WorkersList.vue";

export default {
  name: "LayoutDefault",

  components: { WorkersList },

  setup() {
    return {
      leftDrawerOpen: ref(false)
    };
  }
};
</script>

<style scoped>
.page {
  display: flex;
  justify-content: center;
  padding-top: 50px;
}
</style>
