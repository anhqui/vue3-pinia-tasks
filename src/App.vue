<template>
<main>
  <!-- heading -->
  <header>
    <img src="./assets/pinia-logo.svg" alt="pinia log">
    <h1>Todo List</h1>
  </header>

  <!-- new task form -->

  <div class="new-task-form">
    <TaskForm />

  </div>

  <!-- filter -->
  <nav class="filter">
    <button @click="filter = 'all'">All tasks</button>
    <button @click="filter = 'favs'">Fav tasks</button>
  </nav>

  <!-- loading -->
  <div class="loading" v-if="loading">Loading tasks...</div>

  <!-- task list -->
  <div class="task-list" v-if="filter === 'all'">
    <p>You have {{totalCount}} tasks left to do</p>
    <div v-for="task in tasks" :key="task.id">
      <TaskDetails :task="task" />
     </div>
  </div>
  
    <div class="task-list" v-if="filter === 'favs'">
       <p>You have {{favCount}} fav tasks left to do</p>
    <div v-for="task in favs" :key="task.id">
      <TaskDetails :task="task" />
     </div>
  </div>

  <button @click="taskStore.$reset">reset state</button>

</main>
</template>

<script>
import {ref} from 'vue'
import {storeToRefs} from 'pinia' 
import TaskDetails from './components/TaskDetails.vue'
import TaskForm from './components/TaskForm.vue'
import {useTaskStore} from './stores/TaskStore'
export default{
  components: {
TaskDetails,
TaskForm
  },
  setup(){
    const taskStore = useTaskStore()

    const {tasks, loading, favs, totalCount, favCount} = storeToRefs(taskStore)

    const filter = ref('all')
    // fetch tasks
    taskStore.getTasks()

    return {
      taskStore,
      filter,
      tasks, 
      loading, 
      favs, 
      totalCount, 
      favCount
    }

  }

}
</script>


