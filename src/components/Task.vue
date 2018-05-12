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
      v-on:click="editTask"
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
    columnID: String
    // handleDragTask: Function
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
    },
    editTask () {
      console.log('click to edit task')
    }
  }
}
</script>
<style lang="scss" scoped>
.btn-create{
  position: absolute;
  right: 0;
  bottom: 0;
  width: 48px;
  height: 36px;
  background: #dcdcdc url("../assets/baseline-edit-24px.svg") center center no-repeat;
  background-size: 24px;
  text-indent: -9999px;
  &:hover{
    background-color: #f0f0f0;
  }
}
</style>
