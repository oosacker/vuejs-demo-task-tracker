<template>
  <div class='container'>  
    <Header 
      title='My Tasks' 
      @toggle-add-task="toggleAddTask"
      :showAddTask="showAddTask"
    />

    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    
    <Tasks 
      @toggle-reminder="toggleReminder" 
      @delete-task="deleteTask" 
      :tasks='tasks' 
    />
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header'
import Tasks from './components/Tasks'
import AddTask from './components/AddTask'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
    Footer
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    }
  },
  methods: {
    async toggleReminder(id) {

      const result = this.tasks.filter( task => task.id === id )
      let reminder = (result[0].reminder) // result is a Proxy object
      if(!confirm(`Change reminder from ${reminder} to ${!reminder}?`))
        return

      const taskToUpdate = await this.fetchTask(id)
      const currentStatus = taskToUpdate.reminder
      const updatedTask = {...taskToUpdate, reminder:!currentStatus}
      
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updatedTask),
      })

      const status = await res.status

      if (status === 200) {

        const result = await res.json()

        console.log(`new reminder state: ${result.reminder}`)
        const newState = result.reminder

        this.tasks = this.tasks.map((task) => { 

          // this makes each array element to a new object with properties
          // {...task} 

          // you can update or add a new property to the object like this
          // {...task, reminder: !task.reminder, text: 'sdaadasd', h: 'adas'}

          return task.id === id ? {...task, reminder: newState} : task 
        })
      }

    },
    async deleteTask(id) {
      //console.log(id)

      if(!confirm('OK to delete?'))
        return

      const res = await fetch(`api/tasks/${id}`, {
        method: 'DELETE'
      })
      // const data = await res.json()
      

      if (res.status === 200) {
        console.log(`deleted id: ${id}`)
        // return a new array with requested task id removed
        this.tasks = this.tasks.filter((task) => {
          // loops through all items in this.tasks '(task) =>' is individual array element to check
          console.log(`deleting task id:${id}; checking existing task id: ${task.id}`)
          return task.id !== id
        })
        console.log(this.tasks)
      }

    },
    async addTask(newTask) {
      
      console.log(`new task:  ${JSON.stringify(newTask)}`)

      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newTask),
      })

      const data = await res.json()
      console.log(`return data: ${JSON.stringify(data)}`)

      this.tasks = [...this.tasks, data]

      //this.tasks = [...this.tasks, newTask]
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
      console.log('show add task='+this.showAddTask)
    },
    async fetchTasks() {
      let res = await fetch('api/tasks')  // see vue.config.js for dev api path -- proxy changes the api address and port
      // let res = await fetch('http://localhost:5000/tasks')  
      let data = res.json()
      return data;
    },
    async fetchTask(id) {
      let res = await fetch(`api/tasks/${id}`)  // see vue.config.js for dev api path -- proxy changes the api address and port
      // let res = await fetch('http://localhost:5000/tasks')  
      let data = res.json()
      return data;
    }
  },
  async created() {
    let mytasks = await this.fetchTasks()
    console.log(mytasks)
    this.tasks = mytasks
    //console.log(this.fetchTasks())
    // [
    //   { 
    //     id: 1,
    //     text: 'Doctor',
    //     date: 'March 1 5:30PM',
    //     reminder: true,
    //   },
    //   {
    //     id: 2,
    //     text: 'Meeting at school',
    //     date: 'March 3 1:30PM',
    //     reminder: false,
    //   },
    //   {
    //     id: 3,
    //     text: 'Shopping',
    //     date: 'March 3 4:30PM',
    //     reminder: true,
    //   },
    // ]
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
