<template>
  <fieldset class="select-hotel">
    <legend class="select-hotel__legend">Select Hotel</legend>
    <label for="hotel-select" class="select-hotel__label">Hotel:</label>
    <select
      id="hotel-select"
      class="select-hotel__select"
      :value="modelValue"
      @change="handleHotelChange"
    >
      <option value="" disabled>Select a hotel</option>
      <option
        class="select-hotel__option"
        v-for="hotel in hotels"
        :key="hotel.value"
        :value="hotel.value"
      >
        {{ hotel.label }}
      </option>
    </select>
  </fieldset>
</template>

<script setup>


const props = defineProps({
  hotels: {
    type: Array,
    default: () => [],
  },
  modelValue: {
    type: String,
    default: '',
  },
  isFlightIncluded: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(['update:modelValue']);

const handleHotelChange = (event) => {
  emit('update:modelValue', event.target.value);
};
</script>

<!-- <style scoped lang="scss">
/* Estilos específicos para SelectHotel */
.select-hotel {
  border: 1px solid #181717;
  padding: 10px;
  margin: 10px 0;
  border-radius: 4px;
  max-width: 300px;
  background-color: #f9f9f9;

  &__legend {
    font-weight: bold;
    padding: 0 5px;
    color: #333;
  }

  &__label {
    display: block;
    margin-bottom: 5px;
    font-size: 14px;
    color: #181717;
  }

  &__select {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    font-size: 14px;
    color: #181717;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 4px;
    appearance: none;
    outline: none;
    transition: border-color 0.3s ease;

    &:focus {
      border-color: #4caf50;
    }
  }

  &__option {
    color: #333;
    background-color: #fff;
  }
}
</style>
<!--  revisa si  hotel.name esta llegando al componente option --> -->