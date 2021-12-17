<template>
  <div class="main columns is-marginless">
    <div class="column is-narrow sidebar is-paddingless">
      <div style="width: 300px">
        <button @click="handleClickAdd" class="button is-fullwidth is-primary">
          New Note
        </button>
        <ul>
          <li v-for="(note, index) in notes" :key="index">
            <a
              @click="handleClickNote(index)"
              class="item"
              :class="{ active: index === currentIndex }"
              href="#"
              :title="note.title"
              >{{ note.title }}
            </a>
          </li>
        </ul>
      </div>
    </div>
    <div class="column content is-paddingless" v-if="notes.length != 0">
      <input
        v-model="note.title"
        ref="title"
        type="text"
        class="note-title"
        placeholder="Title"
        @keyup="handleChange('title')"
      />
      <textarea
        class="note-content"
        placeholder="Content..."
        v-model="note.content"
        @keyup="handleChange('content')"
      >
      </textarea>
      <button
        @click="handleClickDelete"
        class="btnDelete button is-danger"
        type="button"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      notes: [],
      note: null,
      currentIndex: 0,
    };
  },
  mounted() {
    this.notes = JSON.parse(window.localStorage.getItem("notes")) || [];
    if (this.notes.length) this.note = this.notes[0];
  },
  methods: {
    handleClickNote(index) {
      this.currentIndex = index;
      this.note = this.notes[this.currentIndex];
    },
    handleClickAdd() {
      this.add({
        title: "",
        content: "",
      });
    },
    add(note) {
      this.notes.unshift(note);
      this.note = note;
    },
    handleChange(field) {
      this.notes[this.currentIndex][field] = this.note[field];
    },
    handleClickDelete() {
      this.notes.splice(this.currentIndex, 1);
      this.currentIndex = 0;
      this.note = this.notes[this.currentIndex];
    },
  },
  watch: {
    notes: {
      handler() {
        localStorage.setItem("notes", JSON.stringify(this.notes));
      },
      deep: true,
    },
  },
};
</script>
