<template>
  <li class="list-group-item">
    {{ task }}
    <button class="btn btn-danger btn-sm float-end" @click="removeTask">
      Delete
    </button>
  </li>
</template>

<script>
export default {
  props: {
    task: String,
    index: Number,
  },
  methods: {
    removeTask() {
      this.$emit("remove", this.index);
    },
  },
};
</script>
