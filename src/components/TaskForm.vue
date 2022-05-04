<template>
  <v-form action="" @submit.prevent>
    <v-container>
      <v-row>
        <v-col cols="12" md="10" class="d-flex justify-center align-center">
          <v-text-field
              v-model="task.body"
              @keyup.enter="createTask"
              label="First name"
              required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="2">
          <v-btn
              variant="outlined"
              color="teal-accent-4"
              class="btn-form-centered"
              @click="createTask"
          >Add</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: 'task-form',
  data() {
    return {
      task: {
        id: '',
        body: '',
        checked: false,
      },
    };
  },
  methods: {
    createTask() {
      this.task.id = Date.now();
      this.$emit('create', this.task);
      this.task = { body: '' };
    },
  },
};
</script>

<style scoped>
.btn-form-centered {
  margin-top: 10px;
}
</style>
