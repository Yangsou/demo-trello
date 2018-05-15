<template>
  <div class="container">
    <div class="wrap">
      <div
        v-for="column in columns"
        v-bind:key="column.id"
        class="column"
      >
        <Column
          :column="column"
          :handleAddTask="handleAddTask"
          :handleDragTaskEnd="handleDragTaskEnd"
          :handleClickEdit="handleClickEdit"
          :handleDeleteColumn="handleDeleteColumn"
          :handleDeleteTask="handleDeleteTask"
        ></Column>
      </div>
      <div class="column">
        <div class="relative full-height">
          <input
            type="text"
            placeholder="Add new column"
            class="column_title column_title-create"
            v-model="newColumnTitle"
            v-on:keyup.enter="handleAddColumn"
          />
          <button
            class="btn btn-border btn-add-column"
            v-on:click="handleAddColumn"
          >
            add column
          </button>
        </div>
      </div>
    </div>
    <div class="dialog" v-if="showForm">
      <div class="dialog_container">
        <FormTask
          :task="taskEdit"
          :handleSubmit="handleSubmit"
          :handleCloseDialog="handleCloseDialog"
        ></FormTask>
      </div>
    </div>
  </div>
</template>

<script>
import Column from './Column'
import FormTask from './FormTask'

export default {
  components: {Column, FormTask},
  name: 'HelloWorld',
  data () {
    return {
      columns: [
        {
          id: '1',
          title: 'Doing',
          tasks: [
            {
              id: '11',
              title: 'building layout',
              description: 'Curabitur aliquet quam id dui posuere blandit. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus.'
            },
            {
              id: '12',
              title: 'task default',
              description: 'Curabitur aliquet quam id dui posuere blandit. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus.'
            }
          ]
        },
        {
          id: '2',
          title: 'Done',
          tasks: [
            {
              id: '21',
              title: 'schedule project',
              description: 'Curabitur aliquet quam id dui posuere blandit. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus.'
            },
            {
              id: '22',
              title: 'task default',
              description: 'Curabitur aliquet quam id dui posuere blandit. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus.'
            }
          ]
        },
        {
          id: '3',
          title: 'Bug',
          tasks: [
            {
              id: '31',
              title: 'schedule project',
              description: 'Curabitur aliquet quam id dui posuere blandit. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus.'
            },
            {
              id: '32',
              title: 'task default',
              description: 'Curabitur aliquet quam id dui posuere blandit. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus.'
            }
          ]
        }
      ],
      newColumnTitle: '',
      showForm: false,
      taskEdit: {}
    }
  },
  methods: {
    handleAddColumn () {
      let title = this.newColumnTitle
      let newColumn = {
        id: Date.now().toString(),
        title,
        tasks: []
      }
      if (title.trim() !== '') {
        this.columns.push(newColumn)
        this.newColumnTitle = ''
        console.log('title', title)
      }
    },
    handleDeleteColumn (id) {
      this.columns = this.columns.filter(a => a.id !== id)
    },
    handleDeleteTask (taskID, columnID) {
      console.log(taskID, columnID)
      let columns = this.columns
      let coloumnDelete = columns.filter(a => a.id === columnID)[0]
      // remove task out old column
      let coloumnDeleteTasks = coloumnDelete.tasks.filter(a => a.id !== taskID)
      coloumnDelete.tasks = coloumnDeleteTasks
    },
    handleAddTask (task, callBack) {
      // console.log('add task', task)
      let columnID = task.columnID
      let columns = this.columns
      let column = columns.filter(a => a.id === columnID)[0]
      delete task.columnID
      column.tasks.push(task)
      callBack()
      // console.log('column', column)
    },
    handleDragTaskEnd (dataDragged) {
      // console.log('dataDragged', dataDragged)
      let columns = this.columns
      let coloumnFrom = columns.filter(a => a.id === dataDragged.from)[0]
      // task was dragged
      let taskDragged = coloumnFrom.tasks.filter(a => a.id === dataDragged.taskID)[0]
      // remove task out old column
      let coloumnFromTasks = coloumnFrom.tasks.filter(a => a.id !== dataDragged.taskID)
      coloumnFrom.tasks = coloumnFromTasks
      // add task into new column
      let columnTo = columns.filter(a => a.id === dataDragged.to)[0]
      // console.log(columnTo, taskDragged)
      columnTo.tasks.push(taskDragged)
    },
    handleClickEdit (taskID, columnID) {
      let task = this.columns.filter(a => a.id === columnID)[0].tasks.filter(b => b.id === taskID)[0]
      console.log(task)
      task.columnID = columnID
      this.showForm = true
      this.taskEdit = task
      // console.log(columnID)
    },
    handleCloseDialog () {
      this.showForm = false
    },
    handleSubmit (columnID, taskID, title, description) {
      console.log('handleSubmit', title)
      console.log('handleSubmit', description)
      let task = {
        id: taskID,
        title,
        description
      }
      let columnEditted = this.columns.filter(a => a.id === columnID)[0]
      let columnEdittedTasks = columnEditted.tasks
      columnEdittedTasks = columnEdittedTasks.filter(a => a.id !== taskID)
      columnEdittedTasks.push(task)
      columnEditted.tasks = columnEdittedTasks
      console.log('columnEditted', columnEdittedTasks)
      this.showForm = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../default";
  h1, h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  .container{
    position: relative;
    height: 100vh;
    background: #000;
  }
  .wrap{
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    overflow-x: auto;
    overflow-y: hidden;
    white-space: nowrap;
    /*padding-top: 36px;*/
  }
  .column{
    width: 350px;
    height: 100%;
    display: inline-flex;
    flex-direction: column;
    margin: 0 8px;
    padding: 8px;
    white-space: normal;
  }
  .dialog{
    position: fixed;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 99;
    background: rgba($color-black, 0.34);
    &_container{
      width: 70%;
      min-width: 320px;
      max-width: 596px;
      margin: 30px auto;
    }
  }
</style>
