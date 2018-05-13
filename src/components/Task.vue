<template>
  <div
    class="task"
    draggable="true"
    v-on:dragstart="handleDragTask"
  >
    <h3>{{ task.title }}</h3>
    <p>{{ task.description }}</p>
    <button
      class="btn btn-border btn-create"
      v-on:click="handleClickEdit(task.id, columnID)"
    >
      click
    </button>
    <button
      class="btn btn-border btn-delete-task"
      v-on:click="handleDeleteTask(task.id, columnID)"
    >
      click
    </button>
  </div>
</template>
<script>
export default {
  name: 'Task',
  props: {
    task: {},
    columnID: String,
    handleClickEdit: Function,
    handleDeleteTask: Function
  },
  data () {
    return {}
  },
  mounted () {
    // console.log('props', this)
  },
  methods: {
    handleDragTask (event) {
      // console.log('drag', event)
      var _task = this.task
      event.dataTransfer.setData('from', this.columnID)
      event.dataTransfer.setData('taskID', _task.id)
    }
  }
}
</script>
<style lang="scss" scoped>
.btn-create,
.btn-delete-task{
  position: absolute;
  width: 48px;
  height: 36px;
  background: #dcdcdc url("../assets/baseline-edit-24px.svg") center center no-repeat;
  background-size: 20px;
  text-indent: -9999px;
  &:hover{
    background-color: #f0f0f0;
  }
}
.btn-create{
  right: 48px;
  bottom: 0;
}
.btn-delete-task{
  right: 0;
  bottom: 0;
  border-radius: 0 !important;
  background-image: url('../assets/baseline-delete-24px.svg');
}
</style>
