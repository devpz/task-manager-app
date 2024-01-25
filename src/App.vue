<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-6">
        <h2>Task Manager</h2>
        <TaskForm @add="addTask" />
        <TaskList :tasks="tasks" @remove="removeTask" />
      </div>
    </div>
  </div>
</template>

<script>
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";

export default {
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    addTask(newTask) {
      this.tasks.push(newTask);
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
  },
  components: {
    TaskForm,
    TaskList,
  },
};
</script>
