<template>
  <div
    @click="showModal"
    class="task-card"
    :style="{ 'background-color': priorityColor }"
  >
    <p>Name: {{ task.name }}</p>
    <p>Description: {{ task.description }}</p>
    <p>Priority: {{ task.priority }}</p>
    <p>Assignee: {{ task.assignee }}</p>
    <button @click.stop="editTask" class="btn btn-link">Edit</button>
    <Modal
      :isVisible="isModalVisible"
      :title="'Task Details'"
      :content="taskDetailsContent"
      @click.stop="hideModal"
    />
  </div>
</template>

<script>
import Modal from "./PopUp.vue";

export default {
  name: "TaskCard",
  props: ["task"],
  components: {
    Modal,
  },
  data() {
    return {
      isModalVisible: false,
      taskDetailsContent: "",
    };
  },
  computed: {
    priorityColor() {
      switch (this.task.priority) {
        case "Normal":
          return "rgb(208,254,240)";
        case "High":
          return "rgb(248,219,187)";
        case "Urgent":
          return "rgb(249,226,228)";
        default:
          return "";
      }
    },
  },
  methods: {
    showModal() {
      this.taskDetailsContent = `
              <p>Name: ${this.task.name}</p>
              <p>Description: ${this.task.description}</p>
              <p>Assignee: ${this.task.assignee}</p>
              <p>Status: ${this.task.status}</p>
              <p>Due Date: ${this.task.dueDate}</p>
              <p>Priority: ${this.task.priority}</p>
            `;
      this.isModalVisible = true;
    },
    hideModal() {
      this.isModalVisible = false;
    },
    editTask() {
      this.$emit("edit-task", this.task); // Emit event with the task
    },
  },
};
</script>

<style scoped>
.task-card {
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer; /* Optional: indicates clickable area */
}
</style>
