<template>
  <div
    class="relative full-height"
    v-on:drop="drop"
    v-on:dragover="allowDrop"
  >
    <button
      class="btn btn-toggle-add-task btn-border"
      v-bind:class="{show: showAddTask}"
      v-on:click="toggleAddTask"
      >
      Add task
    </button>
    <div class="full-height">
      <h2
        v-bind:class="column.title.toLowerCase()"
        class="column_title"
      >
        {{ column.title }}
      </h2>
      <div class="column_content">
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
          >
      </textarea>
          <button
            v-on:click="addTask"
            class="btn column_create_btn"
          >
            Add
          </button>
        </div>
        <div
          v-for="task in column.tasks"
          v-bind:key="task.id"
        >
          <task :task="task" :columnID="column.id"></task>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Task from './Task'

export default {
  components: {Task},
  name: 'Column',
  props: {
    stages: {},
    column: {},
    handleAddTask: Function,
    handleDragTaskEnd: Function
  },
  data () {
    return {
      newTitle: '',
      newDescription: '',
      showAddTask: false
    }
  },
  mounted () {
    // console.log('props column', this)
  },
  methods: {
    addTask () {
      let _task = {
        id: Date.now().toString(),
        columnID: this.column.id,
        title: this.newTitle,
        description: this.newDescription
      }
      if (_task.title.trim() !== '' && _task.description.trim() !== '') {
        this.handleAddTask(_task, this.handleAddTaskSuccess)
      }
    },
    handleAddTaskSuccess () {
      // console.log('add success')
      this.newTitle = ''
      this.newDescription = ''
    },
    toggleAddTask () {
      let showAddTask = this.showAddTask
      this.showAddTask = !showAddTask
    },
    allowDrop (e) {
      e.preventDefault()
    },
    drop (e) {
      // console.log('from', e.dataTransfer.getData('from'))
      // console.log('taskID', e.dataTransfer.getData('taskID'))
      // console.log('to', this.column.id)
      let dataDragged = {
        from: e.dataTransfer.getData('from'),
        to: this.column.id,
        taskID: e.dataTransfer.getData('taskID')
      }
      if (dataDragged.from !== dataDragged.to) {
        this.handleDragTaskEnd(dataDragged)
      } else {
        return null
      }
    }
    // handleDragTask (event) {
    //   console.log('drag', event)
    // }
  }
}
</script>
<style lang="scss">
$color-black: #222;
.relative{
  position: relative;
  &-column{
    height: 100%;
  }
}
.full-height{
  height: 100%;
}
.btn{
  outline: none;
  border: none;
  cursor: pointer;
  font-size: 1em;
  &-border{
    border-top-left-radius: 18px;
    height: 36px;
  }
  &-toggle-add-task{
    position: absolute;
    right: 0;
    top: 0;
    width: 50px;
    height: 50px;
    border-top-left-radius: 25px;
    background: #dcdcdc url("../assets/baseline-add-24px.svg") center center no-repeat;
    text-indent: -9999px;
    transition: all 500ms ease-in;
    &.show{
      border-radius: 25px;
      transform: rotate(45deg);
    }
  }
}
.column_create{
  display: none;
  border-radius: 0 !important;
  &.show{
    display: block;
  }
  &_btn{
    padding: 8px 16px;
    background: #f0f0f0;
    transition: background-color 500ms ease-in-out;
    &:hover{
      background: #dcdcdc;
    }
  }
  &_title{
    width: 100%;
    height: 50px;
    padding: 4px 8px;
    outline: none;
    border: 1px solid rgba(22,22,22, .22);
    border-radius: 2px;
    font-size: 14px;
  }
  &_description{
    width: 100%;
    padding: 4px 8px;
    margin-top: 8px;
    outline: none;
    border: 1px solid rgba(22,22,22, .22);
    border-radius: 2px;
    min-height: 100px;
    max-height: 200px;
    font-size: 14px;
    resize: none;
  }
}

.column_title{
  /*<!--margin: -8px -8px 0;-->*/
  padding: 8px;
  &.done{
    background: #4eb87c;
    color: #fff;
  }
  &.doing{
    background: #3d5a96;
    color: #fff;
  }
  &.bug{
    background: #e22222;
    color: #fff;
  }
}

.column_content{
  white-space: normal;
  flex: 1 1 auto;
  background: #dcdcdc;
  max-height: calc(100% - 50px);
  overflow: scroll;
}
.task{
  position: relative;
  background-color: #fff;
  margin: 8px;
  padding: 8px;
  border-top-left-radius: 24px;
  box-shadow: 0 1px 2px rgba($color-black, .22);
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>
