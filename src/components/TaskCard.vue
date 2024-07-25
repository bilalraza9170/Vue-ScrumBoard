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
    <button @click="showEditModal" class="btn btn-link">Edit</button>
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
      taskTitle: "",
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
              <p>Spent Time: ${this.task.spentTime}</p>
              <p>Priority: ${this.task.priority}</p>
            `;
      this.isModalVisible = true;
    },
    hideModal() {
      this.isModalVisible = false;
    },
    showEditModal() {
      // Similar to showModal, but prepare the modal for editing
      this.taskDetailsContent = `
      <form @submit.prevent="confirmEdit">
        <input v-model="task.name" placeholder="Task Name">
        <textarea v-model="task.description" placeholder="Task Description"></textarea>
        <!-- More fields as needed -->
        <button type="submit">Save Changes</button>
      </form>
    `;
      this.isModalVisible = true;
    },
    confirmEdit() {
      // Emit an event to notify the parent component about the edit
      this.$emit("edit-task", this.task);
      this.hideModal();
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
