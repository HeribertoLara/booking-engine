<template>
  <div>
    <!-- Botón para abrir el modal -->
    <button @click.prevent="showModal">
      {{ localAdults }} Adults, {{ localChildren }} Children
    </button>
    
    <!-- Modal -->
    <div v-if="modalVisible" class="modal-overlay">
      <div class="modal-content">
        <h3>Select Guests</h3>
        
        <div class="counter">
          <label>Adults:</label>
          <button @click.prevent="decrementAdults" :disabled="localAdults <= 1">-</button>
          <span>{{ localAdults }}</span>
          <button @click.prevent="incrementAdults">+</button>
        </div>
        
        <div class="counter">
          <label>Children:</label>
          <button @click.prevent="decrementChildren" :disabled="localChildren <= 0">-</button>
          <span>{{ localChildren }}</span>
          <button @click.prevent="incrementChildren">+</button>
        </div>

        <!-- Inputs para edades de niños -->
        <div v-for="(age, index) in localChildren" :key="index" class="child-age-selector">
          <select v-model="childAges[index]" @change="updateChildrenAges">
            <option value="" disabled>Select age</option>
            <option v-for="ageOption in ages" :key="ageOption" :value="ageOption">{{ ageOption }}</option>
          </select>
          <label v-if="!childAges[index]">Children age</label>
        </div>
        
        <button @click.prevent="confirmSelection">Confirm</button>
        <button @click="closeModal">Close</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Definir las props
const props = defineProps({
  adults: {
    type: Number,
    required: true,
  },
  numberChildren: {
    type: Number,
    required: true,
  },
  childrenAges: {
    type: Array,
    default: () => [], // Si es necesario manejar edades de los niños
  },
});

// Definir emit para enviar eventos al componente padre
const emit = defineEmits(['update:adults', 'update:numberChildren', 'update:childrenAges']);

// Estado para el modal
const modalVisible = ref(false);
const localAdults = ref(props.adults); // Variable local para adultos
const localChildren = ref(props.numberChildren); // Variable local para niños
const childAges = ref([...props.childrenAges]); // Array para edades de niños
const ages = Array.from({ length: 17 }, (_, i) => i); // Edades de 0 a 16

// Mostrar modal
const showModal = () => {
  // Sincronizar los valores locales con las props
  localAdults.value = props.adults;
  localChildren.value = props.numberChildren;
  
  // Inicializar o actualizar childAges según el número de niños
  if (childAges.value.length !== localChildren.value) {
    childAges.value = Array(localChildren.value).fill(''); // Inicializar con valores vacíos
  }

  modalVisible.value = true;
};

// Cerrar modal
const closeModal = () => {
  modalVisible.value = false;
};

// Incrementar y decrementar adultos
const incrementAdults = () => {
  localAdults.value++;
};

const decrementAdults = () => {
  if (localAdults.value > 1) localAdults.value--;
};

// Incrementar y decrementar niños
const incrementChildren = () => {
  localChildren.value++;
  childAges.value.push(''); // Agregar un nuevo valor vacío para la nueva edad
};

const decrementChildren = () => {
  if (localChildren.value > 0) {
    localChildren.value--;
    childAges.value.pop(); // Eliminar el último valor
  }
};

// Actualizar edades de niños
const updateChildrenAges = () => {
  // Lógica adicional para manejar cambios en las edades
};

// Confirmar selección
const confirmSelection = () => {
  // Emitir los valores actualizados a las props
  emit('update:adults', localAdults.value);
  emit('update:numberChildren', localChildren.value);
  emit('update:childrenAges', childAges.value); // Emitir las edades de los niños
  closeModal();
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.counter {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 10px 0;
}

.child-age-selector {
  margin: 10px 0;
}

button {
  margin: 0 5px;
}
</style>
