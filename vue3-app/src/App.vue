<template>
  <div class="app">
    <header>
      <img src="/vite.svg" alt="Vite Logo" class="logo" />
      <h1>{{ message }}</h1>
    </header>
    <main>
      <div class="card">
        <button @click="count++">Contador: {{ count }}</button>
        <p>Edita <code>src/App.vue</code> y guarda para ver los cambios en tiempo real.</p>
      </div>
      <p class="info">
        Aplicación Vue 3 + Vite dockerizada
      </p>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('¡Hola Vue 3!')
const count = ref(0)
</script>

<style scoped>
.app {
  text-align: center;
  padding: 2rem;
}

header {
  margin-bottom: 2rem;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

h1 {
  font-size: 3.2em;
  line-height: 1.1;
  color: #42b883;
}

.card {
  padding: 2em;
}

button {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #42b883;
  color: white;
  cursor: pointer;
  transition: border-color 0.25s;
}

button:hover {
  border-color: #42b883;
}

button:focus,
button:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}

.info {
  color: #888;
}
</style>
