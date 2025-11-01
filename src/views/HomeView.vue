<template>
  <div class="home">
    <h1>Gestor de Tareas 📝</h1>
    <TaskList />
  </div>
</template>

<script setup>
import TaskList from '../components/TaskList.vue'
</script>

<style>
.home {
  max-width: 800px;
  margin: 40px auto;
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}
</style>
