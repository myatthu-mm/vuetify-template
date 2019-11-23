<template>
  <v-container>
    <v-row>

      <v-col class="text-center" cols="12" v-show="!isCreating">
        <v-btn color="primary" @click="openForm" class="d-none d-md-inline-block">+ Create</v-btn>
        <v-btn
              fixed
              large
              fab
              bottom
              right
              color="primary"
              class="d-md-none"
              @click="openForm"
            >
              <v-icon>mdi-plus</v-icon>
            </v-btn>
      </v-col>

      <v-col class="text-center" cols="12" v-show="isCreating">
        <v-layout justify-center>
          <v-card min-width="400" outlined>
            <v-card-text>
              <v-text-field label="Title" v-model="title" outlined></v-text-field>
              <v-textarea label="Detail" rows="1" v-model="detail" auto-grow outlined></v-textarea>
              <v-spacer></v-spacer>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text @click="sendTask">Save</v-btn>
              <v-btn color="default" text @click="closeForm">Cancel</v-btn>
            </v-card-actions>
          </v-card>
        </v-layout>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "CreateTaskComponent",
  data() {
      return {
          title: '',
          detail: '',
          isCreating: false
      }
  },
  methods: {
      openForm() {
      this.isCreating = true;
    },
    closeForm() {
      this.isCreating = false;
    },
    sendTask() {
      if (this.title && this.detail) {
        const newTask = {
          title: this.title,
          detail: this.detail,
          done: false
        };
        this.$emit("create-task", newTask);
        this.title = ''
        this.detail = ''
      }
      this.isCreating = false;
    }
  },
};
</script>

<style scoped>
</style>