<template>
  <v-app>
    <v-main>
      <task-container @clear="clearList">
        <!-- Create Task        -->
        <task-form
          @create="createTask"
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
    };
  },
  mounted() {
    this.getTaskData();
  },
  methods: {
    createTask(task) {
      this.tasks.push(task);
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
    changeTaskListPosition(newTasks) {
      this.tasks.forEach((data) => {
        axios.delete(`${this.dbUrl}/${data.id}`);
      });
      newTasks.forEach((task) => {
        axios.post(this.dbUrl, task);
      });
    },
    updateAboutTask() {

    },
  },
};
</script>

<style>
</style>
