<template>
  <base-layout>
    <h1>Notes app</h1>
    <form ref="noteForm" class="d-flex flex-column my-5">
      <input type="text" v-model="form.name" class="my-1">
      <textarea v-model="form.text" cols="30" rows="10" class="my-1"></textarea>
      <button type="submit" class="btn btn-primary" @click="createCard">Отправить</button>
    </form>
    <div class="row row-cols-1 row-cols-md-2 g-4 mt-5">
      <div class="col" v-for="note in notes" :key="note.id">
        <note-card :name="note.name" :text="note.text" />
      </div>
    </div>
  </base-layout>
</template>

<script>
import { mapActions, mapState } from 'pinia'
import useNotesStore from '@/stores/notes'
import BaseLayout from '@/layouts/BaseLayout.vue'
import NoteCard from '@/components/NoteCard.vue'

export default {
  name: 'NotesPage',
  components: { BaseLayout, NoteCard },
  computed: {
    ...mapState(useNotesStore, ['notes'])
  },
  methods: {
    ...mapActions(useNotesStore, ['loadNotes', 'createNote']),
    async createCard() {
      await this.createNote(this.form)
      await this.loadNotes()


      this.$refs.noteForm.reset()
    }
  },
  data() {
    return {
      form: {
        name: '',
        text: '',
        userId: 1
      }
    }
  },
  mounted() {
    this.loadNotes();
  }
}
</script>
