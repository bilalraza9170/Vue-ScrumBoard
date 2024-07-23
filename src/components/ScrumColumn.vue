<template>
  <div class="scrum-column">
    <h3>{{ column.title }}</h3>
    <div v-for="task in column.tasks" :key="task.id">
      <TaskCard :task="task" @edit-task="handleEditTask" />
    </div>
  </div>
</template>

<script>
import TaskCard from "./TaskCard.vue";

export default {
  name: "ScrumColumn",
  props: {
    column: {
      type: Object,
      required: true,
    },
  },
  components: {
    TaskCard,
  },
  methods: {
    handleEditTask(task) {
      this.$emit("edit-task", task); // Emit event to parent
    },
  },
};
</script>

<style scoped>
.scrum-column {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 5px;
  width: 200px;
}
</style>
