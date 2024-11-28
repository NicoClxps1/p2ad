<template>
  <div class="container">
    <header>
      <h1>PR UD 2</h1>
    </header>

    <main>
      <!-- Panel izquierdo -->
      <aside class="sidebar">
        <button 
          class="sidebar-button" 
          v-for="option in options" 
          :key="option"
          @click="selectOption(option)"
          :class="{ active: selectedOption === option }"
        >
          {{ option }}
        </button>
      </aside>

      <!-- Panel central -->
      <section class="main-content">
        <div class="actions">
          <button @click="getFiles">Get Files</button>
          <button @click="storeFile">Store</button>
          <button @click="showFile">Show</button>
          <button @click="updateFile">Update</button>
          <button @click="deleteFile">Delete</button>
        </div>

        <div class="response">
          <pre>{{ response }}</pre>
        </div>

        <button class="send-button" @click="sendData">Send</button>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Opciones del panel izquierdo
const options = ref(['Class Storage', 'JSON', 'CSV', 'XML']);
const selectedOption = ref('Class Storage');

// Datos simulados para mostrar la respuesta
const response = ref({
  mensaje: 'Listado de ficheros',
  contenido: ['.gitignore'],
});

// Métodos de ejemplo
const selectOption = (option) => {
  selectedOption.value = option;
};

const getFiles = () => {
  response.value = { mensaje: 'Listado de ficheros', contenido: ['file1.txt', 'file2.csv'] };
};

const storeFile = () => {
  response.value = { mensaje: 'Archivo guardado exitosamente' };
};

const showFile = () => {
  response.value = { mensaje: 'Detalles del archivo', contenido: 'Contenido del archivo mostrado' };
};

const updateFile = () => {
  response.value = { mensaje: 'Archivo actualizado correctamente' };
};

const deleteFile = () => {
  response.value = { mensaje: 'Archivo eliminado correctamente' };
};

const sendData = () => {
  response.value = { mensaje: 'Datos enviados correctamente' };
};
</script>

<style scoped>
/* Contenedor principal */
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  font-family: Arial, sans-serif;
  height: 100vh; /* Ocupa toda la altura de la página */
  box-sizing: border-box;
}

/* Encabezado */
header {
  text-align: center;
  margin-bottom: 20px;
}

/* Contenedor principal */
main {
  display: flex;
  width: 100%;
  height: 100%; /* Asegura que ocupe el resto de la pantalla */
  max-width: 1200px; /* Ancho máximo */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border: 1px solid #ddd;
  background-color: #fff;
}

/* Sidebar (panel izquierdo) */
.sidebar {
  flex: 1;
  display: flex;
  flex-direction: column;
  border-right: 1px solid #ddd;
  padding: 10px;
  background-color: #f8f8f8;
}

.sidebar-button {
  padding: 15px;
  margin-bottom: 15px;
  font-size: 18px; /* Botones más grandes */
  background-color: #f0f0f0;
  border: 1px solid #ddd;
  cursor: pointer;
  text-align: left;
}

.sidebar-button.active {
  background-color: #007bff;
  color: white;
}

/* Contenido principal */
.main-content {
  flex: 3;
  padding: 30px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.actions {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.actions button {
  padding: 15px 20px;
  font-size: 16px; /* Tamaño de texto más grande */
  border: 1px solid #ddd;
  background-color: #f8f8f8;
  cursor: pointer;
}

.actions button:hover {
  background-color: #007bff;
  color: white;
}

/* Sección de respuesta */
.response {
  flex-grow: 1;
  border: 1px solid #ddd;
  background-color: #f9f9f9;
  padding: 15px;
  font-size: 16px;
  overflow: auto;
}

pre {
  margin: 0;
}

/* Botón enviar */
.send-button {
  padding: 15px 30px;
  border: none;
  background-color: #28a745;
  color: white;
  font-size: 16px;
  cursor: pointer;
  align-self: flex-end; /* Botón alineado a la derecha */
}

.send-button:hover {
  background-color: #218838;
}
</style>
