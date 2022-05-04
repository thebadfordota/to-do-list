<template>
  <v-form @submit.prevent>
    <v-row>
      <v-col cols="12" md="1">
        <v-checkbox
            color="red"
            v-model="mutableTask.checked"
        ></v-checkbox>
      </v-col>
      <v-col cols="12" md="9">
        <v-text-field
            v-model="mutableTask.body"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="2">
        <v-btn
            color="red"
            icon="mdi-delete-forever"
            class="btn-icon-centered"
            @click="$emit('remove', mutableTask)"
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

  },
};
</script>

<style scoped>
.btn-icon-centered {
  margin-top: 5px;
}
</style>
