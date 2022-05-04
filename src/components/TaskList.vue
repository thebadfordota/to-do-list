<template>
  <div v-if="tasks.length > 0">
  <task-item
      v-for="task in tasks"
      :task="task"
      :key="task.id"
      @remove="$emit('remove', task)"
      @update="updateTask"
  ></task-item>
  </div>
  <h2 v-else>No new tasks</h2>
</template>

<script>
import TaskItem from '@/components/TaskItem.vue';

export default {
  name: 'task-list',
  components: { TaskItem },

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
  },
};
</script>

<style scoped>

</style>
