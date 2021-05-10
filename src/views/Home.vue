<template>
  <AddTask 
    v-show="showAddTask" 
    @add-task="addTask" 
  />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async toggleReminder(id) {
      const result = this.tasks.filter((task) => task.id === id);
      let reminder = result[0].reminder; // result is a Proxy object
      if (!confirm(`Change reminder from ${reminder} to ${!reminder}?`)) return;

      const taskToUpdate = await this.fetchTask(id);
      const currentStatus = taskToUpdate.reminder;
      const updatedTask = { ...taskToUpdate, reminder: !currentStatus };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updatedTask),
      });

      const status = await res.status;

      if (status === 200) {
        const result = await res.json();

        console.log(`new reminder state: ${result.reminder}`);
        const newState = result.reminder;

        this.tasks = this.tasks.map((task) => {
          // this makes each array element to a new object with properties
          // {...task}

          // you can update or add a new property to the object like this
          // {...task, reminder: !task.reminder, text: 'sdaadasd', h: 'adas'}

          return task.id === id ? { ...task, reminder: newState } : task;
        });
      }
    },
    async deleteTask(id) {
      //console.log(id)

      if (!confirm("OK to delete?")) return;

      const res = await fetch(`api/tasks/${id}`, {
        method: "DELETE",
      });
      // const data = await res.json()

      if (res.status === 200) {
        console.log(`deleted id: ${id}`);
        // return a new array with requested task id removed
        this.tasks = this.tasks.filter((task) => {
          // loops through all items in this.tasks '(task) =>' is individual array element to check
          console.log(
            `deleting task id:${id}; checking existing task id: ${task.id}`
          );
          return task.id !== id;
        });
        console.log(this.tasks);
      }
    },
    async addTask(newTask) {
      console.log(`new task:  ${JSON.stringify(newTask)}`);

      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newTask),
      });

      const data = await res.json();
      console.log(`return data: ${JSON.stringify(data)}`);

      this.tasks = [...this.tasks, data];

      //this.tasks = [...this.tasks, newTask]
    },

    async fetchTasks() {
      let res = await fetch("api/tasks"); // see vue.config.js for dev api path -- proxy changes the api address and port
      // let res = await fetch('http://localhost:5000/tasks')
      let data = res.json();
      return data;
    },
    async fetchTask(id) {
      let res = await fetch(`api/tasks/${id}`); // see vue.config.js for dev api path -- proxy changes the api address and port
      // let res = await fetch('http://localhost:5000/tasks')
      let data = res.json();
      return data;
    },
  },
  async created() {
    let mytasks = await this.fetchTasks();
    console.log(mytasks);
    this.tasks = mytasks;
  },
};
</script>
