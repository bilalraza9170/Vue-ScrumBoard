<template>
  <div v-if="showForm" class="edit-form-overlay">
    <form @submit.prevent="handleSubmit" ref="editForm">
      <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="editedTask.name" required />
      </div>

      <div class="form-group">
        <label for="description">Description:</label>
        <textarea id="description" v-model="editedTask.description"></textarea>
      </div>

      <div class="form-group">
        <label for="assignee">Assignee:</label>
        <input type="text" id="assignee" v-model="editedTask.assignee" />
      </div>

      <div class="form-group">
        <label for="dueDate">Due Date:</label>
        <input type="date" id="dueDate" v-model="editedTask.dueDate" />
      </div>

      <div class="form-group">
        <label for="status">Status:</label>
        <select id="status" v-model="editedTask.status">
          <option value="backlog">Backlog</option>
          <option value="open">Open</option>
          <option value="new">New</option>
          <option value="in-progress">In Progress</option>
          <option value="feedback-needed">Feedback Needed</option>
          <option value="ready-for-testing">Ready For Testing</option>
          <option value="qa-in-progress">QA In Progress</option>
        </select>
      </div>

      <div class="form-group">
        <label for="spentTime">Spent Time:</label>
        <input
          type="number"
          id="spentTime"
          v-model="editedTask.spentTime"
          placeholder="Hours"
        />
      </div>

      <div class="form-group">
        <label for="priority">Priority:</label>
        <select id="priority" v-model="editedTask.priority">
          <option value="Normal">Normal</option>
          <option value="High">High</option>
          <option value="Urgent">Urgent</option>
        </select>
      </div>

      <div class="buttons">
        <button type="submit">Save Changes</button>
        <button type="button" @click="closeForm">Cancel</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "EditForm",
  props: ["task"],
  data() {
    return {
      showForm: true,
      editedTask: { ...this.task },
    };
  },
  methods: {
    handleSubmit() {
      this.$emit("update-task", this.editedTask);
      this.closeForm();
    },
    closeForm() {
      this.$emit("close");
    },
  },
  watch: {
    task(newTask) {
      this.editedTask = { ...newTask };
    },
  },
};
</script>

<style scoped>
.edit-form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input,
.form-group textarea,
.form-group select {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

.buttons {
  display: flex;
  justify-content: space-between;
}

.buttons button {
  padding: 10px 20px;
}
</style>
