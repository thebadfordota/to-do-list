<template>
  <v-form @submit.prevent>
    <v-row>
      <v-col cols="12" md="1">
        <v-checkbox
            color="red"
            v-model="mutableTask.checked"
            v-on:change="updateTask"
        ></v-checkbox>
      </v-col>
      <v-col cols="12" md="9">
        <v-text-field
            v-on:change="updateTask"
            v-model="mutableTask.body"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="1">
        <v-btn
          color="teal-accent-4"
          icon="mdi-plus"
          class="btn-icon-centered"
          @click="$emit('changeCollapse')"
        ></v-btn>
      </v-col>
      <v-col cols="12" md="1">
        <v-btn
            color="red"
            icon="mdi-delete-forever"
            class="btn-icon-centered"
            @click="deleteTask"
        ></v-btn>
      </v-col>
    </v-row>
  </v-form>
</template>

<script>
export default {
  name: 'task-item',
  props: {
    task: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      mutableTask: {
        id: this.task.id,
        body: this.task.body,
        checked: this.task.checked,
      },
    };
  },
  mounted() {
  },
  methods: {
    updateTask() {
      this.$emit('update', this.mutableTask);
    },
    deleteTask() {
      this.$emit('remove', this.mutableTask);
    },
  },
};
</script>

<style lang="sass" scoped>
.btn-icon-centered
  margin-top: 5px
</style>
