<template>
  <v-layout sm6 align-start justify-space-between>
    <v-textarea
      :value="title"
      ref="input"
      v-bind:class="[todo.done ? 'is-done' : '']"
      @blur="editingCompleted"
      @keydown.enter.prevent="editingCompleted"
      @dblclick="edit"
      :readonly="isReadOnly"
      solo
      :rules="[rules.minLength]"
      :rows="1"
      auto-grow
    ></v-textarea>
    <v-btn icon @click="edit()">
      <v-icon>edit</v-icon>
    </v-btn>
    <v-btn icon @click="toggleTodo(todo)">
      <v-icon>done</v-icon>
    </v-btn>
    <v-btn icon @click="deleteItem()">
      <v-icon>delete_forever</v-icon>
    </v-btn>
  </v-layout>
</template>

<script>
import { mapActions } from "vuex";
import { EventBus } from "../utils/EventBus";

const MIN_LENGTH_VALIDATION = 3;

export default {
  name: "TodoItem",
  props: ["todo", "index"],
  data() {
    return {
      isReadOnly: true,
      rules: {
        minLength: value => {
          return (
            value.length >= MIN_LENGTH_VALIDATION ||
            `Required having at least ${MIN_LENGTH_VALIDATION} characters.`
          );
        }
      }
    };
  },
  components: {},
  computed: {
    title() {
      return this.todo.title;
    }
  },
  methods: {
    ...mapActions(["editTodo", "toggleTodo"]),
    edit() {
      this.isReadOnly = this.todo.done;
      this.$refs.input.focus();
    },
    editingCompleted(e) {
      const newTitle = e.target.value;
      const validation = newTitle.length >= MIN_LENGTH_VALIDATION;

      if (!this.isReadOnly && validation) {
        this.editTodo({ todo: this.todo, title: newTitle });
        this.isReadOnly = true;
      }
    },
    deleteItem() {
      EventBus.$emit("dialog:open", "todo:delete", { indexs: [this.index] });
    }
  }
};
</script>

<style>
.is-done {
  text-decoration: line-through;
}
</style>
