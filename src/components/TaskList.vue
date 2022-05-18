<template>
  <div v-if="tasks.length > 0">
    <draggable
      :list="tasks"
      @change="changeTaskListPosition"
    >
      <task-tab
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @remove="$emit('remove', task)"
        @update="updateTask"
        @change-about-task="updateAboutTask"
      >
      </task-tab>
    </draggable>
  </div>
  <h2 v-else>No new tasks</h2>
</template>

<script>
import TaskTab from '@/components/TaskTab.vue';
import { VueDraggableNext } from 'vue-draggable-next';

export default {
  name: 'task-list',
  components: {
    draggable: VueDraggableNext,
    TaskTab,
  },
  props: {
    tasks: {
      type: Array,
      required: true,
    },
  },
  methods: {
    updateTask(mutableTask) {
      this.$emit('update', mutableTask);
    },
    changeTaskListPosition() {
      this.$emit('change-tasks-list-position');
    },
    updateAboutTask(mutableAboutTask) {
      this.$emit('change-about-task', mutableAboutTask);
    },
  },
};
</script>

<style scoped>

</style>
