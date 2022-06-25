<template>
  <div v-for="(note, id) in notes" v-bind:key="id">
    <router-link
      v-bind:to="'/note/' + note.id"
      class="note"
      v-bind:class="note.completed ? 'completed' : ''"
    >
      <span>{{ note.title }}</span>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "NoteCompoent",
  props: {
    notes: {},
  },
};
</script>

<style scoped>
.note {
  background: rgba(131, 247, 193, 0.397);
  min-width: 300px;
  max-width: 90vw;
  height: 70px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px;
  margin: 20px auto;
  text-decoration: none;
  color: black;
}

.completed {
  text-decoration: line-through;
}

.note:hover {
  background: rgba(140, 250, 199, 0.644);
}

.note > span {
  font-size: 18px;
  font-weight: 600;
}
</style>
