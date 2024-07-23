<template>
  <div v-if="showForm" class="task-form-overlay">
    <form @submit.prevent="handleSubmit" ref="taskForm">
      <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="newTask.name" required />
      </div>

      <div class="form-group">
        <label for="description">Description:</label>
        <textarea id="description" v-model="newTask.description"></textarea>
      </div>

      <div class="form-group">
        <label for="assignee">Assignee:</label>
        <input type="text" id="assignee" v-model="newTask.assignee" />
      </div>

      <div class="form-group">
        <label for="dueDate">Due Date:</label>
        <input type="date" id="dueDate" v-model="newTask.dueDate" />
      </div>

      <div class="form-group">
        <label for="status">Status:</label>
        <select id="status" v-model="newTask.status">
          <option value="">Select...</option>
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
          v-model.number="newTask.spentTime"
          step="0.1"
          placeholder="Hours"
        />
      </div>

      <div class="form-group">
        <label for="priority">Priority:</label>
        <select id="priority" v-model="newTask.priority">
          <option value="">Select...</option>
          <option value="Normal">Normal</option>
          <option value="High">High</option>
          <option value="Urgent">Urgent</option>
        </select>
      </div>

      <div class="buttons">
        <button type="submit">Add Task</button>
        <button type="button" @click="closeForm">Cancel</button>
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
