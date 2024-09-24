<template>
  <div>
    <input
      type="text"
      ref="dateInput"
      placeholder="Selecciona un rango de fechas"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import flatpickr from 'flatpickr';
import { addDays } from 'date-fns';
import 'flatpickr/dist/flatpickr.min.css'; 

// Definir propiedades para recibir las fechas desde el componente padre
const props = defineProps({
  modelValueArrival: {
    type: Date,
    default: () => addDays(new Date(), 7), 
  },
  modelValueDeparture: {
    type: Date,
    default: () => addDays(new Date(), 12), 
  }
});

const emit = defineEmits(['update:modelValueArrival', 'update:modelValueDeparture']);
const dateInput = ref(null);

onMounted(() => {
  flatpickr(dateInput.value, {
    mode: 'range', // Permite seleccionar un rango de fechas
    dateFormat: 'Y-m-d', // Formato de la fecha en el input
    altFormat: 'd M, D', // Formato alternativo que se muestra en el input
    altInput: true, // Muestra el input con formato alternativo
    defaultDate: [props.modelValueArrival, props.modelValueDeparture], // Usar las fechas pasadas desde el padre
    showMonths: 2, // Mostrar dos meses uno al lado del otro
    onChange: (selectedDates) => {
      if (selectedDates.length === 2) {
        // Emitir las fechas seleccionadas al componente padre
        emit('update:modelValueArrival', selectedDates[0]);
        emit('update:modelValueDeparture', selectedDates[1]);
      }
    },
  });

  // Configurar el valor inicial del input con las fechas recibidas
  dateInput.value._flatpickr.setDate(
    [props.modelValueArrival, props.modelValueDeparture], 
    false // No dispara el evento onChange al establecer la fecha predeterminada
  );
});

// Si las fechas cambian en el componente padre, actualizar el flatpickr
watch(() => [props.modelValueArrival, props.modelValueDeparture], (newValues) => {
  dateInput.value._flatpickr.setDate(newValues, false);
});
</script>

<style scoped>
/* Aquí puedes agregar estilos personalizados si es necesario */
</style>
