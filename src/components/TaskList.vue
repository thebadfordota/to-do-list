<template>
  <div v-if="tasks.length > 0">
    <draggable
      class="dragArea list-group w-full"
      :list="tasks"
      @change="changeTaskListPosition"
    >
      <task-tab
        v-for="task in tasks"
        :task="task"
        :key="task.id"
        @remove="$emit('remove', task)"
        @update="updateTask"
      >
      </task-tab>
    </draggable>
  </div>
  <h2 v-else>No new tasks</h2>
</template>

<script>
// import TaskItem from '@/components/TaskItem.vue';
import TaskTab from '@/components/TaskTab.vue';
// import { defineComponent } from 'vue';
import { VueDraggableNext } from 'vue-draggable-next';
// import draggable from 'vuedraggable';

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
  data() {
    return {
      // mutableTasks: this.tasks.map((a) => ({ ...a })),
      mutableTasks: this.tasks,
      // mutableTasks: [],
    };
  },
  watch: {

  },
  mounted() {
    this.mutableTasks = structuredClone(this.tasks);
    // this.created();
  },
  methods: {
    updateTask(mutableTask) {
      this.$emit('update', mutableTask);
    },
    changeTaskListPosition(event) {
      // console.log(event);
      this.$emit('change-tasks-list-position', event);
    },
  },
};
</script>

<style scoped>

</style>
