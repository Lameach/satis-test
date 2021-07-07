<template>
  <q-list bordered class="rounded-borders" separator>
    <q-ajax-bar
      ref="bar"
      position="bottom"
      color="blue"
      size="10px"
      skip-hijack
    />
    <draggable
      v-model="workers"
      @start="drag = true"
      @end="
        () => {
          drag = false;
          saveChanges();
        }
      "
      tag="transition-group"
      item-key="name"
      v-if="workers.length"
    >
      <template #item="{element}">
        <worker
          class="list-group-item"
          :worker="element"
          :editClickHandler="editClickHandler"
          :deleteClickHandler="deleteWorker"
        ></worker>
      </template>
    </draggable>

    <q-card v-else class="text-caption text-weight-light">
      <q-card-section>
        Add first worker or wait for loading!
      </q-card-section>
    </q-card>

    <q-dialog v-model="editFormVisible">
      <form-worker
        :worker="
          editAddToggler
            ? {
                name: '',
                gender: 'male',
                avatar: '',
                birthDate: '',
                address: '',
                phone: '',
                email: ''
              }
            : workers[editing]
        "
        :header="
          editAddToggler ? 'Добавить сотрудника' : 'Редактировать данные'
        "
        :submit="editAddToggler ? addSubmit : editSubmit"
      ></form-worker>
    </q-dialog>
    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-btn fab icon="add" color="green" @click="addClickHandler" />
    </q-page-sticky>
  </q-list>
</template>

<style></style>

<script>
import { ref } from "vue";
import Worker from "./Worker.vue";
import axios from "axios";
import FormWorker from "./FormWorker.vue";
import draggable from "vuedraggable";

export default {
  setup() {
    const workers = ref([]);
    const editAddToggler = ref(false);
    const editing = ref(0);
    const editFormVisible = ref(false);
    const drag = ref(false);
    const bar = ref(null);

    let deleteWorker = (event, name) => {
      event.stopPropagation();
      workers.value.splice(
        workers.value.findIndex(e => e.name == name),
        1
      );
      sessionStorage.setItem("workers", JSON.stringify(workers.value));
    };

    let editClickHandler = (event, name) => {
      event.stopPropagation();
      editAddToggler.value = false;
      editFormVisible.value = true;
      editing.value = workers.value.findIndex(e => e.name == name);
    };

    let editSubmit = (event, editedWorker) => {
      event.preventDefault();
      editFormVisible.value = false;
      editedWorker.birthDate = editedWorker.birthDate
        .split("-")
        .reverse()
        .join(".");
      workers.value.splice(editing.value, 1, editedWorker);
      sessionStorage.setItem("workers", JSON.stringify(workers.value));
    };

    let addClickHandler = () => {
      editAddToggler.value = true;
      editFormVisible.value = true;
    };

    let addSubmit = (event, newWorker) => {
      event.preventDefault();
      editFormVisible.value = false;
      newWorker.birthDate = newWorker.birthDate
        .split("-")
        .reverse()
        .join(".");
      workers.value.push(newWorker);
      sessionStorage.setItem("workers", JSON.stringify(workers.value));
    };

    let saveChanges = () => {
      sessionStorage.setItem("workers", JSON.stringify(workers.value));
    };
    return {
      workers,
      editAddToggler,
      editing,
      editFormVisible,
      editClickHandler,
      editSubmit,
      addClickHandler,
      addSubmit,
      deleteWorker,
      drag,
      saveChanges,
      bar
    };
  },
  components: {
    Worker,
    FormWorker,
    draggable
  },
  mounted() {
    if (!sessionStorage.getItem("workers")) {
      const barRef = this.bar;
      barRef.start();
      axios.get("https://randomuser.me/api/?results=5").then(response => {
        for (let i = 0; i < response.data.results.length; i++) {
          let worker = response.data.results[i];
          let date = new Date(Date.parse(worker.dob.date));
          let newWorker = {
            name: worker.name.first + " " + worker.name.last,
            birthDate: date.toLocaleString("ru", {
              year: "numeric",
              month: "numeric",
              day: "numeric"
            }),
            gender: worker.gender,
            avatar: worker.picture.thumbnail,
            address:
              worker.location.street.number +
              " " +
              worker.location.street.name +
              ", " +
              worker.location.city,
            phone: worker.phone,
            email: worker.email
          };
          this.workers.push(newWorker);
        }
        sessionStorage.setItem("workers", JSON.stringify(this.workers));
        barRef.stop();
      });
    } else {
      this.workers = JSON.parse(sessionStorage.getItem("workers"));
    }
  }
};
</script>
