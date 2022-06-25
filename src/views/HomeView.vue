<template>
  <div class="home">
    <section class="loading" v-if="isLoading">
      <p>Loading...</p>
    </section>
    <section v-else>
      <div v-if="showModal">
        <FloatingModal
          v-bind:note="''"
          v-on:cancel="showModal = false"
          v-bind:option="modalOption"
          v-on:getNotes="getNotes"
        />
      </div>
      <div v-if="alertInfo.show">
        <AlertMessage v-bind:alert="alertInfo" />
      </div>
      <NoteComponent v-bind:notes="notes" />
      <RoundButton v-on:openModal="showModal = true" />
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import NoteComponent from "@/components/NoteComponent.vue";
import AlertMessage from "@/components/AlertMessage.vue";
import RoundButton from "@/components/RoundButton.vue";
import FloatingModal from "@/components/FloatingModal.vue";

export default {
  name: "HomeView",
  props: {
    alert: {},
  },
  components: {
    NoteComponent,
    AlertMessage,
    RoundButton,
    FloatingModal,
  },
  data() {
    return {
      notes: [],
      isLoading: true,
      alertInfo: {
        show: false,
        message: "",
        status: "",
      },
      showModal: false,
      modalOption: "post",
    };
  },
  methods: {
    async getNotes() {
      const notes = await fetch("http://localhost:3000/api/v1/notes");
      const response = await notes.json();
      this.notes = response;
    },
  },
  async created() {
    await this.getNotes();
    this.isLoading = false;

    if (this.alert != null) {
      this.alertInfo.message = this.alert;
      this.alertInfo.show = true;
      this.alertInfo.status = "success";
    }
  },
};
</script>

<style scoped>
.loading {
  height: 92vh;
  font-size: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
