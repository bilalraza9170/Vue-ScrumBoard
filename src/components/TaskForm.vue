<template>
  <div v-if="showForm" class="task-form-overlay">
    <form @submit.prevent="handleSubmit" ref="taskForm" class="form-content">
      <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="newTask.name" required />
      </div>

      <div class="form-group">
        <label for="description">Description:</label>
        <textarea
          id="description"
          v-model="newTask.description"
          required
        ></textarea>
      </div>

      <div class="form-group">
        <label for="assignee">Assignee:</label>
        <input type="text" id="assignee" v-model="newTask.assignee" required />
      </div>

      <div class="form-group">
        <label for="dueDate">Due Date:</label>
        <input
          type="date"
          id="dueDate"
          v-model="newTask.dueDate"
          required
          :min="getCurrentDate()"
        />
      </div>

      <div class="form-group">
        <label for="status">Status:</label>
        <select id="status" v-model="newTask.status" required>
          <option value="backlog">Backlog</option>
          <option value="open" disabled>Open</option>
          <option value="new" disabled>New</option>
          <option value="in-progress" disabled>In Progress</option>
          <option value="feedback-needed" disabled>Feedback Needed</option>
          <option value="ready-for-testing" disabled>Ready For Testing</option>
          <option value="qa-in-progress" disabled>QA In Progress</option>
        </select>
      </div>

      <div class="form-group">
        <label for="spentTime">Spent Time:</label>
        <input
          type="number"
          id="spentTime"
          v-model.number="newTask.spentTime"
          step="0.1"
          min="0"
          placeholder="Hours"
          required
        />
      </div>

      <div class="form-group">
        <label for="priority">Priority:</label>
        <select id="priority" v-model="newTask.priority" required>
          <option value="">Select...</option>
          <option value="Normal">Normal</option>
          <option value="High">High</option>
          <option value="Urgent">Urgent</option>
        </select>
      </div>

      <div class="buttons">
        <button type="button" @click="closeForm">Cancel</button>
        <button type="submit">Add Task</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "TaskForm",
  data() {
    return {
      showForm: false,
      newTask: {
        name: "",
        description: "",
        assignee: "",
        dueDate: "",
        status: "",
        spentTime: "",
        priority: "",
      },
    };
  },
  methods: {
    handleSubmit() {
      const newTask = {
        id: Math.random(),
        name: this.newTask.name,
        description: this.newTask.description,
        assignee: this.newTask.assignee,
        dueDate: this.newTask.dueDate,
        status: "backlog",
        spentTime: this.newTask.spentTime,
        priority: this.newTask.priority,
      };

      this.$emit("add-task", newTask);

      this.closeForm();
    },
    toggleForm() {
      this.showForm = !this.showForm;
    },
    closeForm() {
      this.showForm = false;
    },
    getCurrentDate() {
      // Create a new Date object for the current date and time
      let currentDate = new Date();

      // Format the date as a string in YYYY-MM-DD format
      // Note: The toISOString() method returns a string in simplified extended ISO format (ISO 8601), which is always 24 or 27 characters long (YYYY-MM-DDTHH:mm:ss.sssZ or ±YYYYYY-MM-DDTHH:mm:ss.sssZ)
      // We then slice it to get only the date part (YYYY-MM-DD)
      let formattedDate = currentDate.toISOString().slice(0, 10);

      // Return the formatted date string
      return formattedDate;
    },
  },
};
</script>

<style scoped>
.task-form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}
.form-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  max-width: 500px;
  width: 100%;
}
.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.buttons {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

input[type="text"],
input[type="date"],
textarea {
  width: 100%;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

select {
  width: 100%;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #007bff;
  color: white;
}
</style>
