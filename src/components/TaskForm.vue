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
          >Add
          </v-btn>
        </v-col>
      </v-row>
      <!--      <v-row>-->
      <!--        <v-col cols="12" md="10" class="d-flex">-->
<!--      <div class="form-textarea">-->
<!--        <label for="about-task"></label>-->
<!--        <textarea id="about-task" name="about-task">-->
<!--        </textarea>-->
<!--      </div>-->
      <!--        </v-col>-->
      <!--      </v-row>-->
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
      this.task = {
        body: '',
      };
    },
  },
};
</script>

<style lang="sass" scoped>
.btn-form-centered
  margin-top: 10px

.form-textarea
  width: auto
  //display: inline-block
  max-width: 100%
  overflow: auto
  height: 70px
  padding: 10px
  box-sizing: border-box
  border: solid 2px #1E90FF
  border-radius: 5px
  font-size: 16px
  resize: none

</style>
