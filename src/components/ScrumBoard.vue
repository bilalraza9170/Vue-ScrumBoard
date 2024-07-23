<template>
  <button @click="exportTasks" class="btn btn-secondary">Export Tasks</button>
  <button @click="importTasks" class="btn btn-secondary">Import Tasks</button>
  <div class="scrum-board-boundary">
    <div class="scrum-board">
      <ScrumColumn
        v-for="column in columns"
        :key="column.id"
        :column="column"
        @update-task="updateTaskInColumn"
      />
    </div>
  </div>
</template>

<script>
import ScrumColumn from "./ScrumColumn.vue";

export default {
  components: { ScrumColumn },
  props: {
    tasks: Array,
    columns: Array,
  },
  methods: {
    updateTaskInColumn(updatedTask) {
      this.$emit("update-task", updatedTask);
    },
    saveTasks() {
      localStorage.setItem("columns", JSON.stringify(this.columns));
    },
    exportTasks() {
      const dataStr =
        "data:text/json;charset=utf-8," +
        encodeURIComponent(JSON.stringify(this.tasks));
      const downloadAnchorNode = document.createElement("a");
      downloadAnchorNode.setAttribute("href", dataStr);
      downloadAnchorNode.setAttribute("download", "tasks.json");
      document.body.appendChild(downloadAnchorNode);
      downloadAnchorNode.click();
      downloadAnchorNode.remove();
    },
    importTasks(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        const importedTasks = JSON.parse(e.target.result);
        this.$emit("import-tasks", importedTasks);
      };
      reader.readAsText(file);
    },
  },
};
</script>

<style scoped>
.scrum-board-boundary {
  /* Styles for the boundary */
  border: 2px solid #333; /* Example: A dark gray border */
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Optional: Adds a subtle shadow */
  padding: 20px; /* Adds some space inside the boundary */
  max-width: 100%; /* Ensures the boundary does not exceed its parent's width */
  margin: auto; /* Centers the boundary if it's narrower than its parent */
}

.scrum-board {
  display: flex;
  justify-content: space-around;
}

button {
margin: 10px;
  padding: 8px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #007bff;
  color: white;
}
</style>
