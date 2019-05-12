<template>
  <v-app>
    <v-flex xs12 sm6 offset-sm3>
      <v-card>
        <v-card-title primary-title class="title">
          <div>
            <h3 class="mat-h2 mb-0">Todo List</h3>
          </div>
        </v-card-title>
        <v-card-actions v-if="todos.length > 0">
          <v-btn color="success" @click="deleteSelectedItems">Delete selected Item(s)</v-btn>
        </v-card-actions>
        <v-card-text>
          <template v-for="(todo, index) in todos" >
            <v-layout :key="index">
              <v-flex sm1><v-checkbox v-model="selected[index]" sm1></v-checkbox></v-flex>
              <TodoItem :todo="todo" :index="index"></TodoItem>
            </v-layout>
          </template>
          <InputField ref="inputFeild"></InputField>
        </v-card-text>
      </v-card>
    </v-flex>

    <Message/>
  </v-app>
</template>

<script>
import TodoItem from "./components/TodoItem";
import InputField from "./components/InputField";
import Message from "./components/Message";
import { EventBus } from "./utils/EventBus";
import { mapActions } from "vuex";

export default {
  name: "App",
  components: {
    TodoItem,
    InputField,
    Message
  },
  data() {
    return {
      selected: []
    };
  },
  computed: {
    todos() {
      return this.$store.state.todos;
    }
  },
  methods: {
    ...mapActions(["deleteTodo"]),
    deleteSelectedItems() {
      if (this.selected.length > 0) {
        let indexs = [];
        this.selected.forEach((value, index) => {
          if (value === true) {
            indexs.push(index);
          }
        });

        if (indexs.length > 0) {
          EventBus.$emit("dialog:open", "todo:delete", { indexs });
        }
      }
    }
  },
  created() {
    EventBus.$on("todo:delete", ({ indexs }) => {
      indexs.sort();
      for (var i = indexs.length - 1; i >= 0; i--) {
        this.deleteTodo(this.todos[indexs[i]]);
      }
      this.selected = [];
    });
  }
};
</script>

<style>
.title {
  display: flex;
  justify-content: center;
}
</style>
