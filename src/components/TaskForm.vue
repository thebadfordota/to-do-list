<template>
  <v-form action="" @submit.prevent>
    <v-container>
      <v-row>
        <v-col cols="12" md="10" class="d-flex justify-center align-center">
          <v-text-field
            v-model="task.body"
            @keyup.enter="createTask"
            label="New Task"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="2">
          <v-btn
            variant="outlined"
            color="teal-accent-4"
            class="btn-form-centered"
            @click="createTask"
          >Add
          </v-btn>
        </v-col>
      </v-row>
      <textarea
        class="form-textarea"
        v-model="task.aboutTask"
        @keyup.enter="createTask"
      >
        </textarea>
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
        aboutTask: '',
        checked: false,
      },
      errors: [],
    };
  },
  methods: {
    createTask() {
      if (this.checkForm()) {
        this.task.id = Date.now();
        this.$emit('create', this.task);
        this.task = {
          body: '',
          aboutTask: '',
        };
      } else {
        this.$emit('form-error', this.errors);
      }
    },
    checkForm() {
      this.errors.splice(0, this.errors.length);
      let isValid = true;
      if (this.task.body.trim() === '') {
        this.errors.push({
          id: Date.now(),
          body: 'The task body field is empty!',
        });
        isValid = false;
      }
      if (this.task.body.trim().length < 4) {
        this.errors.push({
          id: Date.now(),
          body: 'The length of the task must be at least 4 characters!',
        });
        isValid = false;
      }
      return isValid;
    },
  },
};
</script>

<style lang="sass" scoped>
.btn-form-centered
  margin-top: 10px

.form-textarea
  width: 83%
  display: inline-block
  overflow: auto
  height: 70px
  padding: 10px
  box-sizing: border-box
  border: solid 2px #1E90FF
  border-radius: 5px
  font-size: 16px
  resize: none
</style>
