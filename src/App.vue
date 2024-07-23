<template>
  <div id="app">
    <h1 class="centered-heading">Agile Scrum Board</h1>
    <button @click="toggleForm" class="btn btn-primary">Add Task</button>
    <TaskForm @add-task="addTask" ref="taskForm" />
    <ScrumBoard :tasks="tasks" :columns="columns" @update-task="updateTask" />
  </div>
</template>

<script>
import ScrumBoard from "./components/ScrumBoard.vue";
import TaskForm from "./components/TaskForm.vue";

export default {
  components: { ScrumBoard, TaskForm },
  data() {
    return {
      tasks: [],
      columns: [
        { id: 1, title: "Backlog", tasks: [] },
        { id: 2, title: "Open", tasks: [] },
        { id: 3, title: "New", tasks: [] },
        { id: 4, title: "In Progress", tasks: [] },
        { id: 5, title: "Feedback Needed", tasks: [] },
        { id: 6, title: "Ready For Testing", tasks: [] },
        { id: 7, title: "QA In Progress", tasks: [] },
      ],
    };
  },
  methods: {
    addTask(newTask) {
      const backlogColumn = this.columns.find((column) => column.id === 1);
      if (backlogColumn) {
        backlogColumn.tasks.push(newTask);
        this.tasks.push(newTask);
        this.saveData();
      }
    },
    updateTask(updatedTask) {
      const taskIndex = this.tasks.findIndex(
        (task) => task.id === updatedTask.id
      );
      if (taskIndex !== -1) {
        this.tasks[taskIndex] = updatedTask;
        this.updateColumnTasks(updatedTask);
        this.saveData();
      }
    },
    updateColumnTasks(updatedTask) {
      this.columns.forEach((column) => {
        const taskIndex = column.tasks.findIndex(
          (task) => task.id === updatedTask.id
        );
        if (taskIndex !== -1) {
          column.tasks.splice(taskIndex, 1, updatedTask);
        }
      });
    },
    toggleForm() {
      this.$refs.taskForm.toggleForm();
    },
    saveData() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
      localStorage.setItem("columns", JSON.stringify(this.columns));
    },
    loadData() {
      const savedTasks = JSON.parse(localStorage.getItem("tasks"));
      const savedColumns = JSON.parse(localStorage.getItem("columns"));
      if (savedTasks) this.tasks = savedTasks;
      if (savedColumns) this.columns = savedColumns;
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>

<style scoped>
.centered-heading {
  text-align: center;
}
</style>
