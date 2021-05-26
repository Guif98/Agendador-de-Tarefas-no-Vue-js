<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Lista de Tarefas" :showAddTask="showAddTask"/>
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks @toggle-reminder="toggleReminder" 
    @delete-task="deleteTask" :tasks="tasks" />
  </div>
</template>

<script>
  import Header from './components/Header'
  import Tasks from './components/Tasks'
  import AddTask from './components/AddTask'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      showAddTask: false,
    }
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id) {
      if (confirm('Tem certeza que deseja remover essa tarefa?')) {
      const res = await fetch(`api/tasks/${id}`, {
        method: 'DELETE',

        })
        
        res.status === 200 ? this.tasks = this.tasks.filter((task) => task.id !== id)
        : alert('Erro ao deletar')
      }
    },
    
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json()

      

      this.tasks = this.tasks.map((task) => task.id === id
      ? {...task, reminder: data.reminder} : task) 
    },
    async fetchTasks() {
      const res = await fetch('http://localhost:3000/tasks')

      const data = await res.json()

      return data
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)

      const data = await res.json()

      return data
    },
  },
   async created() {
      this.tasks = await this.fetchTasks()
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.btn {
  display: inline-block;
  background: rgb(18, 131, 56);
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

.container {
  width: 700px;
  height: auto;
  padding: 10px 20px;
  margin: 0 auto;
  border-radius: 20px;
  background: rgb(236, 192, 111);
}


</style>
