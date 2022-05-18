<template>
  <v-app>
    <v-main>
      <task-container @clear="clearList">
        <!--Errors        -->
        <div v-if="formErrors.length > 0">
          <div
            class="error-alert"
            v-for="error in formErrors"
            :key="error.id"
          >
            {{ error.body }}
          </div>
        </div>
        <!--CheckBox        -->
        <label class="check option">
          <input
            type="checkbox"
            class="check__input"
            v-model="arrayIsReversed"
            @change="tasks.reverse()"
          >
          <span class="check__box"></span>
          <span class="check__info"> In reverse order </span>
        </label>
        <!-- Create Task        -->
        <task-form
          @create="createTask"
          @form-error="getFormErrors"
        ></task-form>
        <!--List          -->
        <task-list
          :tasks="tasks"
          @remove="removeTask"
          @update="updateTask"
          @change-tasks-list-position="changeTaskListPosition"
          @change-about-task="updateTask"
        ></task-list>
      </task-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
import TaskForm from '@/components/TaskForm.vue';
import TaskList from '@/components/TaskList.vue';
import TaskContainer from '@/components/TaskContainer.vue';

export default {
  name: 'App',
  components: {
    TaskForm,
    TaskList,
    TaskContainer,
  },

  data() {
    return {
      tasks: [],
      dbUrl: process.env.VUE_APP_API_ADDRESS,
      formErrors: [],
      arrayIsReversed: false,
    };
  },
  mounted() {
    this.getTaskData();
  },
  methods: {
    createTask(task) {
      if (this.arrayIsReversed) {
        this.tasks = [task, ...this.tasks];
      } else {
        this.tasks.push(task);
      }
      axios.post(this.dbUrl, task);
    },
    removeTask(task) {
      this.tasks = this.tasks.filter((p) => p.id !== task.id);
      axios.delete(`${this.dbUrl}/${task.id}`);
    },
    clearList() {
      this.tasks.forEach((data) => {
        axios.delete(`${this.dbUrl}/${data.id}`);
      });
      this.tasks.splice(0, this.tasks.length);
    },
    updateTask(task) {
      axios.put(`${this.dbUrl}/${task.id}`, task);
    },
    getTaskData() {
      axios
        .get(this.dbUrl)
        .then((response) => {
          response.data.forEach((data) => {
            this.tasks.push(data);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    changeTaskListPosition() {
      this.tasks.forEach((data) => {
        axios.delete(`${this.dbUrl}/${data.id}`);
      });
      let newTasks = this.tasks.map((data) => {
        return {...data}
      });
      if (this.arrayIsReversed) {
        newTasks.reverse();
      }
      newTasks.forEach((task) => {
        axios.post(this.dbUrl, task);
      });
    },
    getFormErrors(errors) {
      this.formErrors = errors;
    },
  },
};
</script>

<style lang="sass">

.error-alert
  position: relative
  padding: 0.75rem 1.25rem
  margin-bottom: 1rem
  border: 1px solid transparent
  color: #721c24
  background-color: #f8d7da

.option
  display: block
  margin-bottom: 0.5em
  margin-top: 10px
  max-width: 350px
  margin-left: 33px
  font-size: 18px

.check
  position: relative
  left: 0

.check__input
  position: absolute
  width: 1px
  height: 1px
  overflow: hidden
  clip: rect(0 0 0 0)

.check__box
  position: absolute
  margin-top: 0.2em
  margin-left: -1em
  width: 1em
  height: 1em
  overflow: hidden
  border-radius: 0.05em
  background-color: white
  background-repeat: no-repeat
  background-position: 50% 50%
  box-shadow: 0 0 0 0.1em #4A90E2

.check__info
  margin-left: 5%

.check__input:checked + .check__box
  background-color: #4A90E2
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 12 12'%3E%3Cpolyline fill='none' stroke='%23fff' stroke-width='3' points='1.079 4.999 5 9 10.837 2'/%3E%3C/svg%3E")

.check__input:focus + .check__box
  box-shadow: 0 0 0 0.1em #4A90E2, 0 0 0 0.2em #7ED321

</style>
