<template>
  <div v-if="isVisible" class="modal-overlay">
    <div class="modal-content">
      <h3>{{ title }}</h3>
      <div v-html="content"></div>
      <button @click="close" class="btn btn-secondary">Close</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isVisible: Boolean,
    title: String,
    content: String,
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  max-width: 500px;
  width: 100%;
}
</style>
