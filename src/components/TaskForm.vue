<template>
  <form @submit.prevent="addTask">
    <div class="mb-3">
      <label for="taskInput" class="form-label">Add new task:</label>
      <input
        type="text"
        class="form-control"
        id="taskInput"
        v-model="newTask"
        required
      />
    </div>
    <button type="submit" class="btn btn-primary">Add task</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
    };
  },
  methods: {
    addTask() {
      this.$emit("add", this.newTask);
      this.newTask = "";
    },
  },
};
</script>
