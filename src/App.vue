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
  components: { TaskContainer, TaskList, TaskForm },

  data() {
    return {
      tasks: [],
    };
  },
  mounted() {
    this.getTaskData();
  },
  methods: {
    createTask(task) {
      this.tasks.push(task);
    },
    removeTask(task) {
      this.tasks = this.tasks.filter((p) => p.id !== task.id);
    },
    clearList() {
      this.tasks.splice(0, this.tasks.length);
    },
    getTaskData() {
      axios
        .get(process.env.VUE_APP_API_ADDRESS)
        .then((response) => {
          response.data.forEach((data) => {
            this.createTask(data);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
</style>
