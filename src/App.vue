<template>
  <div class="page-wrapper">
    <div class="content-wrapper">
      <TaskList />
    </div>
  </div>
</template>

<script setup>
import TaskList from './components/TaskList.vue'
</script>

<style>
.page-wrapper {
  min-height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;  /* centra horizontalmente */
  align-items: center;      /* centra verticalmente */
  background: linear-gradient(135deg, #74ebd5 0%, #acb6e5 100%);
}

/* Este div controla el ancho del contenido */
.content-wrapper {
  width: 100%;
  max-width: 900px; /* puedes ajustar este valor */
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
  padding: 40px;
  display: flex;
  justify-content: center;
}
</style>
