<template>
  <li class="shop__left__filter">
    <div class="price__filter">
      <input
          type="range"
          min="0"
          max="180"
          step="1"
          v-model="localMinPrice"
          @input="updatePriceRange"
      />
    </div>
    <p class="filter__text">
      Price: ${{ localMinPrice }} - ${{ props.maxPrice }}
      <span>Filter</span>
    </p>
  </li>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';


const props = defineProps({
  minPrice: {
    type: Number,
    required: true,
  },
  maxPrice: {
    type: Number,
    required: true,
  },
});

const emit = defineEmits();

const localMinPrice = ref(props.minPrice);

const updatePriceRange = () => {
  emit('update:minPrice', localMinPrice.value);
};

</script>
<style scoped>
.price__filter input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  background: transparent;
  height: 4px;
}

.price__filter input[type="range"]::-webkit-slider-runnable-track {
  background: #000000;
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
.price-filter input[type="range"]::-webkit-slider-runnable-track {
  background: #000000;
  height: 2px;
  border-radius: 2px;
}


.price-filter input[type="range"]::-webkit-slider-thumb {
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
}
.filter__text span {
  color: #A18A68;
  font-family: "DM Sans", sans-serif;
  font-size: 14px;
  font-weight: 400;
}
.price-filter input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  background: transparent;
  height: 4px;
}

</style>