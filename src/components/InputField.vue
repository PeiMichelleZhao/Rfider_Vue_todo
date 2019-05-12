<template>
  <v-form>
    <v-text-field
      v-if="!getLoading"
      v-model="newItemTitle"
      label="What do you need to do?"
      @keydown.enter.prevent="add"
      autofocus
      :rules="[rules.minLength]"
      required
    ></v-text-field>
    <div class="text-xs-center" v-if="getLoading">
      <v-progress-circular :size="30" color="primary" indeterminate></v-progress-circular>
    </div>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn color="success" @click="add" :disabled="getLoading">Save Item</v-btn>
    </v-card-actions>
  </v-form>
</template>

<script>
import { mapActions } from "vuex";
const MIN_LENGTH_VALIDATION = 3;

export default {
  name: "InputField",
  data() {
    return {
      newItemTitle: "",
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
  computed: {
    getLoading() {
      return this.$store.state.loading;
    }
  },
  methods: {
    ...mapActions(["addTodo"]),

    add() {
      if (this.newItemTitle.length >= MIN_LENGTH_VALIDATION) {
        this.addTodo(this.newItemTitle);
        this.newItemTitle = "";
      }
    }
  }
};
</script>
