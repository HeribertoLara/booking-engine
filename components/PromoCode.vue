<template>
  <div class="promo-code-input">
    <label for="promo-code">Promo Code:</label>
    <input
      id="promo-code"
      v-model="promoCode"
      placeholder="Enter promo code"
      @input="updatePromoCode"
    />
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const promoCode = ref('');
const emit = defineEmits(['update:promoCode']);

// Emitir el cambio en el código promocional
const updatePromoCode = () => {
  emit('update:promoCode', promoCode.value);
};
</script>

<style scoped>
.promo-code-input {
  margin-bottom: 20px;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
