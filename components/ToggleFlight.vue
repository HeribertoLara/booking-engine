<template>
  <fieldset class="toggle-switch">
    <legend class="toggle-switch__legend">Flight Option</legend>
    <div class="toggle-switch__control">
      <input
        type="checkbox"
        id="flight-toggle"
        :checked="modelValue"
        @change="emitToggle"
        aria-checked="modelValue"
        aria-labelledby="flight-toggle-label"
      />
      <label for="flight-toggle" class="toggle-switch__label">
        <span class="toggle-switch__slider"></span>
        <span id="flight-toggle-label" class="toggle-switch__text">
          {{ modelValue ? 'Flight Included' : 'No Flight' }}
        </span>
      </label>
    </div>
    <button v-if="modelValue" @click.prevent="changeAirport" class="airport-button">
      {{ airport.label ? airport.label : 'Select Airport' }}
    </button>
  </fieldset>
</template>

<script setup>


const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false,
  },
  airport: {
    type: Object,
    default: () => ({ label: '', value: '' }), // Asegúrate de recibir un objeto
  },
});

const emit = defineEmits(['update:modelValue', 'toggle-open-modal']);

const emitToggle = (event) => {
  const isChecked = event.target.checked;
  emit('update:modelValue', isChecked);

  if (isChecked) {
    emit('toggle-open-modal', true);
  }
};

const changeAirport = () => {
  emit('toggle-open-modal', true);
};
</script>

<!-- <style scoped lang="scss">
.toggle-switch {
  /* estilos aquí... */
}
</style> -->
