<script setup lang="ts">
import {ref} from 'vue'


const newTaskText = ref('')

const tasks = ref([
  { id:1, text: 'Task 1', completed: false },
  { id:2, text: 'Task 2', completed: true },
])
const addTask = () =>{
  if(newTaskText.value.trim()==='') {return}



tasks.value.push({
    id: Date.now(),
    text: newTaskText.value.trim(),
    completed: false

})

newTaskText.value = ''

}

const toggleTask = (task: { completed: boolean }) => {
  task.completed = !task.completed
}

const deleteTask = (task: { id: number }) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

</script>

<template>
  
  <h1 class="title">TO DO LIST </h1>

    <div class="input-container">
      <input 
        v-model="newTaskText" 
        @keyup.enter="addTask"
        type="text" 
        placeholder="Unesite novi zadatak..."
      />
      <button @click="addTask">Dodaj</button>
    </div>

    <p class="counter">
      Ukupno zadataka: <strong>{{ tasks.length }}</strong>
    </p>


    <Transition name="fade">
  <p  v-if="tasks.length === 0" class="empty-state">
    Nema unesenih zadataka. Dodaj prvi zadatak iznad!
  </p>


    <ul v-else class="task-list">
      <li 
        v-for="task in tasks" 
        :key="task.id" 
        :class="{ completed: task.completed }"
      >
        <span @click="toggleTask(task)" class="task-title">
          {{ task.text }}
        </span>

        <div class="actions">
          <button @click="toggleTask(task)">
            {{ task.completed ? 'Vrati' : 'Završi' }}
          </button>
          <button @click="deleteTask(task)" class="delete-btn">
            Obriši
          </button>
        </div>
      </li>
    </ul>
    </Transition>
</template>

<style scoped>
.template{
  display: flex;
  align-items: center;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
.empty-state{
  text-align: center;
  color: #94a3b8; 
  font-style: italic; 
  margin-top: 20px; 
}
.title {
  text-align: center;
  margin-bottom: 20px;
}
.completed .task-title {
  text-decoration: line-through;
  color: #888;
}
.counter{
  text-align: center;
  margin-bottom: 20px;
}
.delete-btn {
  background-color: #ff4d4d;
  color: white;
  border: none;
  padding: 4px 8px;
  cursor: pointer;
  margin-left: 5px;
}

.input-container {
  display: flex;
  align-items: center;
  justify-content: center;
  align-items: center;
  margin-bottom: 15px;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  border-bottom: 1px solid #ddd;
}

.task-title {
  cursor: pointer;
}
.empty-message {
  text-align: center; 
  color: #94a3b8;      
  font-style: italic; 
  margin-top: 20px;   
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

</style>
