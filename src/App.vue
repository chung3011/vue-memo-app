<template>
  <div class="main columns is-marginless">
    <div class="column is-narrow sidebar is-paddingless">
      <div style="width: 300px">
        <button @click="handleClickAdd" class="button is-fullwidth is-primary">
          New Note
        </button>
        <ul>
          <li v-for="note in notes" :key="note.id">
            <a
              @click="handleClickNote(note)"
              class="item"
              :class="{ active: note === currentNote }"
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
        v-model="currentNote.title"
        ref="title"
        type="text"
        class="note-title"
        placeholder="Title"
        @keyup="handleChange('title')"
      />
      <textarea
        class="note-content"
        placeholder="Content..."
        v-model="currentNote.content"
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
import { v4 as uuid } from "uuid";

export default {
  name: "App",
  data() {
    return {
      notes: [],
      currentNote: null,
    };
  },
  mounted() {
    this.notes = JSON.parse(window.localStorage.getItem("notes")) || [];
    if (this.notes.length) this.currentNote = this.notes[0];
  },
  methods: {
    updateData(data) {
      localStorage.setItem("notes", JSON.stringify(data));
    },
    handleClickNote(note) {
      this.currentNote = note;
    },
    handleClickAdd() {
      let newNote = {
        id: uuid(),
        title: "",
        content: "",
      }
      this.notes.unshift(newNote);
      this.currentNote = newNote;
      this.updateData(this.notes);
    },
    handleChange(field) {
      this.notes.find(note => note.id == this.currentNote.id )[field] = this.currentNote[field];
      this.updateData(this.notes);
    },
    handleClickDelete() {
      let index = this.notes.findIndex(note => note.id == this.currentNote.id )
      this.notes.splice(index, 1);
      this.currentNote = this.notes[0];
      this.updateData(this.notes);
    },
  },
};
</script>
