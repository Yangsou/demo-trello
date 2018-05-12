<template>
  <div class="container">
    <button>
      add
    </button>
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
        ></Column>
      </div>
    </div>
  </div>
</template>

<script>
import Column from './Column'

export default {
  components: {Column},
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
      ]
    }
  },
  methods: {
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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
</style>
