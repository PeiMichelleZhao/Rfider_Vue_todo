<template>
  <v-layout row justify-center>
    <v-dialog v-model="dialog" persistent max-width="420">
      <v-card>
        <v-card-title class="headline">Warning</v-card-title>
        <v-card-text>Are you sure you want to delete?</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" flat @click="submit">Confirm</v-btn>
          <v-btn color="green darken-1" flat @click="dialog = false">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
import { EventBus } from "../utils/EventBus";

export default {
  data() {
    return {
      dialog: false,
      callbackEvent: null,
      indexs: []
    };
  },
  created() {
    EventBus.$on("dialog:open", (callbackEvent, params) => {
      this.dialog = true;
      this.callbackEvent = null;
      if (callbackEvent) {
        this.callbackEvent = callbackEvent;
        this.indexs = params.indexs;
      }
    });
  },
  methods: {
    submit() {
      this.dialog = false;
      if (this.callbackEvent) {
        EventBus.$emit(this.callbackEvent, { indexs: this.indexs });
      }
    }
  }
};
</script>
