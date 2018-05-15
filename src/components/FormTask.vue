<template>
  <div
    class="task column_create"
    v-bind:class="{show: showAddTask}"
  >
    <input
      type="text"
      placeholder="enter title"
      class="column_create_title"
      v-model="newTitle"
    />
    <textarea
      placeholder="enter description"
      class="column_create_description"
      v-model="newDescription"
    ></textarea>
    <button
      v-on:click="handleSubmit(task.columnID, task.id, newTitle, newDescription)"
      class="btn column_create_btn btn-submit"
    >
      Save
    </button>
    <button
      v-on:click="handleCloseDialog"
      class="btn column_create_btn"
    >
      Cancel
    </button>
  </div>
</template>
<script>
export default {
  name: 'FormTask',
  props: {
    task: {},
    handleSubmit: Function,
    handleCloseDialog: Function
  },
  data () {
    return {
      showAddTask: true,
      newTitle: this.task.title,
      newDescription: this.task.description
    }
  }
}
</script>
<style lang="scss" scoped>
  @import "../_default.scss";
  .btn-submit{
    background-color: $color-green;
    color: $color-white;
  }
</style>
