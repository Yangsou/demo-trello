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
      class="btn column_create_btn"
    >
      Save
    </button>
  </div>
</template>
<script>
export default {
  name: 'FormTask',
  props: {
    task: {},
    handleSubmit: Function
  },
  data () {
    return {
      showAddTask: true,
      newTitle: this.task.title,
      newDescription: this.task.description
    }
  },
  mounted () {
    // let _task = this.task
    // this.task = _task
  },
  methods: {
    // handleChangeTitle () {
    //   console.log('change')
    // },
    // handleChangeDescription () {
    //   console.log('change')
    // }
  }
}
</script>
