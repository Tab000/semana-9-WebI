<template>
  <div class="container">
    <h1>Gestor de Tareas ✅</h1>

    <!-- Input con v-model -->
    <div class="input-box">
      <input
        v-model="newTask"
        placeholder="Escribe el nombre de la tarea"
        @keyup.enter="addTask"
      />
      <button @click="addTask">Agregar</button>
    </div>

    <hr />

    <div v-if="tasks.length > 0">
      <h2>Listado de tareas</h2>

      <div class="board">
        <!-- Columna To Do -->
        <div class="column todo">
          <h3>To Do</h3>
          <ul>
            <li v-for="(task, index) in tasks.filter(t => t.status === 'todo')" :key="index">
              {{ task.name }}
              <div>
                <button @click="moveForward(task)">➡️</button>
              </div>
            </li>
          </ul>
        </div>

        <!-- Columna Doing -->
        <div class="column doing">
          <h3>Doing</h3>
          <ul>
            <li v-for="(task, index) in tasks.filter(t => t.status === 'doing')" :key="index">
              {{ task.name }}
              <div>
                <button @click="moveBackward(task)">⬅️</button>
                <button @click="moveForward(task)">➡️</button>
              </div>
            </li>
          </ul>
        </div>

        <!-- Columna Done -->
        <div class="column done">
          <h3>Done</h3>
          <ul>
            <li v-for="(task, index) in tasks.filter(t => t.status === 'done')" :key="index">
              {{ task.name }}
              <div>
                <button @click="moveBackward(task)">⬅️</button>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div v-else>
      <p>No hay ninguna tarea registrada</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue"

const newTask = ref("")
const tasks = ref([])

const addTask = () => {
  const name = newTask.value.trim()
  if (name !== "") {
    tasks.value.push({ name, status: "todo" }) // todas empiezan en To Do
    newTask.value = ""
  }
}

// Mover tarea hacia adelante
const moveForward = (task) => {
  if (task.status === "todo") task.status = "doing"
  else if (task.status === "doing") task.status = "done"
}

// Mover tarea hacia atrás
const moveBackward = (task) => {
  if (task.status === "done") task.status = "doing"
  else if (task.status === "doing") task.status = "todo"
}
</script>

<style>
.container {
  max-width: 900px;
  margin: 40px auto;
  font-family: sans-serif;
  text-align: center;
}

.input-box {
  margin-bottom: 20px;
}

input {
  padding: 8px;
  width: 60%;
  font-size: 16px;
}

button {
  margin-left: 5px;
  padding: 6px 10px;
  font-size: 14px;
  cursor: pointer;
}

.board {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.column {
  flex: 1;
  margin: 0 10px;
  padding: 10px;
  border: 2px dashed #aaa;
  border-radius: 6px;
  min-height: 200px;
}

.todo {
  border-color: blue;
}

.doing {
  border-color: green;
}

.done {
  border-color: red;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  padding: 6px 10px;
  border-radius: 4px;
  background: white;
  color: black;
  border: 1px solid #ccc;
}
</style>
