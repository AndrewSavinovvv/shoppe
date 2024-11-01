<template>
  <select class="option" @change="handleSelect($event.target.value)">
    <option  disabled selected value> {{propNames}}  </option>
    <option  v-for="option in options" :key="option.value" :value="option.value">{{ option.label }}</option>
  </select>

</template>
<script setup>
import { defineProps, defineEmits } from 'vue';

defineProps({
  options: {
    type: Array,
  },
  propNames: {
    type: String,
  }
})
const emit = defineEmits();
const handleSelect = (value) => {
  emit('select', value);
};
</script>

<style scoped>
.option {
  display: flex;
  width: 100%;
  padding: 16px 12px;
  color: #000000;
  border: 1px solid #D8D8D8;
  font-size: 14px;
  font-family: DM Sans, sans-serif;
  margin-bottom: 16px;
  border-radius: 5px;
  background-image: url("@/assets/icon/arrow-down.svg");
  background-repeat: no-repeat;
  background-position: right 10px center;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

</style>