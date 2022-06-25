<template>
  <div class="floatingModal">
    <form class="form" method="post">
      <div class="form-group">
        <label for="title text-left">Title:</label>
        <input v-model="title" class="form-control" name="title" type="text" />
      </div>
      <div class="form-group">
        <label for="description">Description:</label>
        <textarea
          v-model="description"
          name="description"
          class="form-control"
          cols="30"
        >
        </textarea>
      </div>
    </form>
    <div class="form-group m-2 buttons">
      <button @click="validateMethod" value="Save" class="btn btn-primary">
        Save
      </button>
      <button @click="cancelModal" class="btn btn-danger ml-3">Cancel</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "FloatingModal",
  props: {
    note: {},
    option: String,
  },
  data() {
    return {
      title: this.note.title,
      description: this.note.description,
    };
  },
  methods: {
    cancelModal() {
      this.$emit("cancel");
    },

    update() {
      const url = "http://localhost:3000/api/v1/notes/" + this.note.id;
      axios
        .patch(url, {
          title: this.title,
          description: this.description,
        })
        .then((res) => {
          this.cancelModal();
          this.reloadNote(res.data);
        });
    },
    reloadNote(data) {
      this.$emit("updateNote", data);
    },
    post() {
      axios
        .post("http://localhost:3000/api/v1/notes", {
          title: this.title,
          description: this.description,
        })
        .then(() => {
          this.cancelModal();
          this.$emit("getNotes");
        });
    },
    validateMethod() {
      if (this.option == "update") {
        this.update();
      } else {
        this.post();
      }
    },
  },
};
</script>

<style scoped>
.floatingModal {
  width: 400px;
  height: 300px;
  position: absolute;
  top: 34%;
  left: 34%;
  background-color: rgb(211, 212, 212);
  border: 1px solid rgba(136, 136, 136, 0.548);
  border-radius: 8px;
  padding: 20px;
}

.form {
  text-align: start;
}

.form label {
  margin: auto 5px;
  font-size: 18px;
}

.form textarea {
  height: 90px;
}

.buttons {
  float: inline-end;
}

@media (max-width: 810px) {
  .floatingModal {
    width: 500px;
    height: 400px;
    top: 30vh;
    left: 19%;
  }

  .form {
    padding: 20px;
  }
  .form label {
    font-size: 25px;
  }

  .form textarea {
    height: 110px;
    font-size: 20px;
  }

  .form input {
    height: 50px;
    font-size: 20px;
  }
}

@media (max-width: 480px) {
  .floatingModal {
    top: 28vh;
    left: 12vw;
    right: 12vw;
    width: 320px;
    height: 330px;
  }

  .form {
    padding: 5px;
  }

  .form label {
    font-size: 20px;
  }

  .form textarea {
    height: 90px;
    font-size: 18px;
  }

  .form input {
    height: 45px;
    font-size: 18px;
  }
}

@media (max-width: 360px) {
  .floatingModal {
    left: 5vw;
    right: 5vw;
  }
}
</style>
