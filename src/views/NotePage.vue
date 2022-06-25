<template>
  <div>
    <div class="loading" v-if="loading">
      <p>Loading...</p>
    </div>
    <section v-else>
      <div v-if="alert.show">
        <AlertMessage v-bind:alert="alert" />
      </div>
      <article class="notePage">
        <h2>{{ note.title }}</h2>
        <section class="description-section ml-auto mr-auto mt-5">
          <span>Description:</span>
          <article class="description p-2 mt-2">
            <p>{{ note.description }}</p>
          </article>
        </section>
        <section class="mt-4">
          <button @click="edit" class="btn btn-primary btn-md mr-3 pb-0">
            Edit note
          </button>
          <button @click="deleteNote" class="btn btn-danger btn-md pb-0">
            Delete
          </button>
          <button
            @click="markComplete"
            class="btn btn-success btn-md pb-0 ml-3"
          >
            {{
              note.completed == true ? "Mark as incomplete" : "Mark as complete"
            }}
          </button>
        </section>
        <div v-if="editNote">
          <FloatingModal
            v-bind:note="note"
            v-on:updateNote="updateNote"
            v-on:cancel="cancel"
            v-bind:option="modalOption"
          />
        </div>
      </article>
    </section>
  </div>
</template>

<script>
import FloatingModal from "@/components/FloatingModal.vue";
import AlertMessage from "@/components/AlertMessage.vue";
import axios from "axios";

export default {
  name: "NotePage",
  components: {
    FloatingModal,
    AlertMessage,
  },
  data() {
    return {
      id: this.$route.params.id,
      loading: true,
      note: {},
      editNote: false,
      alert: {
        show: false,
        message: "",
        status: "",
      },
      modalOption: "update",
    };
  },
  created() {
    this.getNote();
  },

  methods: {
    async getNote() {
      const note = await fetch("http://localhost:3000/api/v1/notes/" + this.id);
      const res = await note.json();
      this.note = res;
      this.loading = false;
    },

    edit() {
      this.editNote = true;
    },

    cancel() {
      this.editNote = false;
    },

    updateNote(data) {
      this.note = data;
      this.alert.show = true;
      this.alert.message = "Note updated successfully";
      this.alert.status = "success";
    },

    deleteNote() {
      const answer = confirm("Are you sure you want to delete this note?");
      if (answer) {
        axios
          .delete("http://localhost:3000/api/v1/notes/" + this.note.id)
          .then((res) => {
            this.$router.push({
              name: "home",
              params: {
                alert: res.data.message,
              },
            });
          });
      }
    },

    markComplete() {
      axios
        .patch(`http://localhost:3000/api/v1/notes/${this.note.id}`, {
          completed: !this.note.completed,
        })
        .then(() => {
          this.getNote();
          this.editNote = false;
        });
    },
  },
};
</script>

<style scoped>
h2 {
  font-size: 45px;
}

.loading {
  height: 80vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.notePage {
  margin-top: 10vh;
}

.description-section {
  text-align: start;
  width: 300px;
}

.description {
  min-height: 150px;
  border: 1px solid rgba(124, 124, 124, 0.562);
  text-align: start;
  border-radius: 5px;
}
</style>
