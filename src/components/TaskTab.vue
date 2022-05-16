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
        <textarea
          class="form-textarea"
          v-model="mutableTask.aboutTask"
          v-on:change="updateAboutTask"
          @keyup.enter="updateAboutTask"
        >
        </textarea>
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
      mutableTask: {
        id: this.task.id,
        body: this.task.body,
        aboutTask: this.task.aboutTask,
        checked: this.task.checked,
      },
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
    updateAboutTask() {
      this.$emit('change-about-task', this.mutableTask);
    },
  },
};
</script>

<style lang="sass" scoped>
.tabContainer
  margin-top: 20px

.collapseContent
  padding: 0 20px
  background-color: white
  max-height: 0
  overflow: hidden
  transition: max-height 0.2s ease-out

.collapseContentText
  padding: 10px
  font-size: 16px

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
