<template>
  <li class="shop__left__filter">
    <div class="price__filter">
      <input
          type="range"
          min="0"
          :max="props.maxPrice"
          step="1"
          v-model="localMinPrice"
          @input="updatePriceRange"
      />
      <input
          type="range"
          :min="localMinPrice"
          max="180"
          step="1"
          v-model="localMaxPrice"
          @input="updatePriceRange"
      />
    </div>
    <p class="filter__text">
      Price: ${{ localMinPrice }} - ${{ localMaxPrice }}
      <span>Filter</span>
    </p>
  </li>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps({
  minPrice: {
    type: Number,
  },
  maxPrice: {
    type: Number,
  },
});

const emit = defineEmits();

const localMinPrice = ref(props.minPrice);
const localMaxPrice = ref(props.maxPrice);

const updatePriceRange = () => {
  emit('update:priceRange', { minPrice: localMinPrice.value, maxPrice: localMaxPrice.value });
};
</script>

<style scoped>
.price__filter {
  position: relative;
  width: 100%;
}

.price__filter input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  background: transparent;
  height: 4px;
  position: absolute;
}

.price__filter input[type="range"]:nth-child(1) {
  z-index: 1;
}
.price__filter input[type="range"]:nth-child(2) {
  z-index: 2;
}

.price__filter input[type="range"]::-webkit-slider-runnable-track {
  background: #000000;;
  height: 2px;
  border-radius: 2px;
}

.price__filter input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 2px;
  height: 10px;
  background: #000000;
  cursor: pointer;
  margin-top: -4px;
}

.filter__text {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #707070;
  font-family: "DM Sans", sans-serif;
  font-size: 14px;
  padding-top: 8px;
}
.filter__text span {
  color: #A18A68;
  font-family: "DM Sans", sans-serif;
  font-size: 14px;
  font-weight: 400;
}
</style>