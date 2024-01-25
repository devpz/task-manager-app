<template>
  <ul class="list-group mt-3">
    <TaskItem
      v-for="(task, index) in tasks"
      :key="index"
      :task="task"
      :index="index"
      @remove="removeTask"
    />
  </ul>
</template>

<script>
import TaskItem from "./TaskItem.vue";

export default {
  props: {
    tasks: Array,
  },
  components: {
    TaskItem,
  },
  methods: {
    removeTask(index) {
      this.$emit("remove", index);
    },
  },
};
</script>
