<template>
  <div class='container'>  
    <Header title='My Tasks' />
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks='tasks' />
  </div>
</template>

<script>
import Header from './components/Header'
import Tasks from './components/Tasks'

export default {
  name: 'App',
  components: {
    Header,
    Tasks
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    toggleReminder(id) {
      if(!confirm('Change state?'))
        return

      this.tasks = this.tasks.map((task) => 
        task.id === id ? {...task, reminder: !task.reminder}: task
      )

    },
    deleteTask(id) {
      //console.log(id)

      if(!confirm('OK to delete?'))
        return

      // return a new array with requested task id removed
      this.tasks = this.tasks.filter((task) => {
        // loops through all items in this.tasks '(task) =>' is individual array element to check
        console.log(`deleting task id:${id}; checking existing task id: ${task.id}`)
        return task.id !== id
      })
    }
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: 'Doctor',
        date: 'March 1 5:30PM',
        reminder: true,
      },
      {
        id: 2,
        text: 'Meeting at school',
        date: 'March 3 1:30PM',
        reminder: false,
      },
      {
        id: 3,
        text: 'Shopping',
        date: 'March 3 4:30PM',
        reminder: true,
      },
    ]
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
