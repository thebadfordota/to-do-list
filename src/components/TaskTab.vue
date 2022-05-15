<template>
  <div class="tabContainer">
    <task-item
      :task="task"
      @update="updateTask"
      @remove="$emit('remove', task)"
      @changeCollapse="changeCollapse"
    ></task-item>
    <div
      class="collapseContent"
      :ref="`${refLink}`"
      style="max-height: 0;"
    >
      <div class="collapseContentText">
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
          ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercit
          ullamc.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import TaskItem from '@/components/TaskItem.vue';

export default {
  name: 'task-tab',
  components: {
    TaskItem,
  },
  props: {
    task: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      idTmp: this.task.id,
      refLink: '',
      collapseMaxHeight: 100,
    };
  },
  mounted() {
    this.refLink = `tab-${this.idTmp}`;
  },
  methods: {
    updateTask(mutableTask) {
      this.$emit('update', mutableTask);
    },
    changeCollapse() {
      if (this.$refs[this.refLink].style.maxHeight === '0px') {
        this.$refs[this.refLink].style.maxHeight = `${this.collapseMaxHeight}px`;
      } else {
        this.$refs[this.refLink].style.maxHeight = '0';
      }
    },
  },
};
</script>

<style scoped>
.tabContainer {
  margin-top: 20px;
}

.collapseContent {
  padding: 0 20px;
  background-color: white;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-out;
}

.collapseContentText {
  padding: 10px;
  font-size: 16px;
  background: #e9ecef;
}
</style>
