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
    };
  },
  mounted() {
    this.getTaskData();
  },
  methods: {
    createTask(task) {
      this.tasks.push(task);
      axios.post(process.env.VUE_APP_API_ADDRESS, task);
    },
    removeTask(task) {
      this.tasks = this.tasks.filter((p) => p.id !== task.id);
      axios.delete(`${process.env.VUE_APP_API_ADDRESS}/${task.id}`);
    },
    clearList() {
      this.tasks.forEach((data) => {
        axios.delete(`${process.env.VUE_APP_API_ADDRESS}/${data.id}`);
      });
      this.tasks.splice(0, this.tasks.length);
    },
    updateTask(task) {
      axios.put(`${process.env.VUE_APP_API_ADDRESS}/${task.id}`, task);
    },
    getTaskData() {
      axios
        .get(process.env.VUE_APP_API_ADDRESS)
        .then((response) => {
          response.data.forEach((data) => {
            this.tasks.push(data);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    changeTaskListPosition(event) {
      const firstElement = this.tasks[event.moved.oldIndex];
      const firstIndex = firstElement.id;
      const secondElement = this.tasks[event.moved.newIndex];
      const secondIndex = secondElement.id;
      console.log(firstIndex);
      console.log(secondIndex);
      axios
        .put(`${process.env.VUE_APP_API_ADDRESS}/${firstIndex}`, secondElement);
      axios
        .put(`${process.env.VUE_APP_API_ADDRESS}/${secondIndex}`, firstElement);
      // console.log(event.moved);
    },
  },
};
</script>

<style>
</style>
