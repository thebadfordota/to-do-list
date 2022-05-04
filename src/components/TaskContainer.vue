<template>
  <v-card class="mx-auto main_container" max-width="70%">
    <v-card-header>
      <v-card-header-text>
        <v-card-title class="text-h5">To-do-list</v-card-title>
        <v-card-subtitle>
        </v-card-subtitle>
      </v-card-header-text>
    </v-card-header>

    <v-card-text>
      <!-- Card content         -->
      <slot></slot>
    </v-card-text>

    <v-card-actions>
      <v-btn
          variant="text"
          color="teal-accent-4"
          @click="$emit('clear')"
      >Clear list</v-btn>
    </v-card-actions>

  </v-card>
</template>

<script>
export default {
  name: 'task-container',
  methods: {

  },
};
</script>

<style scoped>
.main_container {
  margin-top: 55px;
}
</style>
