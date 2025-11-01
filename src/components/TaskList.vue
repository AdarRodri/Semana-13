<template>
  <div class="page-wrapper">
    <div class="task-container">
      <h1>🗒️ Gestor de Tareas</h1>

      <div class="form">
        <input v-model="newTitle" placeholder="Título de la tarea" />
        <input v-model="newDesc" placeholder="Descripción" />
        <select v-model="newStatus">
          <option value="pendiente">Pendiente</option>
          <option value="doing">En progreso</option>
          <option value="done">Completada</option>
        </select>
        <button @click="addTask">Agregar</button>
      </div>

      <div class="filter">
        <label>Filtrar por estado:</label>
        <select v-model="filter">
          <option value="todas">Todas</option>
          <option value="pendiente">Pendientes</option>
          <option value="doing">En progreso</option>
          <option value="done">Completadas</option>
        </select>
      </div>

      <div v-if="filteredTasks.length" class="task-list">
        <div
          v-for="(task, index) in filteredTasks"
          :key="index"
          class="task-card"
          :class="task.status"
        >
          <div class="task-header">
            <h3>{{ task.title }}</h3>
            <span class="status">{{ estadoNombre(task.status) }}</span>
          </div>
          <p class="desc">{{ task.desc }}</p>
          <div class="actions">
            <button v-if="task.status !== 'pendiente'" @click="moveBackward(task)">⬅️ Atrás</button>
            <button v-if="task.status !== 'done'" @click="moveForward(task)">➡️ Avanzar</button>
            <button class="delete" @click="deleteTask(index)">🗑️ Eliminar</button>
          </div>
        </div>
      </div>

      <p v-else class="empty">No hay tareas en esta categoría 📭</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const tasks = ref([]);
const newTitle = ref("");
const newDesc = ref("");
const newStatus = ref("pendiente");
const filter = ref("todas");

const addTask = () => {
  if (!newTitle.value.trim()) return alert("Escribe un título para la tarea");
  tasks.value.push({
    title: newTitle.value.trim(),
    desc: newDesc.value.trim(),
    status: newStatus.value,
  });
  newTitle.value = "";
  newDesc.value = "";
  newStatus.value = "pendiente";
};

const filteredTasks = computed(() => {
  if (filter.value === "todas") return tasks.value;
  return tasks.value.filter((task) => task.status === filter.value);
});

const moveForward = (task) => {
  if (task.status === "pendiente") task.status = "doing";
  else if (task.status === "doing") task.status = "done";
};

const moveBackward = (task) => {
  if (task.status === "done") task.status = "doing";
  else if (task.status === "doing") task.status = "pendiente";
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

const estadoNombre = (status) => {
  if (status === "pendiente") return "Pendiente";
  if (status === "doing") return "En progreso";
  if (status === "done") return "Completada";
};
</script>

<style scoped>
/* Fondo principal con degradado suave */
.page-wrapper {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #74ebd5 0%, #acb6e5 100%);
  padding: 20px;
}

/* Tarjeta flotante tipo glassmorphism */
.task-container {
  width: 100%;
  max-width: 750px;
  background: rgba(255, 255, 255, 0.25);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-radius: 20px;
  padding: 35px 40px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.25);
  border: 1px solid rgba(255, 255, 255, 0.4);
}

/* Título */
h1 {
  text-align: center;
  color: #fff;
  margin-bottom: 25px;
  font-size: 2rem;
  font-weight: bold;
  text-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

/* Formulario */
.form {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  margin-bottom: 25px;
}
input,
select {
  padding: 10px;
  border-radius: 10px;
  border: 1px solid #ccc;
  min-width: 150px;
  background-color: rgba(255, 255, 255, 0.8);
  color: #333;
}
button {
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 10px 15px;
  cursor: pointer;
  transition: all 0.2s;
}
button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

/* Filtro */
.filter {
  text-align: center;
  margin-bottom: 25px;
}
.filter label {
  margin-right: 8px;
  font-weight: 500;
  color: #fff;
}

/* Lista de tareas */
.task-list {
  display: flex;
  flex-direction: column;
  gap: 18px;
}
.task-card {
  padding: 15px 20px;
  border-radius: 15px;
  background-color: rgba(255, 255, 255, 0.9);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.2s;
}
.task-card:hover {
  transform: translateY(-3px);
}
.task-card.pendiente {
  border-left: 6px solid #ffc107;
}
.task-card.doing {
  border-left: 6px solid #17a2b8;
}
.task-card.done {
  border-left: 6px solid #28a745;
}
.task-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.status {
  font-size: 0.9rem;
  color: #555;
  background: #f1f1f1;
  border-radius: 6px;
  padding: 4px 10px;
}
.desc {
  margin-top: 8px;
  color: #666;
}

/* Botones dentro de las tareas */
.actions {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}
.delete {
  background-color: #dc3545;
}
.delete:hover {
  background-color: #a71d2a;
}

/* Texto vacío */
.empty {
  text-align: center;
  color: #f8f9fa;
  font-style: italic;
  margin-top: 25px;
}
</style>
