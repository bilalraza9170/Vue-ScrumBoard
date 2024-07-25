<template>
  <div class="scrum-column">
    <h3>{{ column.title }}</h3>
    <!-- Check if the current column is the backlog column -->
    <input
      v-if="column.title == 'Backlog'"
      type="text"
      id="searchBar"
      placeholder="Search..."
    />
    <ul id="itemList">
      <!-- Dynamically populated with items -->
    </ul>
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
    handleEditTask(updatedTask) {
      const index = this.tasks.findIndex((t) => t.id === updatedTask.id);
      if (index !== -1) {
        this.tasks[index] = updatedTask;
      }
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

/* styles.css */
#searchBar {
  width: 180px;
  height: 30px;
  padding: 5px;
  font-size: 16px;
}

#itemList {
  list-style-type: none;
  padding: 0;
}

.item {
  border: 1px solid #ccc;
  margin: 10px 0;
  padding: 10px;
}
</style>
