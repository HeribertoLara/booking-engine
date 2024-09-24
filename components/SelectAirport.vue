<template>
  <div v-if="isOpenModal" class="modal">
    <fieldset>
      <legend>Select Airport</legend>

      <label for="airport-input">Select an airport:</label>
      <input
        id="airport-input"
        v-model="inputValue"
        @input="fetchAirports"
        placeholder="Type to search..."
      />

      <ul v-if="filteredAirports.length > 0" class="airport-list">
        <li
          v-for="airport in filteredAirports"
          :key="airport.value"
          @click="selectAirport(airport)"
          class="airport-item"
        >
          {{ airport.label }}
        </li>
      </ul>

      <!-- Botón para cerrar el modal y cambiar isOpenModal a false -->
      <button @click.prevent="disableFlightInclusion">Close</button>
    </fieldset>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

// Definir propiedades
const props = defineProps({
  isOpenModal: {
    type: Boolean,
    required: true,
  },
  airport: {
    type: Object,
    default: () => ({ label: '', value: '' }), // Cambiado a objeto
  },
});

// Emitir eventos para actualizar los valores
const emit = defineEmits(['update:isOpenModal', 'update:airport']);

const airports = ref([]);
const filteredAirports = ref([]);
const inputValue = ref('');

// Función para buscar aeropuertos
const fetchAirports = async () => {
  if (inputValue.value) {
    try {
      const response = await fetch(
        `https://www.reservhotel.com/win/owa/ibe5.get_airport_json?p_search=${inputValue.value}`
      );
      const data = await response.json();
      airports.value = data; // Asignar la respuesta al array de aeropuertos
      filterAirports(); // Filtrar las opciones
    } catch (error) {
      console.error('Error fetching airports:', error);
    }
  } else {
    filteredAirports.value = []; // Limpiar opciones si no hay entrada
  }
};

// Función para filtrar aeropuertos
const filterAirports = () => {
  filteredAirports.value = airports.value.filter((airport) =>
    airport.label.toLowerCase().includes(inputValue.value.toLowerCase())
  );
};

// Selecciona un aeropuerto y cierra el modal
const selectAirport = (airport) => {
  emit('update:airport', airport); // Emitir el objeto completo
  inputValue.value = airport.label; // Actualizar inputValue con el label del aeropuerto seleccionado
  cleanInput();
  closeAirportModal();
};

// Limpiar el input después de seleccionar
const cleanInput = () => {
  inputValue.value = '';
};

// Cambia isOpenModal a false y cierra el modal al presionar "Close"
const disableFlightInclusion = () => {
  emit('update:isOpenModal', false); // Asegura que esta propiedad controla el modal
  closeAirportModal();
};

// Función para cerrar el modal sin afectar isOpenModal
const closeAirportModal = () => {
  inputValue.value = ''; // Limpiar el input
};

// Observa cambios en el aeropuerto para sincronizar
watch(
  () => props.airport,
  (newAirport) => {
    inputValue.value = newAirport.label; // Sincronizar inputValue con el label del aeropuerto seleccionado
  }
);
</script>

<!-- <style scoped>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background: white;
  border: 1px solid #ccc;
  z-index: 1000;
}

input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.airport-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  border: 1px solid #ccc;
  max-height: 200px;
  overflow-y: auto;
  background: white;
}

.airport-item {
  padding: 10px;
  cursor: pointer;
}

.airport-item:hover {
  background-color: #f0f0f0;
}
</style>
 -->